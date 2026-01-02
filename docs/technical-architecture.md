# KrakowBites - Technical Architecture Specification

**Date:** 2026-01-01
**Version:** 2.0 (Updated for self-hosted infrastructure)
**Status:** Design phase
**Timeline:** 2-3 months implementation

---

## Technology Stack

### Frontend

**Framework:** Astro 4.x + TypeScript
- **Why:** Zero JavaScript by default, ships pure HTML/CSS for static content
- TypeScript 5.7+ native support
- Islands architecture for interactive components
- SSR (Server-Side Rendering) with Node.js adapter
- Component-agnostic (can use React, Svelte, or vanilla JS for islands)

**Styling:**
- Tailwind CSS 4.x
- Vanilla CSS for critical styles
- Custom design system for KrakowBites brand
- No runtime CSS-in-JS (pure static CSS)

**Interactive Islands (Minimal JS):**
- React (only for complex components: booking calendar, payment form)
- Alpine.js or vanilla JS for simple interactions (dropdowns, toggles)
- Total JS shipped: ~5-10kb for most pages (vs 80kb+ in Next.js)

**Forms & Validation:**
- Native HTML5 validation where possible
- Zod schema validation (server-side)
- Progressive enhancement (works without JS)

**Maps:**
- Leaflet.js (open-source, no API costs)
- OpenStreetMap tiles
- Meeting point markers on tour pages

---

### Backend

**Runtime:** Node.js 20+ LTS
- Self-hosted on VPS (Apache2 reverse proxy)
- PM2 or systemd for process management
- Not serverless - long-running process

**API Layer:**
- Astro API endpoints (`src/pages/api/*.ts`)
- RESTful design for CRUD operations
- TypeScript contracts for type safety

**Database:** PostgreSQL 16+
- **Provider:** Self-hosted on VPS
- **Connection:** Direct via `pg` library or Drizzle ORM
- **Migrations:** Custom SQL scripts or Drizzle Kit
- **No external dependencies** (no Supabase, no managed DB)

**ORM Options:**
1. **Drizzle ORM** (Recommended)
   - Lightweight, TypeScript-first
   - SQL-like syntax, closer to raw queries
   - Excellent for self-hosted PostgreSQL

2. **Raw SQL with `pg`**
   - Full control, no abstraction
   - Manual type definitions
   - Lighter weight

3. **Prisma** (Optional)
   - Type-safe, mature
   - Heavier runtime overhead
   - Use if you prefer schema-first approach

**Authentication:**
- Custom JWT-based auth or Lucia Auth
- Session stored in PostgreSQL
- HTTP-only cookies
- Admin dashboard only (single user initially)

---

### Self-Hosted Infrastructure

**Web Server:** Apache2
- Reverse proxy to Node.js (port 3000)
- Serves static assets directly (`/assets/`, `/_astro/`)
- SSL/TLS via Let's Encrypt (certbot)
- HTTP/2 enabled
- Gzip/Brotli compression

**Process Management:** systemd
- Auto-restart on failure
- Logs to journald
- Environment variables from file
- Graceful shutdown/reload

**Email:** Your existing SMTP cluster
- Nodemailer with custom SMTP config
- React Email for templates (compile to HTML)
- Queue for background sending (optional: BullMQ + Redis)

**File Storage:** Local filesystem
- `/var/www/krakowbites/uploads/` for tour images
- Apache serves directly (no CDN needed initially)
- Optional: CDN later if traffic grows (Cloudflare)

**Backups:**
- PostgreSQL: `pg_dump` daily cron
- Uploads: rsync to backup server
- Systemd timer for automation

---

### Third-Party Integrations (Minimal)

**Payments:** PayU REST API
- **Endpoint:** https://secure.payu.com/api/v2_1/
- **Flow:** Standard payment (redirect to PayU, return to success page)
- **Refunds:** Automated via API
- **Webhook:** Payment status notifications to your server
- **Sandbox:** Test environment before production

**Currency Exchange:** ExchangeRate-API
- **Endpoint:** https://api.exchangerate-api.com/v4/latest/PLN
- **Frequency:** Daily cron job (fetch rates)
- **Cache:** Store in PostgreSQL, serve from cache
- **Fallback:** Static rates if API fails
- **Alternative:** Self-host exchange rate scraper

**Analytics:** Self-hosted
- **Option 1:** Plausible Analytics (self-hosted)
- **Option 2:** Umami (lightweight, self-hosted)
- **Option 3:** Apache access logs + custom parser
- No external tracking (privacy-first)

