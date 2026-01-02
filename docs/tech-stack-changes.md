# Technology Stack Changes - Self-Hosted Infrastructure

**Date:** 2026-01-01
**Version:** Updated from Next.js/Vercel to Astro/Self-hosted

---

## Summary of Changes

Based on your infrastructure expertise (VPS, Apache2, PostgreSQL, SMTP cluster management), the technical architecture has been updated to eliminate external SaaS dependencies and prioritize HTML/CSS with minimal JavaScript.

---

## Key Technology Changes

### Frontend Framework

| Before | After | Why |
|--------|-------|-----|
| **Next.js 15 (App Router)** | **Astro 4.x** | Zero JS by default, ships pure HTML/CSS for static content |
| React Server Components | Astro components (pure HTML) | No React runtime for static pages (~80kb saved) |
| Client Components | React Islands (only where needed) | Interactive components isolated, ~5-10kb JS total |
| Shadcn/ui component library | Tailwind CSS + custom components | No component library overhead |

**JavaScript Shipped:**
- **Before:** 80-100kb base (Next.js + React runtime)
- **After:** 0kb for static pages, 5-10kb for interactive pages (booking calendar, forms)

---

### Backend & Hosting

| Before | After | Why |
|--------|-------|-----|
| **Vercel** serverless | **Self-hosted VPS** | Full control, no monthly fees, Apache expertise |
| Next.js API Routes | Astro API endpoints | Same RESTful pattern, different framework |
| Vercel Edge Functions | Apache2 reverse proxy → Node.js | Traditional long-running process, systemd managed |
| Vercel deployment | Git pull + build + systemd restart | Manual but predictable deployment |

**Infrastructure:**
- Apache2 serves static assets directly (`/uploads/`, `/_astro/`)
- Node.js handles dynamic routes on port 3000
- systemd manages Node.js process (auto-restart, logging)
- Let's Encrypt for SSL (free, auto-renewing)

---

### Database

| Before | After | Why |
|--------|-------|-----|
| **Supabase** (managed PostgreSQL) | **Self-hosted PostgreSQL 16+** | Full control, no external service |
| Prisma ORM | Drizzle ORM or raw SQL (`pg` library) | Lightweight, SQL-like syntax, or full control |
| Supabase client library | Direct `pg` connection | No intermediary layer |
| Supabase migrations | Custom SQL scripts or Drizzle Kit | Standard PostgreSQL migration tools |

**Connection:**
```typescript
// Before (Supabase)
import { createClient } from '@supabase/supabase-js';
const supabase = createClient(url, key);

// After (Drizzle)
import { drizzle } from 'drizzle-orm/node-postgres';
import { Pool } from 'pg';
const pool = new Pool({ host: 'localhost', ... });
const db = drizzle(pool, { schema });

// Or raw SQL
const result = await pool.query('SELECT * FROM tours WHERE slug = $1', [slug]);
```

---

### Email

| Before | After | Why |
|--------|-------|-----|
| **Resend** (SaaS) | **Existing SMTP cluster** | You already manage SMTP/IMAP servers |
| Resend API | Nodemailer | Industry standard, works with any SMTP |
| 100 emails/day free tier | Unlimited | Your infrastructure, no limits |
| React Email templates | React Email templates (same) | Compile to HTML, works with Nodemailer |

**Configuration:**
```typescript
// src/lib/email.ts
import nodemailer from 'nodemailer';

const transporter = nodemailer.createTransport({
  host: process.env.SMTP_HOST, // Your mail server
  port: 587,
  auth: {
    user: 'bookings@krakowbites.com',
    pass: process.env.SMTP_PASSWORD,
  },
});
```

---

### File Storage

| Before | After | Why |
|--------|-------|-----|
| **Cloudinary** or Vercel Blob | **Local filesystem** | Apache serves directly, no CDN needed initially |
| External CDN | `/var/www/krakowbites/uploads/` | Apache handles static file serving efficiently |
| Automatic optimization | Astro's built-in image optimization | Sharp for WebP conversion, responsive sizes |
| Monthly bandwidth limits | Unlimited | Your server bandwidth |

**Apache serves directly:**
```apache
Alias /uploads /var/www/krakowbites/uploads
<Directory /var/www/krakowbites/uploads>
    Require all granted
    Header set Cache-Control "max-age=31536000, public, immutable"
</Directory>
```

---

### Analytics

| Before | After | Why |
|--------|-------|-----|
| **Plausible Analytics** (cloud) | **Self-hosted Plausible or Umami** | Privacy-first, full data ownership |
| Google Analytics 4 | Apache access logs (alternative) | Already collected, can parse for insights |
| External tracking | No external tracking | GDPR compliant by default |

**Self-hosted options:**
- Plausible Analytics (Docker container on VPS)
- Umami (lightweight, self-hosted)
- Custom Apache log parser (AWStats, GoAccess)

