# Booking System Requirements - FareHarbor Alternative

**Project:** KrakowBites Custom Booking Component
**Created:** 2026-01-02
**Purpose:** Self-hosted booking system to replace FareHarbor plugin
**Target Market:** Small city tour websites (single operator, multiple tours)

---

## Executive Summary

Building a lightweight Astro-based booking component with PostgreSQL backend to replace FareHarbor's 6% commission model. Focus on essential features: calendar, booking, capacity management, PayU payment integration, and mobile responsiveness.

**Key Decision:** Self-hosted component vs SaaS = no recurring fees, full data ownership, Polish payment gateway support.

---

## FareHarbor Competitive Analysis

### Functionality Overview

**Core Features:**
- Embedded availability calendars (responsive)
- Lightframe booking overlay (single-page checkout)
- WordPress plugin with shortcodes
- Automated emails and payment processing
- Flexible pricing structures
- Affiliate management
- Group booking handling
- Real-time availability sync

**Integration Capabilities:**
- REST API with webhooks
- 250+ OTA integrations (Google, Airbnb, Booking.com)
- WordPress plugin (9K active installations)
- Custom button/link generators

**Technical Stack:**
- REST API architecture
- Webhook support
- Authentication via API keys
- GitHub public documentation

### Pricing Model (2025)

**Commission Structure:**
- 2% on API bookings
- Up to 6% on direct bookings
- Volume-based discounts available
- No monthly subscription for booking system
- Merchant service fees (additional)

**Website Builder:**
- $5,000/year (or $499/month) - NEW in 2025
- Previously free, now paid service

**User Feedback:**
- ✅ Intuitive interface, responsive support
- ✅ Strong integration capabilities
- ❌ High fees cause cart abandonment
- ❌ Steep learning curve
- ❌ Cumbersome rate management

### Competitive Alternatives

| Platform | Booking Fee | Notes |
|----------|-------------|-------|
| FareHarbor | 6% | Industry standard features |
| TicketingHub | 3% | 50% lower fees |
| Bókun | 1-1.5% | 0% on Viator/offline |
| Bookeo | 0% | No customer fees |
| Peek Pro | 6% | Enterprise-focused |

**Conclusion:** FareHarbor charges premium rates (6%) but offers comprehensive feature set. For small operators, simpler alternatives exist at 1-3% rates.

### What We're NOT Building

**Excluded from Scope:**
- Multi-tenant SaaS architecture
- API/webhook infrastructure for third parties
- OTA integrations (Google, Airbnb, etc.)
- Affiliate network management
- Website builder service
- Advanced pricing tiers/volume discounts
- Reseller/partner features
- Multi-language support (initially)
- PDF ticket generation
- Reporting/analytics dashboard

---

## KrakowBites Requirements

### 1. Payment Flow

**Primary Method:** PayU Integration
- Payment gateway: PayU (Polish market leader)
- Supported methods:
  - Credit/debit cards (Visa, Mastercard, etc.)
  - BLIK (Polish instant payment system)
- Payment timing: During booking process (pay-to-confirm)

**Future Consideration:**
- Mobile payment terminal at tour start
- Pay-on-arrival option

**Technical Implications:**
- Async payment confirmation via PayU webhooks
- Booking status flow: `pending` → `payment_initiated` → `paid` → `confirmed`
- Timeout handling: Release capacity if payment not completed (15-30min window)
- BLIK timeout: 2 minutes (instant payment)
- Card payment timeout: Up to 30 minutes (3DS verification, etc.)

**Critical Requirements:**
- PayU webhook endpoint for payment confirmation
- Secure webhook signature validation
- Payment reconciliation logging
- Failed payment cleanup (release held capacity)

### 2. Booking Confirmation

**Method:** Instant auto-confirmation after successful payment

**Flow:**
1. Customer selects tour + date + party size
2. System checks capacity availability
3. Capacity temporarily held (pending payment)
4. Customer redirected to PayU
5. PayU processes payment
6. PayU webhook confirms payment success
7. System confirms booking + sends email
8. Capacity permanently allocated

**Response Time:** 30 seconds - 2 minutes (webhook dependent)

**Customer Communication:**
- Immediate: "Payment processing, confirmation email coming soon"
- Email: Booking confirmation with tour details, date, meeting point
- No manual admin approval required

