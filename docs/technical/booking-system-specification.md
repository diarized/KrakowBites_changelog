# KrakowBites Booking Platform Specification

**Version:** 1.0
**Created:** 2026-01-02
**Status:** Planning
**Type:** Product Specification
**Scope:** Complete booking platform replacing FareHarbor

---

## Document Purpose

**Purpose:** Define WHAT the KrakowBites booking platform must deliver and WHY each capability is needed

**Audience:** Product Manager, Dev Protocol, Development Team, Stakeholders

**Out of Scope:** Technical implementation details (covered in separate design documents)

---

## Project Overview

### Business Context

**Current Situation:**
- KrakowBites uses FareHarbor for tour bookings
- Commission cost: 6% per transaction (~12,600 PLN/year)
- Limited control over customer data and experience
- Cannot integrate Polish payment methods optimally

**Business Goal:**
- Replace FareHarbor with self-hosted booking platform
- Eliminate recurring commission fees
- Own customer data and relationships
- Support Polish payment ecosystem (PayU, BLIK)

**Success Definition:**
- Platform processes first paid booking successfully
- Zero overbooking incidents
- System operates without developer intervention
- Annual savings of 12,600+ PLN realized

### User Groups

**Primary Users:**
1. **Tourists** - Browse tours, check availability, book and pay
2. **Tour Guide (Joanna)** - Manage tour catalog, schedules, and bookings
3. **System** - Automated payment processing and email notifications

**User Needs:**
- **Tourists:** Simple booking flow, clear tour information, secure payment, immediate confirmation
- **Tour Guide:** Easy schedule management, booking oversight, minimal technical overhead
- **System:** Prevent overbooking, handle payment failures gracefully, maintain data integrity

---

## Core Requirements

### Requirement Categories

**Categories:**
- **Catalog Management:** Tour information and presentation
- **Availability Management:** Schedule creation and capacity tracking
- **Booking Management:** Reservation creation and lifecycle
- **Payment Processing:** Transaction handling and reconciliation
- **Communication:** Customer notifications and confirmations
- **Administration:** Guide's operational tools

---

## Catalog Management Requirements

### Purpose
**Why:** Tourists need comprehensive tour information to make informed booking decisions

### Tour Information Requirements

**MUST have:**
- Tour name with route-based branding ("Route 1: Kazimierz Food Corridor")
- Complete tour description (storytelling narrative)
- Tour itinerary as numbered waypoints
- Meeting point with geographic coordinates
- Duration, capacity, and pricing information
- Tour category (Food Tours, Heritage Tours)
- Visual gallery (hero image + additional photos)

**WHY each element:**
- **Route branding:** Creates memorable identity aligned with Urban Cartography theme
- **Storytelling description:** Sells experience, not just itinerary points
- **Waypoints:** Sets clear expectations for tour flow
- **Coordinates:** Reinforces navigation/exploration brand theme
- **Meeting point:** Eliminates customer confusion on tour day
- **Capacity/pricing:** Essential for booking decision
- **Gallery:** Provides visual proof of experience quality

### Tour Categories

**Categories Required:**
1. Food Tours (6 tours)
2. Heritage Tours (combinations with food tours)

**WHY separate categories:**
- Different customer intents (food-focused vs history-focused)
- Different pricing structures possible
- Enables targeted marketing

### Content Display Requirements

**MUST display:**
- Tour listings page (filterable by category)
- Individual tour detail pages
- Homepage featuring selected tours
- Guide biography and credentials

**WHY:**
- **Listings:** Enable tour discovery and comparison
- **Detail pages:** Provide decision-making information
- **Homepage:** Create compelling first impression
- **Bio:** Build trust with personal guide story

---

## Availability Management Requirements

### Purpose
**Why:** Tours have fixed capacity and specific dates/times requiring schedule management

### Schedule Definition Requirements

**MUST support:**
- Creating tour instances (specific date + time)
- Setting capacity per instance (e.g., "12 people maximum")
- Bulk schedule creation (e.g., "Every Saturday in March at 14:00")
- Schedule modification (change capacity, time, or cancel)
- Schedule visibility window (e.g., "next 60 days")

