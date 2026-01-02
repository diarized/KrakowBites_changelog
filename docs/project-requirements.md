# KrakowBites - Project Requirements Document

**Date:** 2026-01-01
**Status:** Requirements gathered, ready for implementation planning
**Timeline:** 2-3 months to launch
**Project Type:** Solo guide booking platform

---

## Business Context

### Purpose
Enable independent Kraków tour guide to:
- Replace intermediary income (Walkative, FreeWalk)
- Establish direct tourist relationships
- Control pricing and booking flow
- Showcase unique food + heritage tour expertise

### Target Audience
- **Primary:** International tourists visiting Kraków (English-speaking)
- **Secondary:** Tour operators seeking quality guide partnerships
- **Geographic:** Global (optimize for US, UK, Western Europe)

### Differentiators
- **Narrative-driven tours** - Not generic "food tour" but "Shtetl to Street: Lost Recipes"
- **Dual expertise** - Food AND heritage (unique combination)
- **Solo guide authenticity** - Personal connection vs corporate tour companies
- **Customization** - Flexible private tours tailored to interests

---

## Tour Catalog

### Heritage Tours (max 25 people)

**Primary Destinations:**
1. **Auschwitz-Birkenau Memorial**
   - Full-day guided tour (70km from Kraków)
   - UNESCO World Heritage Site
   - 1M+ annual visitors
   - Advance booking required

2. **Schindler's Factory Museum**
   - Nazi occupation exhibition
   - Polish-Jewish relations history
   - Kraków Ghetto context
   - High demand attraction

3. **Kazimierz Jewish Quarter**
   - Historic streets and synagogues
   - Pre-war Jewish community
   - Cultural heritage focus (NOT Holocaust-centric)