### 3. Cancellation Policy

**Method:** Self-service cancellation with automated refund

**Policy Rules (TBD - Examples):**
- Full refund: Cancellation >48 hours before tour
- 50% refund: Cancellation 24-48 hours before tour
- No refund: Cancellation <24 hours before tour
- Operator cancellation: Full refund always

**Technical Implementation:**
- Customer portal: "Cancel Booking" button
- Automated refund via PayU Refund API
- Email notification: Cancellation confirmation + refund details
- Capacity release: Immediately return spots to availability

**⚠️ Complexity Warning:**

This is MORE complex than FareHarbor's typical setup:
- Refund API integration required
- Edge cases:
  - Customer cancels, gets refund, then disputes original charge
  - Partial refunds based on time windows
  - Failed refund processing (insufficient merchant balance)
  - Refund vs. original payment currency mismatch
- Financial reconciliation:
  - Track gross bookings vs. net revenue
  - Refund reporting for accounting
  - PayU transaction fees (non-refundable)

**Alternative Approach (Simpler):**
- Contact-based cancellation: "Email us to cancel"
- Manual refund processing
- Defer self-service until proven demand

### 4. Multi-Tour Architecture

**Operator Model:** Single tour operator with multiple tour offerings

**Simplified by:**
- Single database instance
- Single PayU merchant account
- Single branding/email templates
- No tenant isolation required
- Shared admin dashboard

**Database Structure:**
```
Single PostgreSQL database
├── tours (multiple records)
├── schedules (linked to tours)
├── bookings (linked to schedules)
└── customers (shared across tours)
```

**Example Tours:**
- "Old Town Food Walk" (3 hours, max 12 people)
- "Jewish Quarter History Tour" (2.5 hours, max 15 people)
- "Vodka Tasting Experience" (2 hours, max 8 people)

### 5. Capacity Management

**⚠️ CRITICAL: Edge Case Handling IS Required**

**Scenario:**
- Tour: 10 max capacity, 9 booked, 1 spot remaining
- 3 customers simultaneously click "Book 2 people"
- Without proper locking: All 3 enter payment flow
- Result: Overbooked by 5 people = operational disaster

**Minimum Viable Solution:**

**Database Level:**
```sql
BEGIN TRANSACTION;

-- Lock the schedule row
SELECT remaining_capacity
FROM schedules
WHERE id = $schedule_id
FOR UPDATE;

-- Check capacity
IF remaining_capacity >= $party_size THEN
  -- Create pending booking
  INSERT INTO bookings (...) VALUES (...);
  -- Update capacity
  UPDATE schedules SET remaining_capacity = remaining_capacity - $party_size;
  COMMIT;
ELSE
  ROLLBACK;
  RETURN 'Sold out';
END IF;
```

**Application Level:**
- Transaction isolation: `SERIALIZABLE` or `REPEATABLE READ`
- Row-level locking: `SELECT FOR UPDATE` on schedule
- Timeout: Release lock after payment timeout (30min)
- User feedback: "Checking availability..." → "Sorry, sold out" or "Proceed to payment"

**Payment Timeout Handling:**
- Background job: Every 5 minutes, find bookings with status `payment_initiated` older than 30min
- Action: Delete booking + release capacity + log event

**Race Condition Prevention:**
- Lock acquired BEFORE showing payment form
- Lock held DURING payment initiation
- Lock released AFTER payment confirmation OR timeout

**Testing:**
- Simulate concurrent bookings (load testing)
- Verify no overbooking under stress
- Check timeout cleanup works correctly

---

## Technical Architecture

### Stack

**Frontend:**
- Astro (static site generator + server endpoints)
- HTML/CSS (minimal JavaScript)
- Responsive design (mobile-first)

**Backend:**
- Astro API routes (server-side TypeScript)
- PostgreSQL (local, self-hosted)
- Node.js runtime

**Integrations:**
- PayU REST API (payments + refunds)
- Email service (SendGrid/Mailgun/nodemailer)

### Component Structure