**WHY each capability:**
- **Instance creation:** Tours run on specific dates, not continuously
- **Capacity limits:** Physical constraints of tour experience
- **Bulk creation:** Guide efficiency (create month of tours quickly)
- **Modification:** Weather, guide availability, demand changes
- **Visibility window:** Prevents overwhelming tourists with too many future dates

### Capacity Management Requirements

**MUST prevent:**
- Overbooking (selling more spots than available)
- Race conditions (two customers booking last spot simultaneously)
- Capacity leaks (spots locked but payment fails)

**WHY critical:**
- **Overbooking:** Operational disaster, damages reputation, requires refunds
- **Race conditions:** Creates overbooking risk under concurrent load
- **Capacity leaks:** Lost revenue from falsely unavailable tours

**MUST release capacity when:**
- Booking payment times out (no payment within 30 minutes)
- Customer cancels (if cancellation allowed)
- Payment fails or is declined

**WHY:**
- **Timeout release:** Prevents indefinite locks from abandoned bookings
- **Cancellation release:** Makes spots available to other customers
- **Payment failure:** Booking without payment is invalid

### Availability Display Requirements

**MUST show:**
- Calendar view of available dates
- Available time slots per date
- Remaining capacity per slot ("3 spots left")
- Sold-out dates clearly marked

**WHY:**
- **Calendar view:** Intuitive date selection
- **Time slots:** Tours may run multiple times per day
- **Remaining capacity:** Creates urgency, helps group bookings
- **Sold-out marking:** Prevents frustration from clicking unavailable dates

---

## Booking Management Requirements

### Purpose
**Why:** Customers need to reserve spots, guide needs to track reservations

### Booking Creation Requirements

**MUST collect:**
- Customer name
- Customer email
- Customer phone number
- Party size (number of people)
- Selected tour instance (date + time)
- Dietary restrictions (food tours only)

**WHY each field:**
- **Name:** Identification on tour day
- **Email:** Confirmation delivery, future communication
- **Phone:** Emergency contact, last-minute changes
- **Party size:** Capacity calculation, group size planning
- **Tour instance:** Links booking to specific schedule
- **Dietary restrictions:** Safety (allergies), experience quality (accommodations)

### Booking Identification

**MUST provide:**
- Unique booking reference (e.g., "KB-20260115-A3F9")
- Generated at booking creation
- Used for lookup and customer service

**WHY:**
- **Uniqueness:** Prevents confusion between bookings
- **Format:** Brand identification (KB prefix) + date context + random suffix
- **Lookup:** Customer can retrieve booking without account login

### Booking Lifecycle

**States required:**
1. **Payment Initiated** - Booking created, awaiting payment
2. **Confirmed** - Payment successful
3. **Cancelled** - Customer or guide cancelled

**WHY these states:**
- **Payment Initiated:** Holds capacity temporarily during checkout
- **Confirmed:** Valid booking with payment proof
- **Cancelled:** Frees capacity, tracks cancellation history

### Booking Timeout Requirements

**MUST timeout:**
- Bookings in "Payment Initiated" state after 30 minutes
- Release capacity automatically
- Prevent customer from completing stale payment

**WHY:**
- **30-minute limit:** Prevents indefinite capacity locks
- **Automatic release:** No manual intervention required
- **Stale prevention:** Payment after timeout could overbooking

---

## Payment Processing Requirements

### Purpose
**Why:** Tours require upfront payment to confirm bookings and prevent no-shows

### Payment Gateway Requirements

**MUST support:**
- PayU integration (Polish payment gateway)
- Payment methods: Credit cards, BLIK, bank transfers
- Payment in Polish Złoty (PLN)
- Sandbox testing environment
- Production transaction processing

**WHY PayU:**
- **Polish market leader:** Customer trust and familiarity
- **BLIK support:** Popular Polish mobile payment method
- **Local processing:** Better conversion rates than international gateways
- **Cost structure:** Lower fees than international alternatives

### Payment Flow Requirements

