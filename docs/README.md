# KrakowBites - Project Documentation

**Last Updated:** 2026-01-01
**Status:** Planning complete, ready for implementation
**Timeline:** 2-3 months to MVP launch

---

## Quick Links

| Document | Purpose |
|----------|---------|
| [Preliminary Research](./tour-booking-preliminary-research.md) | Industry analysis, competitor research, best practices |
| [Project Requirements](./project-requirements.md) | Complete functional spec, business requirements, success metrics |
| [Technical Architecture](./technical-architecture.md) | Tech stack, database schema, API design, deployment plan |
| [Brand Identity Brief](./brand-identity-brief.md) | Logo concepts, color palette, typography, visual guidelines |

---

## Project Overview

### Purpose
Enable independent Kraków tour guide to establish direct booking platform, replacing intermediary services (Walkative, FreeWalk) with owned revenue channel.

### Core Value Proposition
**Narrative-driven food and heritage tours** - not generic walking tours, but deeply personal storytelling experiences combining Polish & Jewish cuisine with historical context.

### Unique Positioning
- **Solo guide authenticity** vs corporate tour companies
- **Dual expertise** in food AND heritage (rare combination)
- **Story-first approach** ("Shtetl to Street", "Ghetto Kitchen" vs "Jewish Food Tour #47")
- **Premium quality** at competitive pricing (no middleman fees)

---

## Tour Catalog Summary

### Food Tours (Max 12 people)
1. **Shtetl to Street** - Lost Jewish recipes recreated
2. **Ghetto Kitchen** - Survival cooking + resilience narrative
3. **Market to Table** - Stary Kleparz shopping + home cooking (max 6-8)
4. **Shabbes to Sunday** - Jewish Shabbat vs Polish Sunday feast comparison
5. **Pierogi & Kreplach** - Dumpling deep-dive across cultures
6. **Kazimierz After Dark** - Vodka & pickle trail (21+ evening experience)

