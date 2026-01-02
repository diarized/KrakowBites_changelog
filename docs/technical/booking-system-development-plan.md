# KrakowBites Development Plan - Phased Implementation

**Project:** KrakowBites - Solo Guide Booking Platform
**Created:** 2026-01-02
**Status:** Planning Phase
**Timeline:** 10 weeks to production launch
**Approach:** Sequential phased development (safer, lower risk)

---

## Executive Summary

Building a custom booking platform for KrakowBites to replace FareHarbor's 6% commission model. The platform enables tourists to browse food and heritage tours in Kraków, check availability, and book directly with automated payment processing via PayU.

**Key Decision**: Self-hosted solution eliminates recurring fees (~12,600 PLN/year savings), provides full data ownership, and supports Polish payment gateways (PayU, BLIK).

**Biggest Risk**: PayU payment integration - addressed by deferring to Phase 3 after booking system is rock-solid.

---

## Current State

### Infrastructure ✅
- **Domain:** krakowbites.com registered
- **Hosting:** krakowbites.stonith.pl (Astro + Node.js + Apache2)
- **Server:** Debian 13 (trzeci.stonith.pl)
- **Status:** Basic infrastructure deployed and validated

### Content Status
- ✅ **Guide bio:** Complete (docs/bio.md)
- ⏳ **Tour descriptions:** Need to be written (6 food + heritage tours)
- ⏳ **Photography:** Placeholders only (AI-generated/stock for MVP)
- ⏳ **Brand assets:** Urban Cartography specs ready, awaiting client approval

### Technical Stack
- **Frontend:** Astro 5.x (SSR mode) + TypeScript + Tailwind CSS
- **Backend:** Node.js 24.12.0 + Astro API endpoints
- **Database:** PostgreSQL (not yet deployed)
- **Payments:** PayU (Poland)
- **Email:** Existing SMTP cluster + Nodemailer
- **Hosting:** Apache2 reverse proxy → Node.js (localhost:3000)

### Not Yet Started
- ❌ PostgreSQL database setup
- ❌ Booking system implementation
- ❌ PayU integration
- ❌ Email automation
- ❌ Admin dashboard

---

## Phase Overview

| Phase | Duration | Focus | Deliverable |
|-------|----------|-------|-------------|
| **Phase 0** | Week 1-2 | Foundation & Content | Infrastructure ready, content prepared |
| **Phase 1** | Week 3-4 | Core Website | Marketing site live (no booking) |
| **Phase 2** | Week 5-6 | Booking System MVP | Calendar + booking (no payments) |
| **Phase 3** | Week 7-8 | Payment Integration | PayU + automated emails |
| **Phase 4** | Week 9-10 | Polish & Launch | Production-ready, launched |

**Total Timeline:** 10 weeks (sequential, safer approach)

**Alternative:** 6-7 weeks if Phases 1 & 2 run in parallel (higher coordination overhead, higher risk)

---

## Phase 0: Foundation & Content Preparation

**Duration:** Week 1-2
**Goal:** All prerequisites ready for development

### Deliverables

#### Infrastructure
- [ ] PostgreSQL 16+ deployed on VPS
- [ ] Database user created with limited permissions
- [ ] Database connection tested from Node.js
- [ ] Backup strategy configured (pg_dump daily)
- [ ] Apache2 configuration verified
- [ ] SSL certificate renewed if needed

#### Brand Assets Finalization
- [ ] Client approval on Urban Cartography direction
- [ ] Logo designed (coordinate-based wordmark)
  - Primary logo (full with tagline)
  - Secondary logo (no tagline)
  - Icon/favicon (compass rose)
- [ ] Tailwind config updated with Urban Cartography colors:
  - Map Paper `#f8f5f0`
  - Route Line (terracotta) `#e76f51`
  - Water Blue (teal) `#2a9d8f`
  - Landmark Brown `#6b4226`
  - Path Grey `#4a4a4a`
- [ ] Font imports configured:
  - Space Grotesk (headings)
  - Inter (body)
  - JetBrains Mono (coordinates/technical)