**MUST enable:**
- Customer redirected to PayU for payment
- Customer returns to site after payment (success or failure)
- System receives payment status asynchronously (webhook)
- Booking confirmed only after successful payment
- Payment failures release capacity

**WHY asynchronous:**
- **Redirect flow:** Industry standard, secure (customer enters card details on PayU)
- **Webhook:** Reliable status updates independent of customer browser
- **Async confirmation:** Customer redirect may fail; webhook ensures processing
- **Failure handling:** Prevents confirmed bookings without payment

### Payment Security Requirements

**MUST validate:**
- Webhook authenticity (signature verification)
- Payment amount matches booking total
- Payment is for correct booking
- Webhook not replayed (duplicate processing)

**WHY each validation:**
- **Signature:** Prevents malicious fake webhooks
- **Amount matching:** Prevents partial payment acceptance
- **Booking correlation:** Prevents payment misassignment
- **Replay prevention:** Prevents duplicate confirmations

### Payment Reconciliation

**MUST maintain:**
- Complete transaction log (all payment attempts)
- PayU order ID linkage to bookings
- Payment timestamps
- Payment status history

**WHY:**
- **Transaction log:** Dispute resolution, financial reconciliation
- **Order ID linkage:** PayU support requests require order ID
- **Timestamps:** Audit trail, timeout calculations
- **Status history:** Debugging payment issues

---

## Communication Requirements

### Purpose
**Why:** Customers need confirmation and guide needs booking notifications

### Email Notification Requirements

**MUST send:**
1. **Booking Confirmation Email** (payment successful)
   - Tour details (name, date, time)
   - Meeting point with coordinates
   - Party size
   - Total amount paid
   - Booking reference
   - What to bring
   - Guide contact information

2. **Payment Receipt Email**
   - PayU transaction ID
   - Amount paid
   - Payment method
   - Payment timestamp

**WHY each email:**
- **Confirmation:** Reassurance, critical information for tour day
- **Receipt:** Financial record, tax documentation

**Email timing:**
- Delivered within 2 minutes of payment confirmation

**WHY timing critical:**
- **Immediate delivery:** Customer expects instant confirmation
- **2-minute buffer:** Allows for delivery delays while remaining "instant"

### Email Deliverability Requirements

**MUST ensure:**
- Emails not marked as spam
- Proper SPF/DKIM configuration
- HTML + plain text fallbacks
- Mobile-friendly formatting
- Test on major providers (Gmail, Outlook, Apple Mail)

**WHY:**
- **Spam prevention:** Missed confirmations damage trust
- **SPF/DKIM:** Email authentication prevents spam classification
- **Plain text fallback:** Accessibility, email client compatibility
- **Mobile formatting:** 60-70% of customers read email on mobile
- **Provider testing:** Different rendering engines require testing

---

## Administration Requirements

### Purpose
**Why:** Guide needs tools to manage tour catalog, schedules, and bookings independently

### Tour Management Requirements

**MUST enable guide to:**
- Create new tours
- Edit existing tours (description, pricing, capacity)
- View all tours in catalog
- Deactivate tours (hide from public)

**WHY:**
- **Independence:** No developer required for tour changes
- **Flexibility:** Pricing adjustments, seasonal descriptions
- **Catalog view:** Oversight of complete offering
- **Deactivation:** Temporarily remove tours without deletion

### Schedule Management Requirements

**MUST enable guide to:**
- Create single schedule instance
- Create bulk schedules (recurring pattern)
- View schedules by tour and date range
- Modify schedule capacity or time
- Cancel scheduled tour instance

**WHY each capability:**
- **Single creation:** Ad-hoc tours, private bookings
- **Bulk creation:** Efficiency (create month of Saturdays)
- **Filtered view:** Find specific schedule quickly
- **Modification:** Demand changes, capacity adjustments
- **Cancellation:** Weather, guide unavailability

### Booking Management Requirements

**MUST enable guide to:**
- View all bookings
- Filter bookings (by status, date, tour)
- Search bookings (customer name, email, reference)
- Create manual booking (phone orders)
- View customer details and dietary restrictions