---

### Authentication

| Before | After | Why |
|--------|-------|-----|
| **NextAuth.js 5** | **Custom JWT or Lucia Auth** | Simpler for single admin user |
| OAuth providers | Email/password only | No social auth needed |
| NextAuth session | PostgreSQL session table | Direct database session storage |
| Built-in CSRF protection | Custom CSRF tokens | Standard approach |

---

### Process Management

| Before | After | Why |
|--------|-------|-----|
| **Vercel** automatic scaling | **systemd** service | Standard Linux process management |
| Serverless cold starts | Long-running process | Always warm, consistent performance |
| Automatic deployments | Manual `git pull` + `systemd restart` | Controlled, predictable updates |
| Vercel logs | journalctl logs | Native Linux logging |

**systemd service:**
```ini
[Unit]
Description=KrakowBites Astro SSR
After=postgresql.service apache2.service

[Service]
Type=simple
User=www-data
ExecStart=/usr/bin/node /var/www/krakowbites/dist/server/entry.mjs
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

---

### Cron Jobs

| Before | After | Why |
|--------|-------|-----|
| **Vercel Cron** (vercel.json) | **systemd timers** | Native Linux scheduling |
| Limited to Vercel regions | Runs on your VPS | Local execution |
| Cron secret validation | No external API needed | Direct script execution |

**Example (exchange rates update):**
```bash
# /etc/systemd/system/krakowbites-exchange-rates.timer
[Timer]
OnCalendar=daily
OnCalendar=*-*-* 02:00:00

# /etc/systemd/system/krakowbites-exchange-rates.service
[Service]
ExecStart=/usr/bin/node /var/www/krakowbites/scripts/cron/exchange-rates.js
```

---

### Backups

| Before | After | Why |
|--------|-------|-----|
| **Supabase** automatic backups | **Custom backup strategy** | Full control over backup schedule |
| Point-in-time recovery (PITR) | `pg_dump` daily cron | Standard PostgreSQL backup |
| File backups not needed | `rsync` to backup server | Standard file backup |
| Managed by Supabase | systemd timer + backup script | Self-managed, reliable |

**Backup script:**
```bash
#!/bin/bash
pg_dump krakowbites | gzip > /var/backups/krakowbites_$(date +%Y-%m-%d).sql.gz
tar -czf /var/backups/uploads_$(date +%Y-%m-%d).tar.gz /var/www/krakowbites/uploads
find /var/backups -name "*.gz" -mtime +30 -delete
```

---

## What Stays the Same

These parts of the architecture remain unchanged:

- **PayU payment integration** - Same API, same flow
- **React Email templates** - Still compiled to HTML for emails
- **Tailwind CSS** - Same styling approach
- **TypeScript** - Still primary language
- **PostgreSQL database schema** - Identical tables/structure
- **Leaflet.js maps** - Same mapping library
- **Zod validation** - Same schema validation
- **Business logic** - Tour catalog, booking flow, calendar management all identical

---

## Cost Comparison

### Before (SaaS Stack)

| Service | Cost |
|---------|------|
| Vercel | Free tier (hobby) |
| Supabase | Free tier (500MB) |
| Resend | Free tier (100 emails/day) |
| Cloudinary | Free tier (25GB/month) |
| Domain | ~60 PLN/year |
| **Total** | ~5 PLN/month (domain only) |

**Limits:**
- Vercel: 100GB bandwidth/month
- Supabase: 500MB storage, 2GB bandwidth
- Resend: 100 emails/day
- Cloudinary: 25GB bandwidth

---

### After (Self-Hosted Stack)

| Service | Cost |
|---------|------|
| VPS | 0 PLN (already owned) |
| PostgreSQL | 0 PLN (self-hosted) |
| SMTP | 0 PLN (existing infrastructure) |
| File storage | 0 PLN (local filesystem) |
| SSL | 0 PLN (Let's Encrypt) |
| Domain | ~60 PLN/year |
| **Total** | ~5 PLN/month (domain only) |

**Limits:**
- **None** - your VPS capacity is the limit
- No bandwidth caps
- No email send limits
- No storage quotas

**Same monthly cost, zero external dependencies!**

---

## Development Workflow Changes

### Before (Next.js + Vercel)

```bash
# Development
npm run dev # http://localhost:3000

# Deployment
git push origin main
# Vercel auto-deploys

# Environment variables
# Set in Vercel dashboard
```

---

### After (Astro + Self-hosted)

```bash
# Development
npm run dev # http://localhost:4321

# Build
npm run build

# Deployment
git push origin main
ssh user@vps
cd /var/www/krakowbites
git pull
npm ci --production
npm run build
sudo systemctl restart krakowbites