#### Content Creation
- [ ] Tour descriptions written (6 food + heritage combinations):
  - Shtetl to Street (Lost Jewish Recipes)
  - Ghetto Kitchen (Survival & Resilience)
  - Market to Table (Stary Kleparz + Home Cooking)
  - Shabbes to Sunday (Dual Heritage Feast)
  - Pierogi & Kreplach (Dumpling Trail)
  - Kazimierz After Dark (Vodka & Pickle Trail)
  - Heritage tour combinations documented
- [ ] Tour itineraries outlined (waypoints, stops, timing)
- [ ] Meeting point coordinates collected (lat/long)
- [ ] Pricing finalized (per-person scheduled vs private)
- [ ] Photography organized:
  - Hero images (1 per tour minimum)
  - Gallery images (3-5 per tour)
  - Homepage hero background
  - Guide photo (About page)
  - AI-generated/stock placeholders acceptable for MVP

#### Development Environment
- [ ] Git repository structure finalized
- [ ] GitHub milestones created (0-4)
- [ ] Issue templates configured
- [ ] CLAUDE.md updated with workflow
- [ ] Local development environment tested

### Success Criteria
- ✅ PostgreSQL accepting connections
- ✅ Brand assets approved and ready
- ✅ All tour descriptions compelling and complete
- ✅ Photo placeholders organized by tour
- ✅ Development workflow documented

### GitHub Milestone
`0-foundation`

---

## Phase 1: Core Website (Marketing Site)

**Duration:** Week 3-4
**Goal:** Visitors can browse tours but not book yet (buttons say "Coming Soon")

### Deliverables

#### Pages (Astro Static)
- [ ] **Homepage** (`src/pages/index.astro`)
  - Hero section with background map imagery
  - Featured tours grid (2-3 highlighted tours)
  - Guide introduction preview
  - Social proof placeholder (reviews section)
  - CTA buttons (link to contact for now)

- [ ] **Tour Listing Pages**
  - Food Tours listing (`src/pages/tours/food.astro`)
  - Heritage Tours listing (`src/pages/tours/heritage.astro`)
  - Tour card components with:
    - Hero image
    - Tour name (Route naming: "Route 1: Kazimierz Food Corridor")
    - Coordinates (using JetBrains Mono)
    - Duration, capacity, price
    - "View Details" button

- [ ] **Tour Detail Pages** (`src/pages/tours/[slug].astro`)
  - Hero gallery (5-8 images, placeholder for MVP)
  - Tour title + coordinates
  - Full description (storytelling narrative)
  - Itinerary as waypoints (numbered stops)
  - Meeting point with embedded map (Google Maps or Mapbox)
  - "What's Included" section
  - Reviews placeholder section
  - Booking widget placeholder ("Coming Soon")

- [ ] **About Page** (`src/pages/about.astro`)
  - Joanna's bio (from docs/bio.md)
  - Professional credentials
  - Guide philosophy
  - Languages offered
  - Contact CTA

- [ ] **Contact Page** (`src/pages/contact.astro`)
  - Contact form (name, email, message)
  - Email sent via Nodemailer to guide
  - Phone number (click-to-call on mobile)
  - Use cases highlighted (custom tours, group bookings)

#### Components
- [ ] `Header.astro` - Navigation (desktop + mobile hamburger)
- [ ] `Footer.astro` - Links, contact info, social media
- [ ] `TourCard.astro` - Reusable tour card (for listings)
- [ ] `Button.astro` - Primary/secondary button styles
- [ ] `WaypointItem.astro` - Itinerary waypoint display

#### Styling
- [ ] Tailwind CSS with Urban Cartography colors
- [ ] Typography hierarchy (Space Grotesk headings, Inter body)
- [ ] Responsive breakpoints (mobile-first)
- [ ] Touch-friendly buttons (44x44px minimum)
- [ ] Route line SVG overlays (dashed terracotta lines)

#### SEO & Performance
- [ ] Meta tags (title, description, Open Graph)
- [ ] Schema.org markup (LocalBusiness, TouristAttraction)
- [ ] Sitemap.xml generation
- [ ] robots.txt
- [ ] Image optimization (WebP conversion, lazy loading)
- [ ] Lighthouse score >85