```
KrakowBites Astro Site
├── src/
│   ├── components/
│   │   ├── AvailabilityCalendar.astro
│   │   ├── BookingForm.astro
│   │   ├── BookingConfirmation.astro
│   │   └── AdminDashboard.astro
│   ├── pages/
│   │   ├── tours/[slug].astro
│   │   ├── book/[schedule_id].astro
│   │   └── api/
│   │       ├── availability.ts
│   │       ├── book.ts
│   │       ├── payu-webhook.ts
│   │       ├── cancel.ts
│   │       └── admin/
│   │           ├── tours.ts
│   │           └── schedules.ts
│   └── lib/
│       ├── db.ts (PostgreSQL client)
│       ├── payu.ts (PayU SDK wrapper)
│       └── email.ts (Email sender)
└── PostgreSQL Database
    ├── tours
    ├── schedules
    ├── bookings
    ├── customers
    └── payments (transaction log)
```

### Database Schema (Simplified)

```sql
-- Tours catalog
CREATE TABLE tours (
  id SERIAL PRIMARY KEY,
  slug VARCHAR(100) UNIQUE NOT NULL,
  name VARCHAR(200) NOT NULL,
  description TEXT,
  duration_minutes INTEGER NOT NULL,
  max_capacity INTEGER NOT NULL,
  base_price_pln DECIMAL(10,2) NOT NULL,
  meeting_point TEXT,
  active BOOLEAN DEFAULT true,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Scheduled tour instances
CREATE TABLE schedules (
  id SERIAL PRIMARY KEY,
  tour_id INTEGER REFERENCES tours(id),
  tour_date DATE NOT NULL,
  tour_time TIME NOT NULL,
  max_capacity INTEGER NOT NULL, -- Can override tour default
  remaining_capacity INTEGER NOT NULL,
  price_override_pln DECIMAL(10,2), -- Optional price override
  status VARCHAR(50) DEFAULT 'active', -- active, cancelled, full
  created_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(tour_id, tour_date, tour_time)
);

-- Customer bookings
CREATE TABLE bookings (
  id SERIAL PRIMARY KEY,
  schedule_id INTEGER REFERENCES schedules(id),
  booking_reference VARCHAR(20) UNIQUE NOT NULL, -- e.g., KB-20260102-A3F9
  customer_name VARCHAR(200) NOT NULL,
  customer_email VARCHAR(200) NOT NULL,
  customer_phone VARCHAR(50),
  party_size INTEGER NOT NULL,
  total_price_pln DECIMAL(10,2) NOT NULL,
  status VARCHAR(50) NOT NULL, -- pending, payment_initiated, paid, confirmed, cancelled, refunded
  payment_id VARCHAR(100), -- PayU transaction ID
  payu_order_id VARCHAR(100),
  refund_amount_pln DECIMAL(10,2),
  refund_processed_at TIMESTAMP,
  cancellation_reason TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  confirmed_at TIMESTAMP,
  cancelled_at TIMESTAMP
);

-- Payment transaction log
CREATE TABLE payment_transactions (
  id SERIAL PRIMARY KEY,
  booking_id INTEGER REFERENCES bookings(id),
  payu_order_id VARCHAR(100) NOT NULL,
  transaction_type VARCHAR(50) NOT NULL, -- payment, refund
  amount_pln DECIMAL(10,2) NOT NULL,
  status VARCHAR(50) NOT NULL, -- pending, completed, failed
  payu_response JSONB, -- Store full PayU webhook payload
  processed_at TIMESTAMP DEFAULT NOW()
);

-- Indexes for performance
CREATE INDEX idx_schedules_date ON schedules(tour_date);
CREATE INDEX idx_bookings_schedule ON bookings(schedule_id);
CREATE INDEX idx_bookings_email ON bookings(customer_email);
CREATE INDEX idx_bookings_status ON bookings(status);
```

### PayU Integration Flow

**1. Booking Initiation:**
```typescript
// API endpoint: /api/book
POST /api/book
{
  schedule_id: 123,
  customer: { name, email, phone },
  party_size: 2
}

Response:
{
  booking_reference: "KB-20260102-A3F9",
  payu_redirect_url: "https://secure.payu.com/...",
  expires_at: "2026-01-02T14:30:00Z"
}
```

**2. PayU Payment:**
- Customer redirected to PayU
- Selects payment method (card/BLIK)
- Completes payment
- PayU redirects back to site