**Monitoring:**
- Application: Custom health check endpoint (`/api/health`)
- Server: Standard Linux tools (htop, netstat, journalctl)
- Uptime: Self-hosted Uptime Kuma or external StatusCake
- Error logging: Winston to files + log rotation

---

## Database Schema

### Core Tables

```sql
-- Tours
CREATE TABLE tours (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  category VARCHAR(20) NOT NULL CHECK (category IN ('food', 'heritage')),
  slug VARCHAR(100) UNIQUE NOT NULL,
  name VARCHAR(200) NOT NULL,
  tagline VARCHAR(300),
  description TEXT NOT NULL,
  what_included TEXT[], -- Array of included items
  duration_hours DECIMAL(3,1) NOT NULL,
  max_capacity INTEGER NOT NULL,
  base_price_pln DECIMAL(8,2) NOT NULL,
  private_price_pln DECIMAL(8,2) NOT NULL,
  dietary_friendly BOOLEAN DEFAULT false,
  meeting_point VARCHAR(300),
  meeting_point_lat DECIMAL(10,8),
  meeting_point_lng DECIMAL(11,8),
  image_urls TEXT[], -- Array of image URLs (local paths)
  sample_itinerary JSONB, -- [{ time: "10:00", stop: "Market", description: "..." }]
  status VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'draft', 'archived')),
  display_order INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_tours_category ON tours(category);
CREATE INDEX idx_tours_status ON tours(status);
CREATE INDEX idx_tours_slug ON tours(slug);

-- Tour Availability (scheduled tours)
CREATE TABLE tour_availability (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tour_id UUID REFERENCES tours(id) ON DELETE CASCADE,
  date DATE NOT NULL,
  time TIME NOT NULL,
  available_spots INTEGER NOT NULL,
  status VARCHAR(20) DEFAULT 'open' CHECK (status IN ('open', 'full', 'cancelled')),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(tour_id, date, time)
);

CREATE INDEX idx_availability_tour_date ON tour_availability(tour_id, date);
CREATE INDEX idx_availability_status ON tour_availability(status);

-- Bookings
CREATE TABLE bookings (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  booking_reference VARCHAR(20) UNIQUE NOT NULL, -- e.g., "KB-20260115-A7F3"
  tour_id UUID REFERENCES tours(id),
  tour_availability_id UUID REFERENCES tour_availability(id) NULL,
  booking_type VARCHAR(20) NOT NULL CHECK (booking_type IN ('scheduled', 'private')),

  -- Customer info
  customer_name VARCHAR(200) NOT NULL,
  customer_email VARCHAR(200) NOT NULL,
  customer_phone VARCHAR(50),
  customer_country VARCHAR(100),

  -- Booking details
  num_people INTEGER NOT NULL,
  preferred_date DATE,
  confirmed_date DATE,
  confirmed_time TIME,
  dietary_restrictions TEXT,

  -- Pricing
  price_per_person_pln DECIMAL(8,2) NOT NULL,
  total_price_pln DECIMAL(8,2) NOT NULL,
  currency_selected VARCHAR(3) DEFAULT 'PLN',
  currency_rate DECIMAL(10,6),

  -- Payment
  payment_status VARCHAR(20) DEFAULT 'pending' CHECK (payment_status IN ('pending', 'paid', 'refunded', 'failed')),
  payment_id VARCHAR(200), -- PayU order ID
  payment_method VARCHAR(50),
  paid_at TIMESTAMP,
  refunded_at TIMESTAMP,

  -- Status
  booking_status VARCHAR(20) DEFAULT 'pending' CHECK (booking_status IN ('pending', 'confirmed', 'cancelled', 'completed')),
  cancellation_reason TEXT,
  cancelled_at TIMESTAMP,

  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_bookings_reference ON bookings(booking_reference);
CREATE INDEX idx_bookings_email ON bookings(customer_email);
CREATE INDEX idx_bookings_status ON bookings(booking_status);
CREATE INDEX idx_bookings_payment ON bookings(payment_status);
CREATE INDEX idx_bookings_date ON bookings(confirmed_date);

-- Reviews
CREATE TABLE reviews (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tour_id UUID REFERENCES tours(id) ON DELETE CASCADE,
  booking_id UUID REFERENCES bookings(id) NULL,

  customer_name VARCHAR(200),
  customer_country VARCHAR(100),

  rating INTEGER NOT NULL CHECK (rating >= 1 AND rating <= 5),
  review_text TEXT,

  status VARCHAR(20) DEFAULT 'pending' CHECK (status IN ('pending', 'approved', 'rejected', 'spam')),
  admin_notes TEXT,

  created_at TIMESTAMP DEFAULT NOW(),
  approved_at TIMESTAMP,
  UNIQUE(booking_id)
);

CREATE INDEX idx_reviews_tour ON reviews(tour_id);
CREATE INDEX idx_reviews_status ON reviews(status);
CREATE INDEX idx_reviews_rating ON reviews(rating);

-- Tour Operator Inquiries
CREATE TABLE tour_operator_inquiries (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

  company_name VARCHAR(200) NOT NULL,
  contact_name VARCHAR(200) NOT NULL,
  email VARCHAR(200) NOT NULL,
  phone VARCHAR(50),

  requested_tours TEXT[],
  preferred_dates DATE[],
  group_size INTEGER,
  message TEXT,

  status VARCHAR(20) DEFAULT 'new' CHECK (status IN ('new', 'contacted', 'negotiating', 'converted', 'declined')),
  admin_notes TEXT,

  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_inquiries_status ON tour_operator_inquiries(status);
CREATE INDEX idx_inquiries_created ON tour_operator_inquiries(created_at);

-- Admin Users
CREATE TABLE admin_users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(200) UNIQUE NOT NULL,
  password_hash VARCHAR(200) NOT NULL, -- bcrypt/argon2
  name VARCHAR(200),
  role VARCHAR(20) DEFAULT 'admin',
  created_at TIMESTAMP DEFAULT NOW(),
  last_login TIMESTAMP
);

-- Sessions (for admin auth)
CREATE TABLE sessions (
  id VARCHAR(255) PRIMARY KEY,
  user_id UUID REFERENCES admin_users(id) ON DELETE CASCADE,
  expires_at TIMESTAMP NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_sessions_user ON sessions(user_id);
CREATE INDEX idx_sessions_expires ON sessions(expires_at);

-- Settings (key-value store)
CREATE TABLE settings (
  key VARCHAR(100) PRIMARY KEY,
  value JSONB NOT NULL,
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Example settings:
-- INSERT INTO settings VALUES ('exchange_rates', '{"USD": 4.02, "EUR": 4.35, "GBP": 5.12, "updated_at": "2026-01-01"}');
```