### Success Criteria
- ✅ All pages load <2s
- ✅ Mobile-friendly (Google Mobile-Friendly Test passes)
- ✅ Brand consistency (Urban Cartography colors/typography)
- ✅ Content complete and compelling
- ✅ Contact form delivers emails successfully
- ✅ Navigation works on mobile (hamburger menu)

### GitHub Milestone
`1-core-website`

### Dependencies
- Phase 0 complete (brand assets, content)

---

## Phase 2: Booking System MVP (No Payments)

**Duration:** Week 5-6
**Goal:** Full booking flow EXCEPT payment - everything ready for PayU integration

### Deliverables

#### Database Schema
- [ ] Schema implementation (`schema.sql`):
  ```sql
  -- tours (catalog)
  -- schedules (tour instances with date/time/capacity)
  -- bookings (customer bookings)
  -- payment_transactions (PayU transaction log)
  ```
- [ ] Migrations script (`migrations/001_initial_schema.sql`)
- [ ] Seed data (sample tours and schedules for testing)
- [ ] Indexes for performance:
  - `idx_schedules_date` (availability queries)
  - `idx_bookings_schedule` (capacity calculation)
  - `idx_bookings_status` (filtering)

#### Frontend Components (React Islands)
- [ ] **AvailabilityCalendar.tsx**
  - Display available tour dates (from API)
  - Click date → show available time slots
  - Show remaining capacity ("3 spots left")
  - Disable sold-out dates
  - Mobile-friendly date picker

- [ ] **BookingForm.tsx**
  - Customer details (name, email, phone)
  - Party size selector (1-12, tour max)
  - Dietary restrictions (food tours only)
  - Real-time capacity check
  - Form validation (client + server side)

- [ ] **BookingConfirmation.astro**
  - Booking reference display
  - Tour details summary
  - Payment status (pending for now)
  - Next steps instructions

#### Backend API Endpoints
- [ ] `GET /api/availability/:tour_id`
  - Return available dates/times for tour
  - Calculate remaining capacity per schedule
  - Response: `{ dates: [{ date, time, remaining }] }`

- [ ] `POST /api/book`
  - Accept booking details
  - **Critical:** PostgreSQL row-level locking
  - Capacity validation with race condition prevention:
    ```sql
    BEGIN TRANSACTION;
    SELECT remaining_capacity FROM schedules WHERE id = $1 FOR UPDATE;
    -- Check capacity >= party_size
    -- Create booking (status: pending)
    -- Update remaining_capacity
    COMMIT;
    ```
  - Generate booking reference (e.g., "KB-20260102-A3F9")
  - Return booking reference + redirect URL (placeholder for PayU)

- [ ] `GET /api/bookings/:reference`
  - Lookup booking by reference
  - Return booking details

#### Admin Dashboard (Simple)
- [ ] `src/pages/admin/index.astro` - Dashboard home
  - Protected by basic auth (HTTP Basic for MVP)
  - Overview stats (bookings today, this week, revenue)

- [ ] `src/pages/admin/tours.astro`
  - List all tours
  - Create new tour (form)
  - Edit tour (name, description, price, capacity)

- [ ] `src/pages/admin/schedules.astro`
  - View schedules by tour
  - Create schedule (single or bulk dates)
  - Bulk schedule generator:
    - "Every Saturday in March at 14:00"
    - Generates multiple schedule records

- [ ] `src/pages/admin/bookings.astro`
  - List all bookings
  - Filter by status, date, tour
  - Search by customer email/name
  - Manual booking creation (phone orders)

#### Background Jobs
- [ ] **Payment Timeout Cleanup** (Node.js cron)
  - Run every 5 minutes
  - Find bookings with status `payment_initiated` older than 30min
  - Delete booking + release capacity
  - Log cleanup action

#### Testing
- [ ] **Load Testing:**
  - Simulate 10+ concurrent booking requests
  - Verify no overbooking occurs
  - Tools: Apache Bench or k6

- [ ] **Edge Cases:**
  - Last spot taken (race condition)
  - Payment timeout cleanup works
  - Capacity correctly released
  - Booking reference uniqueness