**3. Webhook Confirmation:**
```typescript
// API endpoint: /api/payu-webhook
POST /api/payu-webhook
Headers: {
  "OpenPayu-Signature": "sender=checkout;signature=abc123..."
}
Body: {
  order: {
    orderId: "PAYU123",
    status: "COMPLETED",
    ...
  }
}

Actions:
1. Validate webhook signature
2. Find booking by payu_order_id
3. Update booking status: paid → confirmed
4. Send confirmation email
5. Log transaction
6. Return 200 OK to PayU
```

**4. Refund Processing:**
```typescript
// API endpoint: /api/cancel
POST /api/cancel
{
  booking_reference: "KB-20260102-A3F9",
  reason: "Customer request"
}

Actions:
1. Validate booking exists and is refundable
2. Calculate refund amount (based on cancellation policy)
3. Call PayU Refund API
4. Update booking: status → refunded
5. Release capacity back to schedule
6. Send cancellation email
7. Log refund transaction
```

---

## Feature Prioritization

### Phase 1 - MVP (Must-Have)

**Core Booking Flow:**
- [ ] Availability calendar (show available tour dates)
- [ ] Capacity calculation (max - booked = remaining)
- [ ] Booking form (name, email, phone, party size)
- [ ] PayU payment integration
- [ ] Webhook handler (payment confirmation)
- [ ] Email confirmation (booking details)
- [ ] Database schema implementation
- [ ] Capacity locking (race condition prevention)

**Admin Functions:**
- [ ] Add/edit tours (name, description, price, capacity)
- [ ] Create schedules (dates, times)
- [ ] View bookings (list, search, filter)
- [ ] Manual booking creation (phone orders)

**Estimated Effort:** 40-60 hours with AI assistance

### Phase 2 - Enhanced (Should-Have)

**Customer Features:**
- [ ] Self-service cancellation
- [ ] Automated refund processing
- [ ] Booking lookup (by email/reference)
- [ ] Customer portal (view my bookings)

**Admin Features:**
- [ ] Bulk schedule creation (recurring tours)
- [ ] Capacity override (block dates, reduce capacity)
- [ ] Email template customization
- [ ] Basic reporting (bookings, revenue)

**Estimated Effort:** 30-40 hours

### Phase 3 - Advanced (Nice-to-Have)

**Customer Experience:**
- [ ] Multi-language support (EN, PL, DE)
- [ ] PDF ticket generation
- [ ] SMS notifications
- [ ] Group booking requests (>max capacity)

**Business Intelligence:**
- [ ] Revenue dashboard
- [ ] Occupancy analytics
- [ ] Customer retention metrics
- [ ] Seasonal trends analysis

**Estimated Effort:** 40-60 hours

---

## Risk Assessment

### High Priority Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| Overbooking due to race conditions | Critical | PostgreSQL row locking, transaction isolation, load testing |
| PayU webhook replay attacks | High | Signature validation, idempotency checks |
| Payment timeout capacity leaks | High | Background cleanup job, monitoring |
| Email deliverability (spam) | Medium | Use transactional email service (SendGrid) |
| Refund API failures | Medium | Retry logic, manual fallback, alerts |

### Medium Priority Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| Time zone confusion | Medium | Store all times in Europe/Warsaw, display clearly |
| Mobile UX issues | Medium | Responsive design testing, real device validation |
| Database backup failures | Medium | Automated backups, test restore process |
| Concurrent booking cancellations | Low | Pessimistic locking on cancellation |

---

## Cost Comparison

### FareHarbor Model (6% commission)

**Example: 100 bookings/month at 200 PLN average:**
- Gross revenue: 20,000 PLN
- FareHarbor fee (6%): **1,200 PLN/month**
- Annual cost: **14,400 PLN**

**Plus:**
- Website builder: 5,000 PLN/year (if used)
- Total annual: **19,400 PLN**

### Custom Solution (Self-Hosted)

**One-Time Development:**
- Phase 1 MVP: 50 hours × AI-assisted rate
- Assuming break-even after first booking (AI productivity)

**Monthly Operating Costs:**
- Server hosting: 50-100 PLN/month (VPS)
- Email service: 0-50 PLN/month (SendGrid free tier)
- PayU transaction fees: ~2-3% (unavoidable)
- Maintenance: Minimal (simple stack)

**Annual cost: 600-1,800 PLN vs. FareHarbor's 14,400 PLN**

**Savings: ~12,600 PLN/year**

**Break-even: After first month of operation** (assuming 100 bookings)

---

## Development Recommendations