**WHY each capability:**
- **View all:** Daily operational awareness
- **Filtering:** Find upcoming tours, pending payments
- **Search:** Customer service ("I can't find my confirmation")
- **Manual creation:** Phone bookings, special arrangements
- **Customer details:** Tour preparation (dietary accommodations)

### Dashboard Requirements

**MUST display:**
- Bookings today
- Bookings this week
- Revenue summary (gross)
- Upcoming tours with capacity

**WHY:**
- **Today's bookings:** Tour preparation checklist
- **Week view:** Planning and logistics
- **Revenue:** Business performance awareness
- **Upcoming capacity:** Availability decisions

### Authentication Requirements

**MUST protect admin area:**
- Login required for all admin pages
- Session timeout after inactivity
- Logout capability

**WHY:**
- **Protection:** Prevent unauthorized access to customer data
- **Timeout:** Security on shared/public computers
- **Logout:** Explicit session termination

---

## System Quality Requirements

### Purpose
**Why:** Platform must meet performance, reliability, and usability standards

### Performance Requirements

**MUST achieve:**
- Page load time: <2 seconds
- Booking API response: <500ms
- Availability calendar load: <1 second
- Mobile performance: Lighthouse >90

**WHY each target:**
- **Page load:** Customer patience threshold, SEO ranking factor
- **Booking API:** Critical path cannot lag during checkout
- **Calendar load:** Frequent interaction, must feel instant
- **Mobile performance:** 60-70% of traffic is mobile

### Reliability Requirements

**MUST maintain:**
- System uptime: >99.5% (monthly)
- Zero overbooking incidents
- Payment webhook processing: 100%
- Email delivery rate: >95%

**WHY each target:**
- **Uptime:** Lost bookings = lost revenue
- **Zero overbooking:** Critical operational requirement
- **Webhook reliability:** Missed payment = unconfirmed booking
- **Email delivery:** Missed confirmation damages trust

### Mobile Usability Requirements

**MUST support:**
- Touch-friendly targets (44x44px minimum)
- Responsive design (mobile-first)
- Mobile payment flow
- Mobile calendar date picker
- Forms usable on mobile keyboards

**WHY mobile critical:**
- **60-70% mobile traffic:** Industry norm for tourism
- **Touch targets:** Fat finger prevention
- **Payment flow:** High-value action on mobile
- **Calendar/forms:** Booking happens on mobile

### Accessibility Requirements

**MUST support:**
- Screen reader navigation
- Keyboard-only navigation
- WCAG AA color contrast
- Semantic HTML
- Focus states visible
- Forms with labels
- Images with alt text

**WHY:**
- **Legal compliance:** Accessibility requirements in many markets
- **Inclusivity:** Expand customer base
- **SEO:** Search engines favor accessible sites
- **Usability:** Benefits all users, not just those with disabilities

### Security Requirements

**MUST prevent:**
- SQL injection attacks
- Cross-site scripting (XSS)
- Cross-site request forgery (CSRF)
- Payment webhook spoofing
- Unauthorized admin access
- Customer data exposure

**WHY:**
- **Data protection:** Legal requirement (GDPR/privacy laws)
- **Payment security:** Financial liability, trust damage
- **Reputation:** Security breach destroys business credibility

### SEO Requirements

**MUST implement:**
- Semantic HTML structure
- Meta tags (title, description, Open Graph)
- Schema.org markup (LocalBusiness, TouristAttraction, Event)
- Sitemap.xml generation
- Google Search Console integration

**WHY:**
- **Organic discovery:** Primary customer acquisition channel
- **Rich snippets:** Improved search result presentation
- **Schema markup:** Enables Google's tour/event features
- **Search Console:** Monitor search performance, fix issues

---

## Operational Requirements

### Purpose
**Why:** Platform must support business operations beyond core booking flow

### Backup Requirements

**MUST maintain:**
- Daily database backups
- 30-day backup retention
- Off-site backup storage
- Tested restore procedure

**WHY:**
- **Daily backups:** Minimize data loss window
- **Retention:** Recovery from historical corruption
- **Off-site:** Disaster recovery
- **Tested restore:** Untested backups are worthless