### Success Criteria
- ✅ Zero overbooking under load (10+ concurrent users)
- ✅ Capacity locks work correctly (PostgreSQL FOR UPDATE)
- ✅ Timeout cleanup releases spots within 5min
- ✅ Admin can create tours and schedules
- ✅ Admin can view all bookings
- ✅ Booking form validates correctly
- ✅ Calendar shows accurate availability

### GitHub Milestone
`2-booking-mvp`

### Dependencies
- Phase 0 complete (PostgreSQL deployed)
- Phase 1 complete (website structure exists)

### Risks & Mitigation
- **Risk:** Race condition overbooking
  - **Mitigation:** PostgreSQL `SELECT FOR UPDATE`, transaction isolation `SERIALIZABLE`
- **Risk:** Timeout cleanup failures
  - **Mitigation:** Logging, monitoring, manual fallback procedures
- **Risk:** Database connection pool exhaustion
  - **Mitigation:** Connection pooling (max 20 connections), timeout configuration

---

## Phase 3: Payment Integration (PayU + Email)

**Duration:** Week 7-8
**Goal:** End-to-end payment processing, automated confirmation emails

### Deliverables

#### PayU Sandbox Setup
- [ ] Create PayU test merchant account
- [ ] Obtain sandbox credentials:
  - POS ID
  - Client ID
  - Client Secret
  - Signature key (webhook validation)
- [ ] Configure sandbox environment variables

#### Payment Flow Implementation
- [ ] **Initiate Payment** (`POST /api/payment/initiate`)
  - Accept booking_reference
  - Create PayU order:
    ```json
    {
      "merchantPosId": "...",
      "description": "Route 1: Kazimierz Food Corridor - 2026-01-15",
      "customerIp": "...",
      "totalAmount": "42000", // 420.00 PLN in grosze
      "currencyCode": "PLN",
      "buyer": { "email": "...", "firstName": "...", ... },
      "continueUrl": "https://krakowbites.com/booking/confirm/...",
      "notifyUrl": "https://krakowbites.com/api/payu-webhook"
    }
    ```
  - Update booking: `status = payment_initiated`, store `payu_order_id`
  - Redirect customer to PayU payment page
  - Log transaction in `payment_transactions` table

- [ ] **Return URL Handler** (`src/pages/booking/confirm/[reference].astro`)
  - Customer redirected here after payment
  - Show "Processing payment..." spinner
  - Poll booking status (webhook may arrive before redirect)
  - Display confirmation once `status = confirmed`

- [ ] **Webhook Handler** (`POST /api/payu-webhook`)
  - **Critical:** Validate signature
    ```typescript
    const signature = request.headers['OpenPayu-Signature'];
    const isValid = validatePayUSignature(signature, requestBody, signatureKey);
    if (!isValid) return 401; // Reject
    ```
  - Parse webhook payload
  - Find booking by `payu_order_id`
  - Update booking status based on PayU status:
    - `COMPLETED` → `confirmed`
    - `CANCELED` → `cancelled`, release capacity
    - `PENDING` → keep as `payment_initiated`
  - Log full webhook payload in `payment_transactions`
  - Send confirmation email (if status = confirmed)
  - Return `200 OK` to PayU (required for webhook acknowledgment)

#### Email Automation
- [ ] Configure Nodemailer with existing SMTP
- [ ] Email templates (HTML + plain text fallback):

  **1. Booking Confirmation Email:**
  ```
  Subject: Route Confirmed: Your Kazimierz Journey on [Date]

  Hi [Name],

  Your booking is confirmed!

  Tour: Route 1: Kazimierz Food Corridor
  Date: Saturday, January 15, 2026
  Time: 14:00 (2:00 PM)
  Meeting Point: Main Market Square, Cloth Hall south entrance
  Coordinates: 50.0619°N, 19.9369°E
  Party Size: 2 people
  Total Paid: 420 PLN

  Booking Reference: KB-20260115-A3F9

  What to Bring:
  - Comfortable walking shoes
  - Appetite!
  - Weather-appropriate clothing

  See you soon!
  Joanna Wylon
  KrakowBites
  ```

  **2. Payment Receipt Email:**
  - PayU transaction ID
  - Amount paid
  - Payment method used
  - Date/time of payment

  **3. Cancellation Email** (future, for Phase 5)