---

### Drizzle ORM Schema (Alternative to Raw SQL)

```typescript
// src/db/schema.ts
import { pgTable, uuid, varchar, text, integer, decimal, timestamp, date, time, boolean, jsonb } from 'drizzle-orm/pg-core';

export const tours = pgTable('tours', {
  id: uuid('id').primaryKey().defaultRandom(),
  category: varchar('category', { length: 20, enum: ['food', 'heritage'] }).notNull(),
  slug: varchar('slug', { length: 100 }).unique().notNull(),
  name: varchar('name', { length: 200 }).notNull(),
  tagline: varchar('tagline', { length: 300 }),
  description: text('description').notNull(),
  whatIncluded: text('what_included').array(),
  durationHours: decimal('duration_hours', { precision: 3, scale: 1 }).notNull(),
  maxCapacity: integer('max_capacity').notNull(),
  basePricePln: decimal('base_price_pln', { precision: 8, scale: 2 }).notNull(),
  privatePricePln: decimal('private_price_pln', { precision: 8, scale: 2 }).notNull(),
  dietaryFriendly: boolean('dietary_friendly').default(false),
  meetingPoint: varchar('meeting_point', { length: 300 }),
  meetingPointLat: decimal('meeting_point_lat', { precision: 10, scale: 8 }),
  meetingPointLng: decimal('meeting_point_lng', { precision: 11, scale: 8 }),
  imageUrls: text('image_urls').array(),
  sampleItinerary: jsonb('sample_itinerary'),
  status: varchar('status', { length: 20, enum: ['active', 'draft', 'archived'] }).default('active'),
  displayOrder: integer('display_order').default(0),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});

export const bookings = pgTable('bookings', {
  id: uuid('id').primaryKey().defaultRandom(),
  bookingReference: varchar('booking_reference', { length: 20 }).unique().notNull(),
  tourId: uuid('tour_id').references(() => tours.id).notNull(),
  bookingType: varchar('booking_type', { length: 20, enum: ['scheduled', 'private'] }).notNull(),
  customerName: varchar('customer_name', { length: 200 }).notNull(),
  customerEmail: varchar('customer_email', { length: 200 }).notNull(),
  customerPhone: varchar('customer_phone', { length: 50 }),
  numPeople: integer('num_people').notNull(),
  confirmedDate: date('confirmed_date'),
  confirmedTime: time('confirmed_time'),
  dietaryRestrictions: text('dietary_restrictions'),
  totalPricePln: decimal('total_price_pln', { precision: 8, scale: 2 }).notNull(),
  paymentStatus: varchar('payment_status', { length: 20, enum: ['pending', 'paid', 'refunded', 'failed'] }).default('pending'),
  paymentId: varchar('payment_id', { length: 200 }),
  bookingStatus: varchar('booking_status', { length: 20, enum: ['pending', 'confirmed', 'cancelled', 'completed'] }).default('pending'),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});

// ... similar for other tables
```