### Monitoring Requirements

**MUST monitor:**
- System uptime (health endpoint checks)
- Payment webhook failures
- Email delivery failures
- Database connectivity
- Critical error rates

**MUST alert on:**
- System downtime
- Webhook processing failures
- Email delivery issues
- Database connection failures

**WHY:**
- **Proactive detection:** Fix issues before customer impact
- **Webhook alerts:** Prevent unprocessed payments
- **Email alerts:** Prevent missed confirmations
- **Database alerts:** Prevent booking system outage

### Support Requirements

**MUST enable guide to:**
- Look up bookings by customer email
- View booking history for customer
- Contact customer (email/phone visible)
- Understand booking status (payment pending vs confirmed)

**WHY:**
- **Customer service:** Handle "I didn't receive confirmation" inquiries
- **Repeat customers:** Recognize returning customers
- **Contact access:** Respond to customer questions
- **Status clarity:** Know whether to expect customer on tour

---

## Phased Delivery Requirements

### Purpose
**Why:** Complex system requires staged delivery to manage risk

### Phase 0: Foundation
**Deliverable:** Infrastructure and content ready for development

**MUST complete:**
- Database deployed and accessible
- Brand assets finalized (logo, colors, typography)
- All tour descriptions written
- Photography organized (placeholders acceptable)
- Development workflow documented

**WHY this phase:**
- **Database first:** All subsequent phases depend on it
- **Content ready:** Development blocked without copy
- **Brand finalized:** Prevents rework of styled components
- **Photos organized:** Enables realistic page layout

### Phase 1: Marketing Website
**Deliverable:** Public can browse tours but cannot book yet

**MUST deliver:**
- Homepage
- Tour listing pages (Food, Heritage)
- Tour detail pages (all tours)
- About page (guide bio)
- Contact page (with working form)

**WHY this phase:**
- **Marketing capability:** Collect email inquiries before booking live
- **Content validation:** Test tour descriptions with real audience
- **SEO foundation:** Begin indexing before booking available
- **Independent value:** Website useful even without booking

### Phase 2: Booking System
**Deliverable:** Complete booking flow except payment

**MUST deliver:**
- Availability calendar
- Booking form
- Capacity management (with overbooking prevention)
- Admin dashboard (tour/schedule/booking management)
- Booking reference generation

**WHY payment excluded:**
- **Complexity isolation:** Payment is highest-risk component
- **Core validation:** Test booking logic before payment complications
- **Load testing:** Verify overbooking prevention without real money

### Phase 3: Payment Integration
**Deliverable:** End-to-end paid bookings

**MUST deliver:**
- PayU integration (sandbox and production)
- Payment flow (redirect and webhook)
- Email automation (confirmation and receipt)
- Payment security (signature validation)
- Payment reconciliation logging

**WHY separate phase:**
- **Risk isolation:** Payment failures don't block booking system testing
- **Focused testing:** Payment scenarios require dedicated attention
- **Production readiness:** Final gate before launch

### Phase 4: Production Launch
**Deliverable:** Platform live and operational

**MUST deliver:**
- Performance optimization (Lighthouse >90)
- SEO finalization (sitemap, Search Console)
- Analytics and monitoring
- Admin documentation
- Soft launch (limited tours/schedules)
- Full launch (all tours available)

**WHY gradual launch:**
- **Risk management:** Soft launch detects issues with limited exposure
- **Feedback incorporation:** Real user testing before full commitment
- **Confidence building:** Validate entire system with real bookings

---

## Success Metrics

### Purpose
**Why:** Define measurable success criteria

### Phase Completion Metrics

**Phase 1 (Website):**
- All pages load <2s: YES/NO
- Mobile-friendly test passes: YES/NO
- Content complete: YES/NO
- Lighthouse score >85: YES/NO

**Phase 2 (Booking):**
- Zero overbooking under load test: YES/NO
- Admin can create tours/schedules: YES/NO
- Capacity locks verified: YES/NO