### Heritage Tours (Max 25 people)
- Auschwitz-Birkenau Memorial (full-day, 70km from city)
- Schindler's Factory Museum (Nazi occupation exhibition)
- Kazimierz Jewish Quarter (pre-war culture focus)
- Kraków Ghetto sites (Heroes' Square, Eagle Pharmacy, walls)
- Płaszów Concentration Camp (memorial site)
- Various combinations (half-day/full-day packages)

---

## Technical Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| **Frontend** | Astro 4.x + TypeScript + Tailwind | Zero JS by default, HTML/CSS first, minimal shipped JS |
| **Database** | PostgreSQL 16+ (self-hosted) | Relational data, full control, no external dependencies |
| **Payments** | PayU Poland | Best option for Polish market |
| **Email** | Existing SMTP cluster + Nodemailer | Self-hosted, no external service fees |
| **Hosting** | Self-hosted VPS + Apache2 | Full control, reverse proxy to Node.js, no monthly fees |
| **ORM** | Drizzle ORM or raw SQL | Lightweight, TypeScript-first, direct PostgreSQL connection |
| **Auth** | Custom JWT or Lucia Auth | Admin dashboard only, session in PostgreSQL |

**Why this stack:**
- **Minimal JavaScript** (Astro ships 0kb JS for static content, ~5-10kb for interactive pages)
- **Self-hosted** (no SaaS dependencies, full infrastructure control)
- **Cost-effective** (~5 PLN/month domain only, VPS already owned)
- **TypeScript native** (type safety without configuration overhead)
- **Performance** (Apache serves static assets, Node.js for dynamic content)

---

## Key Features

### For Tourists
✅ Browse food & heritage tour catalog
✅ View detailed tour pages with photos, itineraries, reviews
✅ Book scheduled tours (fixed dates) or request private tours
✅ Pay securely via PayU (PLN, USD, EUR display)
✅ Receive instant confirmation + mobile ticket (QR code)
✅ Specify dietary restrictions (food tours)
✅ Cancel up to 48h before tour (automated refund)
✅ Submit reviews after tour completion

### For Guide (Admin)
✅ Manage tour availability calendar
✅ View all bookings (upcoming, past, revenue reports)
✅ Approve/decline private tour requests
✅ Moderate customer reviews
✅ Handle tour operator inquiries
✅ Update tour content (descriptions, pricing, photos)

### For Tour Operators (B2B)
✅ Separate inquiry form for bulk bookings
✅ Manual coordination (negotiate rates case-by-case)
✅ No automated booking (protects direct booking margins)

---

## Revenue Model

### Pricing Structure
- **Scheduled tours:** Per-person pricing (e.g., 250 PLN/person)
- **Private tours:** Per-person premium (e.g., 350 PLN/person)
- **Full payment at booking** with 48h free cancellation
- **Tour operators:** Negotiated rates (minimum 10 people)

### Target Metrics (Year 1)
- **High season:** 12-15 tours/week
- **Off-season:** 4-6 tours/week
- **Revenue goal:** 150% of previous Walkative/FreeWalk income
- **Direct bookings:** 90%+ (minimize tour operator dependency)

---

## Brand Identity

### Visual Direction
**Warm, sophisticated, authentic**

| Element | Choice | Rationale |
|---------|--------|-----------|
| **Logo** | Typography-first ("Krakow" serif + "BITES" sans) | Balances heritage + modern |
| **Colors** | Golden Wheat (#D4A574) + Deep Paprika (#8B3A3A) | Warm, food-focused, Kraków brick |
| **Typography** | Playfair Display (headings) + Inter (body) | Elegant + readable |
| **Photography** | Warm natural lighting, authentic moments | No stock photos, real experience |

**Avoid:**
- Tourist-trap Polish flag red/white clichés
- Dark/somber Holocaust imagery (respectful but not branding)
- Generic food blog pastels
- Corporate sterility

---

## Implementation Phases

### Phase 1: MVP (Months 1-2)
**Goal:** Functional booking site accepting real payments

**Deliverables:**
- Homepage + tour catalog pages
- Booking flow with PayU integration
- Admin dashboard (basic)
- Email confirmations
- Mobile-responsive design
- 6 food + 4 heritage tours live

**Launch Strategy:**
- Soft launch to Walkative network (people who know the guide)
- Google Business Profile
- Instagram @krakowbites
- Basic SEO setup

---

### Phase 2: Optimization (Months 3-6)
**Add:**
- Review collection system (automated post-tour emails)
- Refund automation (cron job)
- Enhanced analytics
- Professional food photography (if budget allows)
- Blog (5-10 SEO articles)

**Marketing:**
- Google Ads ("Kraków food tour", "Auschwitz guide")
- Hotel/hostel partnerships
- TripAdvisor listing
- Social media content calendar

---

### Phase 3: Scale (Year 2)
**Explore:**
- Train additional guides (if demand exceeds capacity)
- Gift certificates
- Airbnb Experiences integration
- Multi-language support (Polish, German, Spanish)
- Corporate team-building packages

---

## Key Decisions Made

Based on interactive Q&A session, these decisions finalize the scope:

| Question | Decision | Impact |
|----------|----------|--------|
| Group sizes | Food: <12, Heritage: <25 | Manageable, quality over volume |
| Pricing model | Per-person with private premium | Scales revenue with group size |
| Tour schedule | Flexible monthly calendar | Maximizes guide flexibility |
| Tour operators | Manual coordination | Protects direct booking margins |
| Payment timing | Full payment at booking | Reduces no-shows, simple accounting |
| Calendar mgmt | Manual weekly/monthly updates | Simple MVP, automate later |
| Dietary needs | Critical for food tours | Inclusive, expands market |
| Reviews | Start fresh with launch discount | Authentic, incentivized early adopters |
| Jewish heritage | Historical/cultural focus (pre-war) | Educational, not exploitative |
| Tech approach | Custom Next.js (2-3 months) | Modern UX, full control, scalable |
| Content | DIY photos initially | Zero cost, authentic, professional later |
| Branding | "KrakowBites" + design system | Food-first name, heritage works too |
| Currency | PLN + USD/EUR estimates | Tourist-friendly, updated daily |
| Refunds | Automated via PayU API | Better UX, less manual work |

---

## What Sets This Apart

### From Generic Tour Research
The preliminary research analyzed GetYourGuide, Viator, and competitors to identify:
- **Table stakes** (must-have features like instant confirmation, mobile tickets)
- **Differentiation opportunities** (guide profiles, sustainability scoring, Magic Link booking)

### From Generic Tour Sites
Most local operators use:
- Generic card layouts
- FareHarbor embedded widgets (monthly fees)
- Template designs
- "Polish Food Tour" naming

**KrakowBites offers:**
- Custom-built platform (no monthly widget fees)
- Narrative tour names that tell stories
- Personal brand (guide as protagonist, not faceless company)
- Content-first approach (storytelling in descriptions)

---

## Risks & Mitigations

| Risk | Mitigation |
|------|-----------|
| **Solo capacity bottleneck** | Premium pricing, private tour upsell, waitlist feature |
| **Seasonal demand (winter slow)** | Winter-themed tours (comfort food), gift certificates, content marketing |
| **Review cold start** | Launch discount (first 20 customers), import Walkative testimonials as text |
| **PayU complexity** | Sandbox testing, fallback to manual payment links if needed |
| **Tour operator conflict** | Weekday slots for operators, min 10 people, different tours |

---

## Next Steps (Immediate Actions)

### 1. Domain & Accounts
- [ ] Register krakowbites.com (or .pl)
- [ ] Create PayU merchant account
- [ ] Sign up for Supabase (free tier)
- [ ] Sign up for Vercel (free tier)
- [ ] Create Resend account (email)
- [ ] Setup GitHub repository

### 2. Brand Assets
- [ ] Commission logo design (Direction 1: typography-first)
- [ ] Finalize color palette (Tailwind config)
- [ ] Select and organize tour photos
- [ ] Write guide bio (300-500 words)
- [ ] Write tour descriptions (6 food + 4 heritage)

### 3. Technical Setup
- [ ] Initialize Astro project with Node.js adapter
- [ ] Setup PostgreSQL database on VPS
- [ ] Configure Drizzle ORM or raw SQL connection
- [ ] Configure Tailwind CSS
- [ ] Create database schema (run migrations)
- [ ] Seed tour data
- [ ] Setup PayU sandbox integration
- [ ] Configure Apache reverse proxy

### 4. Design Phase
- [ ] Create Figma mockups (homepage, tour detail, booking flow)
- [ ] Design component library (buttons, cards, forms)
- [ ] Email templates (booking confirmation, cancellation, review request)
- [ ] Responsive breakpoints (mobile, tablet, desktop)

### 5. Development Sprints (2-week iterations)
- **Sprint 1:** Homepage + tour listing pages
- **Sprint 2:** Tour detail pages + calendar
- **Sprint 3:** Booking flow + PayU integration
- **Sprint 4:** Admin dashboard
- **Sprint 5:** Email automation + testing
- **Sprint 6:** Polish, deploy, launch

---

## Open Questions (Needs Answers)

- [ ] Exact pricing for each tour (per-person rates)
- [ ] Confirmation on "Market to Table" capacity (6-8 vs 12)
- [ ] Heritage tour pricing structure (same as food, or different due to transport?)
- [ ] PayU account already registered? (or need to create)
- [ ] Any existing brand assets? (logo sketches, color preferences)
- [ ] Preferred launch month? (align with high season?)
- [ ] Guide's full name for bio and booking communications
- [ ] Meeting points for each tour (addresses for maps)

---

## Success Criteria

**MVP launch is successful if:**
- ✅ Site is live and functional (all tours bookable)
- ✅ Payment flow works (PayU sandbox → production tested)
- ✅ First 5 bookings completed without issues
- ✅ Mobile experience is smooth (70%+ of traffic)
- ✅ Admin can manage calendar and bookings independently
- ✅ Email automation works (confirmation, cancellation, review requests)

**6-month success:**
- ✅ Replaced Walkative/FreeWalk income
- ✅ 20+ verified reviews collected
- ✅ 3-5% conversion rate (visitors → bookings)
- ✅ Direct bookings dominate (90%+)
- ✅ Operational efficiency (minimal manual work)

---

## Files in This Documentation

```
docs/
├── README.md (this file)
├── tour-booking-preliminary-research.md
├── project-requirements.md
├── technical-architecture.md
└── brand-identity-brief.md
```

**Total documentation:** ~25,000 words covering business, technical, and design aspects

---

## Contact & Collaboration

**Project Owner:** Guide (name TBD)
**Technical Lead:** To be determined
**Timeline:** 2-3 months to MVP
**Budget Considerations:**
- VPS: 0 PLN (already owned)
- Domain: ~5 PLN/month (~60 PLN/year)
- SSL Certificate: 0 PLN (Let's Encrypt)
- PostgreSQL: 0 PLN (self-hosted)
- Email (SMTP): 0 PLN (existing infrastructure)
- PayU: Transaction fees only (~2-3% per transaction)
- Exchange rate API: 0 PLN (free tier: 1500 requests/month)
- Backups: 0 PLN (local + optional rsync)
- **Estimated monthly cost:** ~5 PLN (domain only)

**One-time costs:**
- Logo design: 500-1500 PLN (if commissioning designer)
- Photography: 1000-2000 PLN (optional, can use existing)
- Development: DIY

---

**We're ready to build. Let's make it happen! 🚀**