---

## API Design

### Public API Routes (Astro Endpoints)

```typescript
// src/pages/api/tours.ts
import type { APIRoute } from 'astro';
import { db } from '@/db';
import { tours } from '@/db/schema';
import { eq } from 'drizzle-orm';

export const GET: APIRoute = async ({ request }) => {
  const url = new URL(request.url);
  const category = url.searchParams.get('category');

  const allTours = await db.select()
    .from(tours)
    .where(category ? eq(tours.category, category) : undefined);

  return new Response(JSON.stringify({ tours: allTours }), {
    status: 200,
    headers: { 'Content-Type': 'application/json' }
  });
};
```

```typescript
// src/pages/api/bookings.ts
import type { APIRoute } from 'astro';
import { db } from '@/db';
import { bookings } from '@/db/schema';
import { generateBookingReference } from '@/lib/utils';
import { initiatePayUPayment } from '@/lib/payu';

export const POST: APIRoute = async ({ request }) => {
  const body = await request.json();

  // Validate input
  // Create booking record
  const booking = await db.insert(bookings).values({
    bookingReference: generateBookingReference(),
    tourId: body.tourId,
    customerName: body.customerName,
    customerEmail: body.customerEmail,
    // ... other fields
  }).returning();

  // Initiate PayU payment
  const paymentUrl = await initiatePayUPayment(booking[0]);

  return new Response(JSON.stringify({ booking: booking[0], paymentUrl }), {
    status: 200,
    headers: { 'Content-Type': 'application/json' }
  });
};
```

### Admin API Routes (Protected)

```typescript
// src/pages/api/admin/bookings.ts
import type { APIRoute } from 'astro';
import { isAuthenticated } from '@/lib/auth';

export const GET: APIRoute = async ({ request, cookies }) => {
  // Check auth
  const session = await isAuthenticated(cookies);
  if (!session) {
    return new Response('Unauthorized', { status: 401 });
  }

  // Fetch bookings
  const allBookings = await db.select().from(bookings);

  return new Response(JSON.stringify({ bookings: allBookings }), {
    status: 200,
    headers: { 'Content-Type': 'application/json' }
  });
};
```

---

## Apache Configuration

```apache
# /etc/apache2/sites-available/krakowbites.conf

<VirtualHost *:80>
    ServerName krakowbites.com
    ServerAlias www.krakowbites.com

    # Redirect HTTP to HTTPS
    Redirect permanent / https://krakowbites.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName krakowbites.com
    ServerAlias www.krakowbites.com

    # SSL Configuration
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/krakowbites.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/krakowbites.com/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf

    # Enable HTTP/2
    Protocols h2 http/1.1

    # Compression
    <IfModule mod_deflate.c>
        AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript application/json
    </IfModule>

    # Security Headers
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-XSS-Protection "1; mode=block"
    Header always set Referrer-Policy "strict-origin-when-cross-origin"

    # Static assets served directly by Apache (faster)
    Alias /uploads /var/www/krakowbites/uploads
    Alias /_astro /var/www/krakowbites/dist/client/_astro

    <Directory /var/www/krakowbites/uploads>
        Require all granted
        Options -Indexes
        # Cache images for 1 year
        <FilesMatch "\.(jpg|jpeg|png|gif|webp|svg)$">
            Header set Cache-Control "max-age=31536000, public, immutable"
        </FilesMatch>
    </Directory>

    <Directory /var/www/krakowbites/dist/client/_astro>
        Require all granted
        # Cache hashed assets forever
        Header set Cache-Control "max-age=31536000, public, immutable"
    </Directory>

    # Reverse proxy to Node.js for everything else
    ProxyPreserveHost On
    ProxyPass /uploads !
    ProxyPass /_astro !
    ProxyPass / http://127.0.0.1:3000/
    ProxyPassReverse / http://127.0.0.1:3000/

    # Logs
    ErrorLog ${APACHE_LOG_DIR}/krakowbites-error.log
    CustomLog ${APACHE_LOG_DIR}/krakowbites-access.log combined
</VirtualHost>
```

**Enable required modules:**
```bash
sudo a2enmod proxy proxy_http ssl headers deflate http2
sudo systemctl reload apache2
```