**Phase 3 (Payment):**
- 100% webhook processing rate: YES/NO
- Emails delivered <2min: YES/NO
- Production test transaction successful: YES/NO

**Phase 4 (Launch):**
- First real booking successful: YES/NO
- Monitoring active: YES/NO
- Guide operating independently: YES/NO

### Business Metrics

**Month 1-3 targets:**
- Cost savings vs FareHarbor: 12,600+ PLN/year
- Booking conversion rate: ≥3%
- System uptime: >99.5%
- Customer satisfaction: >4.5/5
- Zero overbooking incidents: YES/NO

**WHY these metrics:**
- **Cost savings:** Primary business justification
- **Conversion rate:** Validates user experience quality
- **Uptime:** Operational reliability proof
- **Satisfaction:** Customer experience validation
- **Zero overbooking:** Core requirement verification

---

## Constraints and Assumptions

### Technical Constraints

**MUST use:**
- krakowbites.stonith.pl hosting (existing VPS)
- Astro 5.x framework (already deployed)
- PostgreSQL (decision made)
- PayU payment gateway (Polish market requirement)

**WHY each constraint:**
- **Hosting:** Infrastructure already available and paid
- **Astro:** Framework already selected and validated
- **PostgreSQL:** Robust ACID compliance for booking integrity
- **PayU:** Polish market leader, BLIK support

### Business Constraints

**Timeline:** 10 weeks to production launch (sequential phases)
**Budget:** Self-hosted to eliminate recurring fees
**Staffing:** Solo guide operation (no booking staff)

**WHY timeline critical:**
- **Season planning:** Launch before tourist high season
- **Revenue impact:** Every week delayed = lost bookings
- **Competitive timing:** Replace FareHarbor quickly

### Assumptions

**Assumed true:**
- Guide can write tour descriptions (no copywriter required)
- AI-generated/stock photos acceptable for MVP
- PayU merchant account approval will succeed
- Guide available for admin testing and feedback
- Basic photography skills available for future improvement

**Risk if assumptions false:**
- **No descriptions:** Phase 0 blocked
- **Photo quality issues:** May need professional photographer
- **PayU rejection:** Need alternative payment gateway
- **Guide unavailable:** Testing delayed
- **Photo improvement needed:** Budget for photographer

---

## Open Questions

### Questions Requiring Decisions

**Brand & Content:**
- Urban Cartography brand approved by client? (Blocks: Phase 0)
- Logo design approach: Commissioned or DIY? (Blocks: Phase 0)
- Photography strategy: All placeholders for MVP? (Blocks: Phase 1)

**Tour Catalog:**
- Launch with all 6 tours or subset? (Blocks: Phase 1, Phase 4)
- Heritage tour pricing structure? (Blocks: Phase 0)

**Operational Policy:**
- Cancellation policy details (refund cutoff, percentages)? (Blocks: Phase 5 future)
- Maximum advance booking window? (Blocks: Phase 2)
- Minimum party size to run tour? (Blocks: Phase 2)

**Technical:**
- PayU merchant account status? (Blocks: Phase 3)
- Email service: SMTP only or add SendGrid? (Blocks: Phase 3)
- Analytics platform: Plausible or Umami? (Blocks: Phase 4)
- Timeline preference: 10 weeks sequential or 6-7 weeks parallel? (Blocks: All phases)

---

## Related Documents

**Prerequisites:**
- `/docs/brand-alternatives/urban-cartography-specs.md` - Brand specifications
- `/docs/bio.md` - Guide biography content
- `/docs/technical/booking-system-requirements.md` - Initial requirements document

**Dependent Documents (to be created):**
- Architecture design document (Dev Protocol)
- Database schema design
- Payment integration design
- Deployment procedures
- Admin user guide

---

## Document Metadata

**Version:** 1.0
**Created:** 2026-01-02
**Created By:** Product Manager
**Status:** Draft (awaiting stakeholder review)
**Next Review:** After stakeholder approval
**Approvers:** Guide (Joanna Wylon), Development Team, Business Owner

**Change History:**
- 2026-01-02: Initial specification created from development plan