- [ ] Email deliverability testing:
  - Test with Gmail, Outlook, Apple Mail
  - Verify not in spam folder
  - Check formatting on mobile

#### Database Updates
- [ ] Add `payment_transactions` table:
  ```sql
  CREATE TABLE payment_transactions (
    id SERIAL PRIMARY KEY,
    booking_id INTEGER REFERENCES bookings(id),
    payu_order_id VARCHAR(100) NOT NULL,
    transaction_type VARCHAR(50), -- payment, refund
    amount_pln DECIMAL(10,2),
    status VARCHAR(50), -- pending, completed, failed
    payu_response JSONB, -- Full webhook payload
    processed_at TIMESTAMP DEFAULT NOW()
  );
  ```
- [ ] Update `bookings` table:
  - Add `payu_order_id` column
  - Add `paid_at` timestamp
  - Ensure `status` enum includes payment states

#### Testing (Sandbox)
- [ ] End-to-end payment flow:
  - Create booking
  - Redirect to PayU
  - Complete test payment (card: 4444333322221111)
  - Webhook received and processed
  - Booking confirmed
  - Email sent
- [ ] Payment failures:
  - Declined card
  - Cancelled by user
  - Timeout scenarios
- [ ] Webhook security:
  - Invalid signature rejected
  - Replay attack prevention (idempotency)
  - Malformed payload handling

#### Production Preparation
- [ ] Obtain PayU production credentials
- [ ] Update environment variables (production vs sandbox)
- [ ] Configure production webhook URL with PayU
- [ ] Test with real small-amount transaction (5 PLN)
- [ ] Monitoring setup:
  - Webhook failure alerts
  - Payment timeout alerts
  - Email delivery failures

### Success Criteria
- ✅ 100% webhook processing rate (no missed payments)
- ✅ Emails delivered within 2 minutes of payment
- ✅ Zero payment security vulnerabilities (signature validation works)
- ✅ Failed payments don't leak capacity
- ✅ Webhook replay attacks rejected
- ✅ Production test transaction successful

### GitHub Milestone
`3-payments`

### Dependencies
- Phase 2 complete (booking system working)
- PayU merchant account approved

### Risks & Mitigation
- **Risk:** Webhook failures (PayU down, network issues)
  - **Mitigation:** Retry logic, manual reconciliation script, alerting
- **Risk:** Payment vs booking status mismatch
  - **Mitigation:** Transaction logging, reconciliation reports
- **Risk:** Email deliverability (spam filters)
  - **Mitigation:** SPF/DKIM setup, transactional email service (SendGrid fallback)
- **Risk:** Webhook signature validation bypass
  - **Mitigation:** Code review, security testing, strict validation

---

## Phase 4: Polish & Production Launch

**Duration:** Week 9-10
**Goal:** Production-ready, launched, monitored

### Deliverables

#### Performance Optimization
- [ ] **Image Optimization:**
  - Convert all images to WebP
  - Generate multiple sizes (responsive images)
  - Implement lazy loading (below fold)
  - Compress images (<200KB each)

- [ ] **Caching Strategy:**
  - Static pages: `Cache-Control: public, max-age=3600`
  - Tour detail pages: `stale-while-revalidate=86400`
  - API endpoints: No caching (always fresh)

- [ ] **Lighthouse Audit:**
  - Performance: >90
  - Accessibility: >95
  - Best Practices: >90
  - SEO: >95

- [ ] **Bundle Optimization:**
  - Tree-shaking unused code
  - Minimize JavaScript shipped (<50KB total)
  - Critical CSS inlined

#### SEO Finalization
- [ ] Sitemap.xml automated generation
- [ ] robots.txt configured
- [ ] Google Search Console setup + verification
- [ ] Submit sitemap to Google
- [ ] Schema.org markup complete:
  - `LocalBusiness` (KrakowBites)
  - `TouristAttraction` (tours)
  - `Event` (scheduled tour instances)
  - `Person` (Joanna Wylon)