---

## systemd Service Configuration

```ini
# /etc/systemd/system/krakowbites.service

[Unit]
Description=KrakowBites Astro SSR Application
After=network.target postgresql.service apache2.service
Requires=postgresql.service

[Service]
Type=simple
User=www-data
Group=www-data
WorkingDirectory=/var/www/krakowbites

# Production environment
Environment="NODE_ENV=production"
Environment="HOST=127.0.0.1"
Environment="PORT=3000"

# Load secrets from file (more secure than inline)
EnvironmentFile=/var/www/krakowbites/.env.production

# Start command
ExecStart=/usr/bin/node /var/www/krakowbites/dist/server/entry.mjs

# Restart policy
Restart=on-failure
RestartSec=10
KillMode=mixed
TimeoutStopSec=30

# Resource limits (optional)
LimitNOFILE=65535
MemoryLimit=512M

# Logging
StandardOutput=journal
StandardError=journal
SyslogIdentifier=krakowbites

[Install]
WantedBy=multi-user.target
```

**Environment file:**
```bash
# /var/www/krakowbites/.env.production
DATABASE_URL=postgresql://krakowbites_user:password@localhost:5432/krakowbites
PAYU_POS_ID=123456
PAYU_SECRET_KEY=your_secret_key
PAYU_OAUTH_CLIENT_ID=123456
PAYU_OAUTH_SECRET=your_oauth_secret
SMTP_HOST=mail.yourdomain.com
SMTP_PORT=587
SMTP_USER=bookings@krakowbites.com
SMTP_PASSWORD=your_smtp_password
JWT_SECRET=your_jwt_secret
EXCHANGE_RATE_API_KEY=your_api_key
```

**Manage service:**
```bash
# Enable on boot
sudo systemctl enable krakowbites

# Start/stop/restart
sudo systemctl start krakowbites
sudo systemctl stop krakowbites
sudo systemctl restart krakowbites

# View logs
sudo journalctl -u krakowbites -f

# Check status
sudo systemctl status krakowbites
```

---

## File Structure

```
krakowbites/
├── src/
│   ├── pages/
│   │   ├── index.astro              # Homepage (0 JS)
│   │   ├── tours/
│   │   │   ├── index.astro          # All tours (0 JS)
│   │   │   ├── food.astro           # Food tours (0 JS)
│   │   │   ├── heritage.astro       # Heritage tours (0 JS)
│   │   │   └── [slug].astro         # Tour detail (minimal JS)
│   │   ├── about.astro              # (0 JS)
│   │   ├── contact.astro            # (minimal JS for form)
│   │   ├── booking/
│   │   │   └── [reference].astro    # Confirmation page
│   │   ├── admin/
│   │   │   ├── index.astro          # Dashboard
│   │   │   ├── bookings.astro
│   │   │   ├── tours.astro
│   │   │   └── reviews.astro
│   │   └── api/
│   │       ├── tours.ts
│   │       ├── tours/
│   │       │   └── [id]/
│   │       │       └── availability.ts
│   │       ├── bookings.ts
│   │       ├── bookings/
│   │       │   └── [reference]/
│   │       │       └── cancel.ts
│   │       ├── reviews.ts
│   │       ├── inquiries/
│   │       │   └── tour-operators.ts
│   │       ├── settings/
│   │       │   └── exchange-rates.ts
│   │       ├── webhooks/
│   │       │   └── payu.ts
│   │       └── admin/
│   │           ├── bookings.ts
│   │           ├── reviews.ts
│   │           └── analytics.ts
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Header.astro
│   │   │   ├── Footer.astro
│   │   │   └── Navigation.astro
│   │   ├── tours/
│   │   │   ├── TourCard.astro       # Pure HTML
│   │   │   ├── TourGrid.astro       # Pure HTML
│   │   │   └── TourDetail.astro     # Pure HTML
│   │   ├── islands/                 # Only these have JS
│   │   │   ├── BookingCalendar.tsx  # React island
│   │   │   ├── BookingForm.tsx      # React island
│   │   │   ├── CurrencyToggle.tsx   # React island
│   │   │   └── ReviewForm.tsx       # React island
│   │   ├── ui/                      # Reusable UI components
│   │   │   ├── Button.astro
│   │   │   ├── Card.astro
│   │   │   └── Input.astro
│   │   └── Map.astro                # Leaflet map (lazy loaded)
│   ├── layouts/
│   │   ├── Layout.astro             # Base layout
│   │   └── AdminLayout.astro        # Admin layout
│   ├── db/
│   │   ├── index.ts                 # DB connection
│   │   ├── schema.ts                # Drizzle schema
│   │   └── migrations/              # SQL migration files
│   ├── lib/
│   │   ├── auth.ts                  # Authentication utilities
│   │   ├── payu.ts                  # PayU API client
│   │   ├── email.ts                 # Nodemailer setup
│   │   ├── currency.ts              # Currency conversion
│   │   └── utils.ts                 # General utilities
│   ├── styles/
│   │   ├── global.css               # Global styles + Tailwind
│   │   └── admin.css                # Admin-specific styles
│   └── types/
│       └── index.ts                 # TypeScript types
├── uploads/                         # User uploaded images
│   └── tours/
├── public/                          # Static assets
│   ├── images/
│   ├── fonts/
│   └── favicon.svg
├── dist/                            # Build output
│   ├── server/                      # SSR server
│   └── client/                      # Static assets
├── scripts/
│   ├── migrate.ts                   # Run migrations
│   ├── seed.ts                      # Seed database
│   └── cron/
│       ├── exchange-rates.ts        # Update rates
│       └── send-reviews.ts          # Review reminders
├── emails/                          # React Email templates
│   ├── BookingConfirmation.tsx
│   ├── CancellationConfirmation.tsx
│   └── ReviewRequest.tsx
├── astro.config.mjs
├── tailwind.config.mjs
├── tsconfig.json
├── package.json
├── drizzle.config.ts                # Drizzle ORM config
└── .env.production                  # Environment variables
```