### Start Simple

**Day 1 Build:**
1. Static calendar → Click date → Show tour times
2. Form: Name, Email, Party Size
3. PayU payment redirect
4. Webhook: Confirm booking
5. Email: Send confirmation

**Defer:**
- Admin dashboard (use SQL scripts initially)
- Cancellation (handle via email for first month)
- Advanced features (wait for user feedback)

### Validate Early

**Test with Real Bookings:**
- Soft launch with 1 tour
- Monitor PayU webhooks closely
- Verify email deliverability
- Check capacity locking under load
- Gather customer feedback on UX

**Iterate Based on Data:**
- Which features do customers request?
- Where do bookings drop off?
- What admin tasks are painful?

### Technical Quality Gates

**Before Production:**
- [ ] Load test concurrent bookings (10+ simultaneous users)
- [ ] Verify no overbooking under stress
- [ ] Test PayU webhook signature validation
- [ ] Confirm email deliverability (not in spam)
- [ ] Implement payment timeout cleanup
- [ ] Database backup + restore tested
- [ ] Error monitoring (Sentry or similar)

**Security Checklist:**
- [ ] SQL injection prevention (parameterized queries)
- [ ] CSRF protection on forms
- [ ] Webhook signature validation
- [ ] HTTPS enforced
- [ ] Sensitive data encrypted (payment logs)
- [ ] Admin endpoints authenticated

---

## Questions to Resolve Before Development

### Cancellation Policy Details

- [ ] Full refund cutoff: 24hrs? 48hrs? 72hrs?
- [ ] Partial refund percentage: 50%? 25%?
- [ ] Operator cancellation: Always full refund?
- [ ] Weather cancellation: Reschedule or refund?

### Payment Edge Cases

- [ ] What if PayU is down during booking?
- [ ] Customer pays but webhook fails to arrive?
- [ ] Refund fails but customer expects money back?
- [ ] Currency: Always PLN or support EUR/USD?

### Operational Questions

- [ ] Who manages tour schedules? (Owner? Staff?)
- [ ] How far in advance to publish schedules? (30 days? 90 days?)
- [ ] Minimum party size? (Cancel tour if <4 people?)
- [ ] Maximum advance booking? (6 months? 1 year?)
- [ ] Group requests exceeding capacity: Waitlist? Decline?

### Mobile Terminal Integration

- [ ] Which terminal provider? (PayU POS? Square? Other?)
- [ ] Integration timeline? (Phase 2? Phase 3?)
- [ ] Booking flow: Create booking without payment, mark "pay on arrival"?

---

## Success Metrics

### Phase 1 Launch Goals

- [ ] Zero overbooking incidents
- [ ] 100% payment webhook processing
- [ ] <5% email bounce rate
- [ ] <2% payment timeout failures
- [ ] Mobile responsive (pass Google Mobile-Friendly Test)

### Business Metrics

- [ ] Replace FareHarbor within 60 days
- [ ] Save 12,000+ PLN annually in fees
- [ ] Booking conversion rate ≥ FareHarbor's baseline
- [ ] Customer satisfaction (email survey)

---

## References

### FareHarbor Research Sources

1. Add FareHarbor booking calendars to WordPress - `https://help.fareharbor.com/website/integrations/tools/wordpress-plugin/`
2. FareHarbor 2025 Pricing, Features, Reviews & Alternatives - `https://www.getapp.com/customer-management-software/a/fareharbor/features/`
3. FareHarbor Pricing Guide 2025 - `https://www.bokun.io/fareharbor-pricing`
4. FareHarbor Integration Center - `https://developer.fareharbor.com/api/external/v1/`
5. 10 Best FareHarbor Alternatives - `https://www.ticketinghub.com/blog/fareharbor-alternatives`

### Technical Documentation

- PayU REST API: `https://developers.payu.com/`
- PostgreSQL Locking: `https://www.postgresql.org/docs/current/explicit-locking.html`
- Astro Server Endpoints: `https://docs.astro.build/en/guides/endpoints/`
- BLIK Documentation: `https://blik.com/`

---

## Document Metadata

**Version:** 1.0
**Last Updated:** 2026-01-02
**Author:** KrakowBites Development Team
**Status:** Requirements Definition
**Next Steps:** Technical implementation planning, database schema finalization, PayU sandbox setup