#### Analytics & Monitoring
- [ ] **Analytics Setup:**
  - Plausible Analytics (self-hosted) or Umami
  - Track events:
    - Page views
    - Tour detail views
    - Booking initiated
    - Booking completed
    - Booking funnel drop-off points

- [ ] **Error Monitoring:**
  - Sentry.io or self-hosted alternative
  - Track:
    - JavaScript errors
    - API endpoint failures
    - Payment webhook failures
    - Email delivery failures

- [ ] **Uptime Monitoring:**
  - UptimeRobot or StatusCake (free tier)
  - Monitor `/api/health` endpoint
  - Alert on downtime (email/SMS)

- [ ] **Health Check Endpoint** (`GET /api/health`)
  ```json
  {
    "status": "healthy",
    "timestamp": "2026-01-15T10:30:00Z",
    "database": "connected",
    "uptime": 3600
  }
  ```

#### Documentation
- [ ] **Admin User Guide:**
  - How to create tours
  - How to create schedules (bulk)
  - How to view/manage bookings
  - How to handle payment issues
  - Screenshots of admin interface

- [ ] **Operational Runbook:**
  - Payment webhook failure recovery
  - Database backup/restore
  - Customer support: refund requests
  - Overbooking resolution (manual)
  - Email delivery issues

- [ ] **Backup Procedures:**
  - Automated daily PostgreSQL dumps
  - Test restore procedure (monthly)
  - Backup retention: 30 days
  - Off-site backup location

#### Domain & SSL
- [ ] DNS configuration:
  - Point krakowbites.com → krakowbites.stonith.pl IP
  - WWW subdomain redirect
  - DNS propagation verified

- [ ] SSL Certificate:
  - Let's Encrypt via certbot
  - Auto-renewal configured (systemd timer)
  - HTTPS enforced (HTTP redirects)
  - Certificate expiry monitoring

#### Launch Preparation
- [ ] **Soft Launch (Week 9):**
  - Enable 1-2 tours only
  - Limited schedule (next 2 weeks)
  - Monitor all systems closely
  - Test with real bookings (friends/family discount)
  - Verify:
    - Payment flow works end-to-end
    - Emails delivered correctly
    - No overbooking occurs
    - Admin can manage schedules
    - Customer support flow works

- [ ] **Feedback Collection:**
  - Post-booking survey (simple email)
  - Monitor customer questions
  - Track booking funnel drop-offs
  - Identify UX friction points

- [ ] **Full Launch (Week 10):**
  - Enable all 6 food + heritage tours
  - Publish schedules for next 60 days
  - Marketing activation:
    - Update Google Business Profile
    - Instagram announcement
    - Email existing customer list (if available)
  - Monitor first 48 hours closely

#### Final Quality Checks
- [ ] **Security Audit:**
  - SQL injection prevention verified (parameterized queries)
  - CSRF protection on forms
  - XSS prevention (Astro auto-escaping)
  - Webhook signature validation tested
  - HTTPS enforced everywhere
  - Admin endpoints authenticated
  - Sensitive data encrypted (payment logs)

- [ ] **Accessibility Audit:**
  - Screen reader testing (NVDA/JAWS)
  - Keyboard navigation works (Tab, Enter, Escape)
  - Color contrast validated (WCAG AA)
  - Touch targets minimum 44x44px
  - Focus states visible
  - Semantic HTML used
  - ARIA labels present
  - Forms have labels
  - Images have alt text

- [ ] **Mobile Testing:**
  - Real device testing (iOS Safari, Android Chrome)
  - Touch interactions smooth
  - Forms usable on mobile
  - Calendar picker mobile-friendly
  - Booking flow complete on mobile
  - Payment redirect works on mobile

### Success Criteria
- ✅ First real booking processed successfully
- ✅ Zero critical bugs in production
- ✅ Email deliverability >95%
- ✅ Mobile UX smooth (no scrolling issues, touch targets correct)
- ✅ Lighthouse scores meet targets
- ✅ Client can manage tours independently (no developer intervention)
- ✅ Monitoring alerts working
- ✅ Backup/restore tested successfully

### GitHub Milestone
`4-launch`

### Dependencies
- Phase 3 complete (payments working)
- All content finalized
- Photography ready (or acceptable placeholders)

---