---

## Database Connection

### Using Drizzle ORM (Recommended)

```typescript
// src/db/index.ts
import { drizzle } from 'drizzle-orm/node-postgres';
import { Pool } from 'pg';
import * as schema from './schema';

const pool = new Pool({
  host: process.env.DB_HOST || 'localhost',
  port: parseInt(process.env.DB_PORT || '5432'),
  database: process.env.DB_NAME || 'krakowbites',
  user: process.env.DB_USER || 'krakowbites_user',
  password: process.env.DB_PASSWORD,
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

export const db = drizzle(pool, { schema });

// Health check
export async function checkDatabaseHealth() {
  try {
    await pool.query('SELECT 1');
    return true;
  } catch (error) {
    console.error('Database health check failed:', error);
    return false;
  }
}
```

### Using Raw SQL (Alternative)

```typescript
// src/db/index.ts
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20,
});

export const db = {
  async query<T = any>(text: string, params?: any[]): Promise<T[]> {
    const result = await pool.query(text, params);
    return result.rows as T[];
  },

  async one<T = any>(text: string, params?: any[]): Promise<T | null> {
    const rows = await this.query<T>(text, params);
    return rows[0] || null;
  },

  async transaction<T>(callback: (client: any) => Promise<T>): Promise<T> {
    const client = await pool.connect();
    try {
      await client.query('BEGIN');
      const result = await callback(client);
      await client.query('COMMIT');
      return result;
    } catch (error) {
      await client.query('ROLLBACK');
      throw error;
    } finally {
      client.release();
    }
  }
};

// Usage:
import { db } from '@/db';
import type { Tour } from '@/types';

const tour = await db.one<Tour>(
  'SELECT * FROM tours WHERE slug = $1 AND status = $2',
  [slug, 'active']
);
```

---

## Email Configuration

```typescript
// src/lib/email.ts
import nodemailer from 'nodemailer';
import { render } from '@react-email/render';
import BookingConfirmation from '../emails/BookingConfirmation';
import type { Booking, Tour } from '@/types';

const transporter = nodemailer.createTransport({
  host: process.env.SMTP_HOST,
  port: parseInt(process.env.SMTP_PORT || '587'),
  secure: false, // true for 465, false for other ports
  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASSWORD,
  },
  // Optional: DKIM signing
  dkim: {
    domainName: 'krakowbites.com',
    keySelector: 'mail',
    privateKey: process.env.DKIM_PRIVATE_KEY,
  },
});

export async function sendBookingConfirmation(booking: Booking, tour: Tour) {
  const html = render(BookingConfirmation({ booking, tour }));
  const text = render(BookingConfirmation({ booking, tour }), { plainText: true });

  const info = await transporter.sendMail({
    from: '"KrakowBites" <bookings@krakowbites.com>',
    to: booking.customerEmail,
    subject: `Your tour is confirmed! ${tour.name}`,
    text,
    html,
    attachments: [{
      filename: `ticket-${booking.bookingReference}.pdf`,
      content: await generateTicketPDF(booking, tour),
    }],
  });

  console.log('Email sent:', info.messageId);
}

export async function sendReviewRequest(booking: Booking, tour: Tour) {
  // Similar implementation
}

export async function sendCancellationConfirmation(booking: Booking, tour: Tour) {
  // Similar implementation
}
```