**Secondary Sites:**
- Kraków Ghetto (Heroes' Square, Eagle Pharmacy, wall remnants)
- Płaszów Concentration Camp (memorial site)

**Popular Combinations:**
- Full-day: Auschwitz + Schindler's Factory
- Half-day: Kazimierz + Schindler's Factory
- Comprehensive: Kazimierz + Ghetto + Płaszów + Schindler's Factory

---

### Food Tours (max 12 people)

**1. Shtetl to Street: Lost Recipes of Jewish Kazimierz**
- **Concept:** Jewish dishes that disappeared during WWII, recreated by historians and chefs
- **Featured:** Challah variations, pre-war gefilte fish, forgotten Shabbat stews, cholent traditions
- **Duration:** 3-4 hours
- **Stops:** 5-6 traditional Jewish restaurants/bakeries in Kazimierz

**2. The Ghetto Kitchen: Survival & Resilience Through Food**
- **Concept:** How Jewish families adapted recipes during occupation
- **Narrative:** Substitution cooking, underground bakeries, food smuggling stories
- **Contrast:** Wartime versions vs full traditional recipes today
- **Duration:** 3-4 hours
- **Emotional weight:** Heavy historical content, requires sensitivity

**3. Market to Table: Stary Kleparz & Jewish Home Cooking**
- **Concept:** Morning market shopping + private home meal preparation
- **Experience:** Interactive cooking with local Jewish or Polish family
- **Featured:** Family stories, traditional techniques, market since 1775
- **Duration:** 4-5 hours (shopping + cooking + eating)
- **Group size:** Max 6-8 (limited by home kitchen capacity)

**4. Shabbes to Sunday: Dual Heritage Feast**
- **Concept:** Parallel traditions - Jewish Shabbat vs Polish Sunday feast
- **Featured:** Kugel, cholent, challah vs rosół, pierogi, bigos
- **Cultural:** Comparing rituals, ingredients, family customs, shared history
- **Duration:** 3-4 hours
- **Unique angle:** Side-by-side comparison rarely offered

**5. Pierogi & Kreplach: Dumpling Trail Across Cultures**
- **Concept:** Deep dive into dumpling traditions
- **Featured:** Pierogi (ruskie, mięsne, z kapustą), kreplach, uszka
- **Hands-on:** Family recipes, regional variations, wrapping demonstration
- **Duration:** 3 hours
- **Appeal:** Highly Instagram-friendly, interactive

**6. Kazimierz After Dark: Underground Vodka & Pickle Trail**
- **Concept:** Evening hidden bars + Jewish tavern culture
- **Featured:** Craft vodkas, pickles, herring, rye bread, kvass
- **Cultural:** Traditional drinking songs and toasts
- **Duration:** 2.5-3 hours
- **Audience:** 21+ only, adult evening experience

---

## Core Requirements

### 1. Booking System

**Scheduled Tours:**
- Guide publishes monthly schedule with available tour dates
- Tourists select date, tour type, number of people
- Full payment required at booking (PayU integration)
- **Food tours:** Max 12 people (exception: Market to Table max 6-8)
- **Heritage tours:** Max 25 people
- **Cancellation:** Free if cancelled >48h before tour
- **Confirmation:** Instant email/SMS with tour details and mobile ticket (PDF/QR code)

**Private Tours:**
- Same tour catalog available for private booking
- **Pricing:** Per-person with premium (e.g., 250 PLN scheduled, 350 PLN private)
- Tourists select preferred date (subject to guide availability)
- Request submitted, guide approves within 24h
- Payment after approval

**Calendar Management:**
- Guide manually updates available tour dates weekly/monthly
- Admin panel to add/remove tour availability
- Booking closes automatically when capacity reached
- No Google Calendar sync initially (MVP keeps it simple)

---

### 2. Payment Integration

**PayU (Primary Gateway):**
- Full payment at booking
- Support PLN, USD, EUR, GBP (display all, charge in PLN)
- Automated refund via PayU API if cancelled >48h before tour
- No partial payments/deposits - simplifies accounting

**Currency Display:**
- Show prices in PLN + USD + EUR estimates
- Update exchange rates daily (API: exchangerate-api.io or similar)
- User can toggle preferred currency display
- Actual payment processed in PLN

**Refund Automation:**
- If cancellation >48h before tour → instant automated refund
- If cancellation <48h → no refund (policy clearly stated at booking)
- Track refunds in admin dashboard

---

### 3. Tour Operator Flow

**Separate B2B Path:**
- "Tour Operators" link in main nav
- Inquiry form requesting:
  - Company name, contact person
  - Requested tour(s), dates, group size
  - Commission expectations
- Email notification to guide
- Manual coordination (no automated booking for operators initially)
- Negotiated rates case-by-case

**Why Manual:**
- Maintains control over partnerships
- Flexibility in pricing/terms
- Avoid cannibalizing direct bookings
- Can automate later if volume justifies

---

### 4. Dietary Restrictions (Food Tours)

**Critical Feature:**
- During booking flow, ask: "Any dietary restrictions?"
- Checkboxes: Vegetarian, Vegan, Gluten-free, Kosher, Dairy-free, Other (text field)
- Guide receives notification with restrictions
- Email confirmation mentions: "We'll accommodate your [restriction] - specific dishes will vary based on availability"

**Implementation:**
- Not tour-specific (some tours easier to accommodate than others)
- Guide responsibility to plan alternatives
- Set expectations during booking

---

### 5. User Experience

**Homepage:**
- Hero section: High-quality food photo or Kazimierz street scene
- Value proposition: "Taste Kraków's forgotten stories with a local guide"
- Two clear paths: "Food Tours" | "Heritage Tours"
- Featured tours (3-4 cards with images)
- Guide bio preview with photo
- Social proof: Review count, years experience
- CTA: "Browse Tours" or "Book Now"

**Tour Listing Pages:**
- **Food Tours:** Grid of 6 tour cards
- **Heritage Tours:** List of destinations + popular combinations
- Each card shows:
  - Tour name + tagline
  - Hero image
  - Duration, group size, price (in selected currency)
  - Key highlights (3-4 bullet points)
  - Dietary accommodation icon (food tours)
  - "View Details" button

**Tour Detail Page:**
- Hero image gallery (5-8 photos)
- Title, tagline, duration, price
- Full description (storytelling narrative, not bullet points)
- "What's Included" section (food, drinks, guide, entrance fees if applicable)
- "Meeting Point" with map
- Sample itinerary/stops
- Dietary accommodations note (food tours)
- Reviews section
- Booking widget:
  - Select date (calendar shows available dates only)
  - Number of people (up to max capacity)
  - Dietary restrictions form (food tours)
  - Price calculation (updates based on # people)
  - "Book Now" → PayU checkout

**About the Guide:**
- Personal story: Why Kraków? Why food? Why this work?
- Photo (professional, welcoming)
- Credentials: Years guiding, certifications, languages
- Personal touch: Favorite Kraków spot, favorite dish
- NOT a resume - storytelling format

**Contact Page:**
- Form: Name, email, message
- Phone number (click-to-call on mobile)
- Use cases highlighted: "Custom tours", "Group bookings", "Questions about dietary needs"

---

### 6. Mobile Optimization

**Requirements:**
- Mobile-first design (60%+ traffic will be mobile)
- Touch-friendly buttons (min 44px tap targets)
- Fast load times (<2 seconds on 3G)
- Offline capability for mobile tickets (PWA)
- Easy thumb navigation
- Readable text without zooming
- Sticky CTA button on tour detail pages

---

### 7. Reviews System

**Strategy: Start Fresh**
- No imported reviews from Walkative/FreeWalk
- Post-tour automated email requesting review
- Email sent 24h after tour completion
- Simple form: 1-5 stars, text review, name (optional), country
- Admin approval before publishing (filter spam)
- Display on tour detail pages + homepage

**Launch Incentive:**
- First 20 customers: 20% discount in exchange for guaranteed review
- Clearly communicated during booking
- Tracks "launch customer" status in database

---

### 8. SEO & Content

**Pages Needed:**
- Homepage
- Food Tours (listing)
- Heritage Tours (listing)
- Individual tour detail pages (12+ pages total)
- About the Guide
- Contact
- FAQs
- Booking Terms & Cancellation Policy
- Privacy Policy
- Tour Operator Inquiries

**SEO Priorities:**
- Target: "Kraków food tour", "Jewish heritage tour Kraków", "Auschwitz guide"
- Blog potential: Recipes, historical stories, Kraków travel tips (Phase 2)
- Schema markup: LocalBusiness, TouristAttraction, Event
- Open Graph tags for social sharing

---

## Technical Architecture

### Tech Stack

**Framework:** Astro 4.x + TypeScript
- **Why:** Zero JavaScript by default (HTML/CSS first), islands architecture for minimal JS
- TypeScript native support
- SSR with Node.js adapter (self-hosted)
- Component-agnostic (React islands only where needed)

**Database:** PostgreSQL 16+
- **Why:** Self-hosted on VPS, relational data (tours, bookings, users), full control
- Drizzle ORM or raw SQL via `pg` library
- No external database services

**Payments:** PayU Poland integration
- Server-side API calls (secure)
- Webhook for payment confirmations

**Hosting:** Self-hosted VPS
- **Why:** Apache2 reverse proxy to Node.js, full control, no monthly fees
- Let's Encrypt SSL (free)
- systemd process management

**Email:** Existing SMTP cluster
- Nodemailer with custom SMTP configuration
- React Email for templates (compile to HTML)
- No external email services

**Storage:** Local filesystem
- `/var/www/krakowbites/uploads/` for tour images
- Apache serves static assets directly
- Optional CDN later (Cloudflare) if traffic grows

**Analytics:** Self-hosted
- Plausible Analytics (self-hosted) or Umami
- Privacy-first, GDPR compliant
- No external tracking dependencies

---

### Database Schema (Preliminary)

**Tours Table:**
```
id, category (food|heritage), name, slug, tagline, description,
duration_hours, max_capacity, base_price_pln, dietary_friendly,
image_urls[], what_included[], meeting_point, status (active|draft)
```

**Tour Availability:**
```
id, tour_id, date, time, available_spots, status (open|full|cancelled)
```

**Bookings:**
```
id, tour_availability_id, customer_name, customer_email, customer_phone,
num_people, total_price_pln, currency_selected, dietary_restrictions,
payment_status, payment_id (PayU), booking_reference, created_at
```

**Reviews:**
```
id, tour_id, customer_name, country, rating (1-5), review_text,
status (pending|approved|rejected), created_at
```

**Tour Operators (inquiries only):**
```
id, company_name, contact_name, email, phone, requested_tours,
dates, group_size, message, status (new|contacted|converted), created_at
```

**Admin Users:**
```
id, email, password_hash, role (admin), created_at
```

---

### Key Features Implementation

**1. Booking Flow:**
- Tourist selects tour → picks date from calendar → enters details → PayU checkout
- On success: Send confirmation email, generate QR code ticket, store booking
- On failure: Show error, don't create booking

**2. Admin Dashboard:**
- View all bookings (upcoming, past, cancelled)
- Manage tour availability (add/remove dates)
- Approve/reject reviews
- View revenue reports
- Process manual refunds if needed
- Manage tour content (edit descriptions, prices, images)

**3. Automated Emails:**
- **Booking confirmation:** Tour details, meeting point, QR ticket, dietary note
- **Reminder:** 48h before tour with weather check
- **Review request:** 24h after tour completion
- **Cancellation confirmation:** Refund processed, sorry to miss you

**4. Multi-currency:**
- Fetch exchange rates daily via API
- Store in cache (Redis or simple JSON file)
- Display toggle: PLN | USD | EUR
- Cookie to remember preference
- Always charge in PLN (PayU handles conversion)

**5. Mobile Ticket:**
- PDF generation with QR code
- QR contains: booking_reference, tour_name, date, num_people
- Guide scans with phone app (simple QR scanner, no custom app needed initially)
- Alternatively: Guide manually checks email confirmation

**6. Refund Automation:**
- Cron job runs daily: check bookings cancelled >48h before tour date
- If payment_status = 'paid' and cancellation valid → trigger PayU refund API
- Update booking status to 'refunded'
- Send refund confirmation email

---

## Design & Branding

### Brand Identity: KrakowBites

**Name Rationale:**
- "Bites" = food focus (primary differentiator)
- Also works for heritage (bite-sized history stories)
- Easy to remember, pronounce, spell
- .com domain available? (check)

**Visual Identity Needed:**
- Logo design
- Color palette
- Typography system
- Photography style guide

**Suggested Direction:**
- **Colors:** Warm, inviting (think: bread, golden challah, paprika, brick red)
- **Avoid:** Tourist-trap red/white Polish flag cliché
- **Style:** Sophisticated but approachable, historical but modern
- **Photography:** Authentic (not stock), focus on food close-ups + candid tour moments

**Logo Concepts to Explore:**
- Typography-first (elegant serif + modern sans)
- Subtle icon: pierogi outline, Star of David + fork, bread loaf
- NOT: Generic tour bus, castle, or cliché Polish symbols

---

## Content Requirements

### Guide Bio
- **Tone:** Personal, storytelling, authentic
- **Length:** 300-500 words
- **Include:**
  - Why Kraków (personal connection)
  - How you discovered food/heritage guiding
  - What makes your tours different
  - Favorite Kraków memory or dish
  - Photo: Welcoming, genuine smile, in Kraków setting

### Tour Descriptions
- **Length:** 400-600 words per tour
- **Structure:**
  1. Opening hook (storytelling angle)
  2. What you'll experience (sensory details)
  3. Historical/cultural context
  4. What's included (practical details)
  5. Who this tour is for
- **Tone:** Evocative, narrative, NOT bullet points
- **Example opening (Shtetl to Street):**
  > "Before World War II, Kazimierz's bakeries filled the air with challah on Friday afternoons. Grandmothers passed down gefilte fish recipes that varied from family to family, street to street. These flavors disappeared in 1939. But through the work of historians and devoted chefs, we can taste them again..."

### Photography
- **DIY approach initially** (use existing photos)
- **Priority shots:**
  - Each tour's signature dishes (close-ups, well-lit)
  - Guide in action with tourists (candid, genuine)
  - Kazimierz streets, Stary Kleparz market, dining moments
  - Heritage sites (Auschwitz requires respectful framing)
- **Later investment:** Professional food photographer day (~1500-2000 PLN)

---

## Launch Strategy

### Phase 1: MVP (Months 1-2)

**Goal:** Functional booking site

**Scope:**
- Homepage + tour listing pages + tour detail pages
- Booking flow with PayU
- Basic admin dashboard
- Automated confirmation emails
- Mobile-responsive design
- 6 food tours + 3-4 heritage tour combinations live

**Marketing:**
- Soft launch to existing network (Walkative customers who know you)
- Google Business Profile
- Instagram account (@krakowbites)
- Basic SEO setup

---

### Phase 2: Optimization (Months 3-6)

**Add:**
- Review collection system
- Refund automation
- Enhanced admin analytics
- Professional photography
- Blog with 5-10 articles (SEO content)
- Email marketing (newsletter for repeat customers)

**Marketing:**
- Google Ads (target "Kraków food tour", "Auschwitz guide")
- Partner with Kraków hotels/hostels (brochure placement)
- TripAdvisor listing
- Social media content calendar

---

### Phase 3: Scale (Year 2)

**Explore:**
- Train additional guides (if demand exceeds solo capacity)
- Gift certificates
- Partnership with Airbnb Experiences
- Expand tour catalog (seasonal tours, multi-day packages)
- Corporate team-building offerings

---

## Success Metrics

### Launch Targets (First 6 Months)
- **Bookings:** 4-8 tours/week (high season), 2-3/week (off-season)
- **Revenue:** Replace Walkative/FreeWalk income within 3 months
- **Reviews:** 20+ verified reviews collected
- **Conversion:** 3-5% (visitors → bookings)
- **Traffic:** 1000-2000 monthly visitors

### Growth Targets (Year 1)
- **Bookings:** 12-15 tours/week (high season)
- **Revenue:** 150% of previous intermediary income
- **Reviews:** 100+ verified reviews
- **Repeat customers:** 10-15%
- **Direct bookings:** 90%+ (vs tour operator bookings)

---

## Risks & Mitigations

### Risk: Capacity bottleneck (solo guide)
**Mitigation:** Premium pricing, waitlist feature, private tour upsell (higher margins)

### Risk: Seasonal demand (Nov-Feb slowdown)
**Mitigation:** Winter tour angles (comfort food, cozy indoor experiences), gift certificates, content marketing during slow months

### Risk: Review cold start (no social proof)
**Mitigation:** Launch discount for first 20 customers, import testimonials as text (not fake reviews), emphasize years of Walkative experience

### Risk: PayU integration complexity
**Mitigation:** Use PayU REST API documentation, sandbox testing, fallback to manual payment links initially if needed

### Risk: Tour operator competition with direct bookings
**Mitigation:** Weekday availability for operators, minimum 10-person groups, keep best time slots for direct bookings

---

## Next Steps

1. **Finalize tour catalog** - Confirm all 6 food tours + heritage combinations, finalize pricing
2. **Brand identity design** - Logo, colors, typography for KrakowBites
3. **Content creation** - Write tour descriptions, guide bio, take/organize photos
4. **Technical setup** - Initialize Next.js project, Supabase database, PayU sandbox
5. **Design mockups** - Homepage, tour listing, tour detail, booking flow wireframes
6. **Development sprints** - Build in 2-week iterations with weekly check-ins

---

## Open Questions

- [ ] Domain registration - krakowbites.com available? (.pl alternative?)
- [ ] PayU account setup - already registered or need to create?
- [ ] Exact pricing for each tour (per-person scheduled, per-person private)
- [ ] Confirmation on "Market to Table" max capacity (mentioned 6-8 vs standard 12)
- [ ] Heritage tour pricing (same structure as food, or different given transport costs?)
- [ ] Any existing brand assets (logo sketches, color preferences, photography style likes/dislikes?)
- [ ] Preferred launch date target (specific month?)

---

**Document Status:** Requirements complete, awaiting technical architecture design
**Next Document:** Technical specification with API contracts, component architecture, deployment plan