## Post-Launch Phases (Future)

### Phase 5: Cancellation & Refunds (Month 2)
**Goal:** Self-service cancellation with automated refunds

**Deliverables:**
- Customer cancellation portal (lookup by email + reference)
- Cancellation policy enforcement (48h cutoff)
- PayU Refund API integration
- Automated refund processing
- Capacity release on cancellation
- Cancellation email notifications

**Estimated Effort:** 20-30 hours

---

### Phase 6: Enhanced Admin & Analytics (Month 3)
**Goal:** Better admin tools and business intelligence

**Deliverables:**
- Bulk schedule creation (recurring tours)
- Revenue dashboard (bookings, gross, net after refunds)
- Customer database (repeat customers, email lists)
- Email template customization (admin UI)
- Export bookings to CSV
- Financial reconciliation reports

**Estimated Effort:** 30-40 hours

---

### Phase 7: Advanced Features (Month 4+)
**Goal:** Competitive differentiation, scale features

**Deliverables:**
- Multi-language support (EN, PL, DE)
- PDF ticket generation (QR codes)
- SMS notifications (booking reminders)
- Group booking requests (>max capacity)
- Gift certificates
- Mobile payment terminal integration (pay on arrival)
- Seasonal pricing (high/low season rates)

**Estimated Effort:** 60-80 hours

---

## Risk Management

### Critical Risks

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| **Overbooking** | Critical | Medium | PostgreSQL row locking, load testing, monitoring |
| **Payment webhook failure** | High | Medium | Retry logic, manual reconciliation, alerts |
| **Payment timeout capacity leak** | High | Medium | Background cleanup job, monitoring |
| **PayU downtime during booking** | High | Low | Show error message, retry later, customer support |
| **Email deliverability (spam)** | Medium | Medium | SPF/DKIM, transactional email service |
| **Database corruption** | Critical | Low | Daily backups, tested restore, replication |

### Medium Risks

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| **Time zone confusion** | Medium | Medium | Always store Europe/Warsaw, display clearly |
| **Mobile UX issues** | Medium | Medium | Real device testing, user feedback |
| **Slow page load** | Low | Medium | Image optimization, caching, CDN |
| **Concurrent cancellations** | Low | Low | Pessimistic locking, transaction isolation |
| **SSL certificate expiry** | Medium | Low | Auto-renewal, expiry monitoring |

---

## Success Metrics

### Phase Completion Metrics

**Phase 1 (Website):**
- All pages live and functional
- Content complete
- Mobile-friendly test passes
- Lighthouse score >85

**Phase 2 (Booking MVP):**
- Zero overbooking under load test
- Admin can create tours/schedules
- Capacity locks work correctly

**Phase 3 (Payments):**
- 100% webhook processing
- Emails delivered <2min
- Zero security vulnerabilities
- Production test successful

**Phase 4 (Launch):**
- First real booking successful
- Monitoring active
- Client managing independently

### Business Metrics (Month 1-3)

- **Cost Savings:** 12,000+ PLN/year vs FareHarbor
- **Booking Conversion:** ≥3% (industry baseline)
- **Email Deliverability:** >95%
- **System Uptime:** >99.5%
- **Customer Satisfaction:** >4.5/5 (post-tour survey)
- **Mobile Traffic:** 60-70% (industry norm)
- **Zero Overbooking:** No operational disasters

---

## Timeline & Dependencies

```
Week 1-2:  Phase 0 (Foundation)
           │
           ├─ PostgreSQL setup
           ├─ Brand finalization
           └─ Content writing
           │
Week 3-4:  Phase 1 (Website) ◄── depends on Phase 0
           │
           ├─ Homepage
           ├─ Tour pages
           └─ Contact
           │
Week 5-6:  Phase 2 (Booking MVP) ◄── depends on Phase 0, 1
           │
           ├─ Database schema
           ├─ Calendar component
           ├─ Booking form
           └─ Admin dashboard
           │
Week 7-8:  Phase 3 (Payments) ◄── depends on Phase 2
           │
           ├─ PayU integration
           ├─ Webhook handler
           └─ Email automation
           │
Week 9-10: Phase 4 (Launch) ◄── depends on Phase 3
           │
           ├─ Soft launch (1-2 tours)
           ├─ Monitoring
           └─ Full launch
```