---

## Cron Jobs (systemd timers)

### Exchange Rates Update

```bash
# /etc/systemd/system/krakowbites-exchange-rates.timer
[Unit]
Description=Update KrakowBites exchange rates daily

[Timer]
OnCalendar=daily
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

```bash
# /etc/systemd/system/krakowbites-exchange-rates.service
[Unit]
Description=KrakowBites Exchange Rates Update

[Service]
Type=oneshot
User=www-data
WorkingDirectory=/var/www/krakowbites
ExecStart=/usr/bin/node /var/www/krakowbites/scripts/cron/exchange-rates.js
EnvironmentFile=/var/www/krakowbites/.env.production
```

```typescript
// scripts/cron/exchange-rates.ts
import { db } from '../src/db';
import { settings } from '../src/db/schema';

async function updateExchangeRates() {
  const response = await fetch('https://api.exchangerate-api.com/v4/latest/PLN');
  const data = await response.json();

  await db.insert(settings)
    .values({
      key: 'exchange_rates',
      value: {
        USD: data.rates.USD,
        EUR: data.rates.EUR,
        GBP: data.rates.GBP,
        updatedAt: new Date().toISOString(),
      }
    })
    .onConflictDoUpdate({
      target: settings.key,
      set: {
        value: {
          USD: data.rates.USD,
          EUR: data.rates.EUR,
          GBP: data.rates.GBP,
          updatedAt: new Date().toISOString(),
        },
        updatedAt: new Date(),
      }
    });

  console.log('Exchange rates updated');
}

updateExchangeRates().catch(console.error);
```

**Enable timer:**
```bash
sudo systemctl enable krakowbites-exchange-rates.timer
sudo systemctl start krakowbites-exchange-rates.timer
sudo systemctl list-timers # Check status
```

---

## Backup Strategy

### PostgreSQL Backup

```bash
# /etc/systemd/system/krakowbites-backup.timer
[Unit]
Description=KrakowBites daily backup

[Timer]
OnCalendar=daily
OnCalendar=*-*-* 03:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

```bash
# /etc/systemd/system/krakowbites-backup.service
[Unit]
Description=KrakowBites Database Backup

[Service]
Type=oneshot
User=postgres
ExecStart=/usr/local/bin/krakowbites-backup.sh
```

```bash
#!/bin/bash
# /usr/local/bin/krakowbites-backup.sh

BACKUP_DIR=/var/backups/krakowbites
DATE=$(date +\%Y-\%m-\%d_\%H-\%M-\%S)

# Create backup directory
mkdir -p $BACKUP_DIR

# Dump database
pg_dump krakowbites | gzip > $BACKUP_DIR/krakowbites_$DATE.sql.gz

# Backup uploads
tar -czf $BACKUP_DIR/uploads_$DATE.tar.gz /var/www/krakowbites/uploads

# Keep only last 30 days
find $BACKUP_DIR -name "*.sql.gz" -mtime +30 -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +30 -delete

# Optional: rsync to remote backup server
# rsync -avz $BACKUP_DIR/ backup-server:/backups/krakowbites/

echo "Backup completed: $DATE"
```

```bash
chmod +x /usr/local/bin/krakowbites-backup.sh
sudo systemctl enable krakowbites-backup.timer
sudo systemctl start krakowbites-backup.timer
```

---

## Deployment Workflow

### Development

```bash
# Clone repository
git clone https://github.com/yourname/krakowbites.git
cd krakowbites

# Install dependencies
npm install

# Setup environment
cp .env.example .env.local
# Edit .env.local with your credentials

# Run migrations
npm run db:migrate

# Seed database
npm run db:seed

# Start dev server
npm run dev
# Visit http://localhost:4321
```

### Production Deployment

```bash
# On VPS
cd /var/www
sudo git clone https://github.com/yourname/krakowbites.git
cd krakowbites

# Install production dependencies
npm ci --production

# Build
npm run build

# Setup environment
sudo cp .env.example .env.production
sudo nano .env.production # Fill in production values

# Run migrations
npm run db:migrate

# Set permissions
sudo chown -R www-data:www-data /var/www/krakowbites
sudo chmod 640 .env.production

# Start service
sudo systemctl start krakowbites

# Check logs
sudo journalctl -u krakowbites -f
```

### Updates (Zero-downtime)