# Environment variables
# Edit /var/www/krakowbites/.env.production
```

**Future improvement:** Setup git post-receive hook for automatic deployment on push.

---

## File Structure Comparison

### Component Example

**Before (Next.js):**
```tsx
// app/tours/page.tsx (React Server Component)
export default async function ToursPage() {
  const tours = await db.tour.findMany();

  return (
    <div>
      {tours.map(tour => <TourCard key={tour.id} tour={tour} />)}
    </div>
  );
}
```

**After (Astro):**
```astro
---
// src/pages/tours/index.astro (0 JS shipped)
import { db } from '@/db';
import TourCard from '@/components/tours/TourCard.astro';

const tours = await db.select().from(tours);
---

<Layout title="Tours">
  <div class="tour-grid">
    {tours.map(tour => <TourCard tour={tour} />)}
  </div>
</Layout>
```

**Result:**
- Next.js: Ships React runtime + hydration code (~80kb)
- Astro: Ships pure HTML + CSS (0kb JS)

---

### Interactive Component (Island)

**Before (Next.js Client Component):**
```tsx
'use client';
import { useState } from 'react';

export default function BookingCalendar({ tourId }: Props) {
  const [selectedDate, setSelectedDate] = useState(null);
  // ... calendar logic
}
```

**After (Astro Island):**
```tsx
// src/components/islands/BookingCalendar.tsx (React island)
import { useState } from 'react';

export default function BookingCalendar({ tourId }: Props) {
  const [selectedDate, setSelectedDate] = useState(null);
  // ... same logic
}
```

```astro
<!-- Usage in Astro page -->
<BookingCalendar client:load tourId={tour.id} />
```

**Result:**
- Same React code, but only this component gets JS
- Rest of page is static HTML
- ~10kb JS instead of 80kb for the whole page

---

## Migration Path (If Needed)

If you had already started with Next.js, here's how to migrate:

1. **Database:** Export Supabase data → Import to self-hosted PostgreSQL
2. **Images:** Download from Cloudinary → Upload to `/var/www/krakowbites/uploads/`
3. **Components:** Convert React components to Astro components (mostly HTML/CSS)
4. **API routes:** Minimal changes (same logic, different framework syntax)
5. **Environment variables:** Move from Vercel dashboard to `.env.production` file

**Time estimate:** 2-3 days for experienced developer

---

## Performance Expectations

### Page Load Times

**Before (Next.js + Vercel):**
- Homepage: ~1.2s (includes React hydration)
- Tour detail: ~1.4s (includes React hydration)
- Booking page: ~1.6s (interactive components)

**After (Astro + Self-hosted):**
- Homepage: ~0.4s (pure HTML, no JS)
- Tour detail: ~0.5s (pure HTML, no JS)
- Booking page: ~0.8s (only calendar island has JS)

**3x faster for static content pages**

---

### Lighthouse Scores (Expected)

**Before:**
- Performance: 85-90
- Accessibility: 95+
- Best Practices: 95+
- SEO: 100

**After:**
- Performance: 95-100 (less JS to parse)
- Accessibility: 95+
- Best Practices: 95+
- SEO: 100

---

## Operational Advantages

### Self-hosted Benefits

1. **Full control** - No service limitations, change anything
2. **Debugging** - Direct server access, journalctl logs, database inspection
3. **Cost predictability** - No surprise bills for bandwidth overages
4. **Data sovereignty** - All data on your infrastructure
5. **Customization** - Tweak Apache, PostgreSQL, Node.js settings as needed
6. **Learning** - Understand entire stack deeply
7. **No vendor lock-in** - Can move to different VPS provider easily

### Trade-offs

1. **Manual scaling** - Need to upgrade VPS if traffic explodes (unlikely for solo guide)
2. **Manual deployment** - No automatic Vercel magic (can automate with git hooks)
3. **Maintenance responsibility** - You manage updates, security patches (you already do this)
4. **No global CDN** - Single server location (can add Cloudflare CDN later if needed)

**For a solo guide operation, benefits far outweigh trade-offs.**

---

## Documentation Updated

All project documentation has been updated to reflect self-hosted architecture:

- ✅ `technical-architecture.md` - Completely rewritten for Astro + self-hosted
- ✅ `project-requirements.md` - Tech stack section updated
- ✅ `README.md` - Overview and costs updated
- ✅ `brand-identity-brief.md` - No changes (design system agnostic)
- ✅ `tour-booking-preliminary-research.md` - No changes (industry research)

---

## Next Steps

1. **Initialize Astro project** with Node.js adapter
2. **Setup Apache virtual host** with reverse proxy configuration
3. **Create PostgreSQL database** and run migrations
4. **Configure systemd service** for Node.js process
5. **Setup SMTP** with Nodemailer
6. **Build core pages** (homepage, tours, booking flow)
7. **Test PayU integration** in sandbox mode
8. **Deploy to production** and monitor

---

**Ready to start building with Astro? Let me know!**