**Critical Path:** Phase 0 → 1 → 2 → 3 → 4 (sequential, cannot parallelize safely)

---

## Alternative Approach: Parallel Tracks (Faster)

**Timeline:** 6-7 weeks instead of 10

**Structure:**
- **Track A:** Website (Phases 0 + 1) - Weeks 1-3
- **Track B:** Booking System (Phase 2) - Weeks 2-4 (starts during Phase 1)
- **Track C:** Payments (Phase 3) - Weeks 5-6 (after Track B complete)
- **Track D:** Launch (Phase 4) - Week 7

**Pros:**
- 30% faster to market
- Booking system developed while website content finalized

**Cons:**
- Higher coordination overhead
- Risk of rework if website structure changes
- Harder to test integration
- Requires more developer focus

**Recommendation:** Only use parallel approach if timeline is critical (e.g., high season starting soon).

---

## Development Workflow

### GitHub Structure
- **Milestones:** `0-foundation`, `1-core-website`, `2-booking-mvp`, `3-payments`, `4-launch`
- **Labels:** `phase-0`, `phase-1`, `backend`, `frontend`, `database`, `payments`, `critical`
- **Issues:** Atomic deliverables from each phase (using templates)
- **Branches:** `main` (production), `develop` (integration), feature branches per issue

### Issue Workflow
1. Issue created with deliverable template
2. Assigned to milestone
3. Developer creates feature branch
4. Implementation + testing
5. PR to `develop` branch
6. Review + merge
7. Close issue

### CLAUDE.md Updates
- Document workflow for working with issues
- Reference GitHub milestones for phase tracking
- Link to Wiki for architecture decisions

### Wiki Pages (to create)
- Architecture Decision Records (ADRs):
  - Why PostgreSQL row locking for capacity
  - Why PayU webhook security approach
  - Why sequential phases vs parallel
- Technical specs:
  - Booking system flow diagrams
  - Payment integration sequence diagrams
  - Database schema documentation
- Operational guides:
  - Deployment procedures
  - Backup/restore procedures
  - Customer support runbook

---

## Questions to Resolve Before Phase 0

### Brand & Content
- [ ] Urban Cartography brand approved by client?
- [ ] Logo design commissioned or DIY?
- [ ] Photography plan: All placeholders for MVP? Or hire photographer?
- [ ] Tour pricing finalized (per-person scheduled vs private)?

### Tour Catalog
- [ ] Launch with all 6 food tours + heritage tours?
- [ ] Or subset (e.g., 2-3 tours for soft launch)?
- [ ] Heritage tour pricing structure (same as food, or different)?

### Operational
- [ ] Cancellation policy details:
  - Full refund cutoff: 24h? 48h? 72h?
  - Partial refund percentage: 50%?
  - Weather cancellation: Reschedule or refund?
- [ ] Maximum advance booking: 3 months? 6 months? 1 year?
- [ ] Minimum party size to run tour? (e.g., cancel if <4 people?)

### Technical
- [ ] PayU merchant account: Already registered? Or need to create?
- [ ] Email service: Use existing SMTP only? Or add SendGrid for reliability?
- [ ] Analytics: Plausible (paid) or Umami (free)?
- [ ] Timeline preference: 10 weeks sequential? Or push for 6-7 weeks parallel?

---

## Next Steps

Once this plan is approved:

1. **Create GitHub Milestones** (0-4)
2. **Generate GitHub Issues** using deliverable templates
3. **Update CLAUDE.md** with workflow documentation
4. **Create Wiki pages** for architecture decisions
5. **Begin Phase 0** (infrastructure + content)

---

## Document Metadata

**Version:** 1.0
**Created:** 2026-01-02
**Author:** Development Team
**Status:** Planning (awaiting approval)
**Next Review:** After Phase 0 completion
**Related Documents:**
- `/docs/technical/booking-system-requirements.md`
- `/docs/brand-alternatives/urban-cartography-specs.md`
- `/docs/bio.md`
- `/specs/001-infrastructure-setup/plan.md`