```bash
# Pull latest code
cd /var/www/krakowbites
sudo -u www-data git pull

# Install dependencies
sudo -u www-data npm ci --production

# Build new version
sudo -u www-data npm run build

# Run migrations if needed
sudo -u www-data npm run db:migrate

# Reload service (graceful restart)
sudo systemctl reload krakowbites

# Or restart if reload not supported
sudo systemctl restart krakowbites
```

---

## Performance Optimization

### Astro Config

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import node from '@astrojs/node';
import react from '@astrojs/react';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  output: 'server', // SSR mode
  adapter: node({
    mode: 'standalone', // Single executable
  }),
  integrations: [
    react(), // Only for islands
    tailwind(),
  ],
  build: {
    inlineStylesheets: 'auto',
  },
  vite: {
    build: {
      minify: 'esbuild',
      cssMinify: true,
    },
  },
  image: {
    service: {
      entrypoint: 'astro/assets/services/sharp', // Local image optimization
    },
  },
  compressHTML: true,
});
```

### Caching Headers

```typescript
// src/pages/tours/[slug].astro
---
// Cache tour pages for 1 hour (revalidate in background)
Astro.response.headers.set('Cache-Control', 'public, max-age=3600, stale-while-revalidate=86400');
---
```

### Image Optimization

```astro
---
import { Image } from 'astro:assets';
---

<!-- Automatic WebP conversion, responsive sizes, lazy loading -->
<Image
  src={tour.imageUrls[0]}
  alt={tour.name}
  width={800}
  height={600}
  format="webp"
  quality={80}
  loading="lazy"
/>
```

---

## Monitoring & Health Checks

### Health Check Endpoint

```typescript
// src/pages/api/health.ts
import type { APIRoute } from 'astro';
import { checkDatabaseHealth } from '@/db';

export const GET: APIRoute = async () => {
  const dbHealthy = await checkDatabaseHealth();

  const health = {
    status: dbHealthy ? 'healthy' : 'unhealthy',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    database: dbHealthy,
  };

  return new Response(JSON.stringify(health), {
    status: dbHealthy ? 200 : 503,
    headers: { 'Content-Type': 'application/json' }
  });
};
```

### Uptime Monitoring

- **Self-hosted Uptime Kuma:** https://github.com/louislam/uptime-kuma
- **External:** StatusCake, UptimeRobot (free tiers available)
- Monitor `/api/health` endpoint every 5 minutes

---

## Security Checklist

- [x] HTTPS enforced (Let's Encrypt)
- [x] Security headers (X-Frame-Options, CSP, etc.)
- [x] HTTP-only cookies for sessions
- [x] CSRF protection (built into forms)
- [x] SQL injection prevention (parameterized queries)
- [x] XSS prevention (Astro auto-escaping)
- [x] Rate limiting (nginx/Apache or custom middleware)
- [x] Password hashing (bcrypt/argon2)
- [x] Environment variables for secrets
- [x] File upload validation
- [x] PayU webhook signature verification
- [x] Regular security updates (`npm audit`)
- [x] Fail2ban for SSH brute force protection
- [x] Firewall (ufw) - only ports 22, 80, 443 open
- [x] Database user limited permissions (no DROP/CREATE on production)

---

## Estimated Costs

| Item | Cost (Monthly) |
|------|----------------|
| VPS (existing) | 0 PLN (already owned) |
| Domain (.com) | ~5 PLN/month (~60 PLN/year) |
| SSL Certificate | 0 PLN (Let's Encrypt) |
| PostgreSQL | 0 PLN (self-hosted) |
| Email (SMTP) | 0 PLN (existing infrastructure) |
| PayU fees | ~2-3% per transaction |
| Exchange rate API | 0 PLN (free tier: 1500 requests/month) |
| Backups | 0 PLN (local + optional rsync) |
| **Total fixed costs** | **~5 PLN/month** |

**One-time costs:**
- Development: DIY
- Logo design: 500-1500 PLN (optional)
- Photography: 1000-2000 PLN (optional)

---

## Next Steps

1. **Initialize Astro project** on VPS
2. **Configure Apache** reverse proxy
3. **Setup PostgreSQL** database and migrations
4. **Implement core pages** (homepage, tour listing, tour detail)
5. **Build booking flow** with PayU sandbox
6. **Configure email** with existing SMTP
7. **Deploy systemd service** and timers
8. **Test end-to-end** booking flow
9. **Go live**

---

**Document Status:** Architecture updated for self-hosted infrastructure
**Tech Stack:** Astro + TypeScript + PostgreSQL + Apache2 + Node.js
**Hosting:** Self-managed VPS (no external SaaS dependencies)
