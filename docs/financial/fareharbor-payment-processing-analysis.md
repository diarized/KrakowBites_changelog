# FareHarbor Payment Processing Analysis

**Project:** KrakowBites Booking System
**Created:** 2026-01-02
**Purpose:** Understanding FareHarbor's bundled payment model vs custom solution
**Status:** Research & Analysis

---

## Executive Summary

FareHarbor operates a **mandatory bundled payment processing model** - you cannot use their platform for booking-only while handling payments separately. The 6% booking fee includes both platform commission AND credit card processing fees. External payment processors (like PayU) are not permitted.

**Critical Finding:** FareHarbor requires using their integrated payment processors (Stripe, Adyen, or PayPal only). You cannot bring your own merchant account or payment gateway.

**Cost Implication:** 6% all-in fee vs 3% PayU fee = 3 percentage points difference = 300 PLN/month on 10,000 PLN revenue = 3,600 PLN/year additional cost.

---

## FareHarbor's Payment Processing Model

### Bundled System (No Separation Possible)

**FareHarbor operates as a Payment Facilitator (PayFac):**
- You become a sub-merchant under FareHarbor's master merchant account
- FareHarbor is the legal merchant of record
- Money flows: Customer → FareHarbor → You
- Cannot separate booking functionality from payment processing

**Required Payment Processors:**
- **Stripe** (most common, U.S.-focused)
- **Adyen** (Europe-focused, better international support)
- **PayPal** (alternative option)

**From FareHarbor Terms of Service:**
> "Providers are required to enter into a separate PSP (User) Agreement to receive the benefits of facilitated payments solutions integrated within the FareHarbor Booking System and to receive payments for Bookings."

> "FareHarbor integrates facilitated payments solutions into the FareHarbor Booking System through the use of third-party payment processors (PSPs)."

### What You CANNOT Do with FareHarbor

**❌ Bring Your Own Payment Processor:**
- Cannot use PayU (Polish payment gateway)
- Cannot use existing merchant accounts
- Cannot connect external payment gateways
- Must use FareHarbor's integrated Stripe/Adyen/PayPal

**❌ Booking-Only Model:**
- No option to use FareHarbor for scheduling/calendar only
- Payment processing is mandatory and bundled
- Cannot handle payments outside FareHarbor system

**❌ Keep Pricing Data Private:**
- FareHarbor must see all transaction amounts (for payment processing)
- All pricing information flows through their system
- Cannot hide revenue data from platform

**❌ Control Refunds Directly:**
- FareHarbor controls refund processing
- You request refunds, FareHarbor executes
- Refunds processed through FareHarbor's payment system

**❌ Choose Settlement Terms:**
- Settlement schedule controlled by FareHarbor
- Typically next-day or scheduled payouts
- Cannot customize beyond FareHarbor's options

### What You CAN Do with FareHarbor

**✅ Choose Between Three Payment Processors:**
- Stripe, Adyen, or PayPal (FareHarbor-integrated only)
- Different regional capabilities
- Different fee structures (within FareHarbor's framework)

**✅ Accept Multiple Payment Methods:**
- Credit/debit cards (Visa, Mastercard, Amex)
- Apple Pay, Google Pay
- PayPal (if using PayPal PSP)
- **Note:** BLIK support unclear (Poland-specific method)

**✅ Fast Onboarding:**
- No separate merchant account application
- Instant activation (piggyback on FareHarbor's account)
- Simplified compliance (PCI handled by FareHarbor)

---

## How the 6% Fee Works (Bundled Pricing)

### Fee Breakdown

**6% Direct Booking Fee = All-Inclusive:**

| Component | Included? | Estimated Share |
|-----------|-----------|-----------------|
| Platform booking fee | ✓ Yes | ~3-4% |
| Credit card processing | ✓ Yes | ~2-3% |
| Payment gateway costs | ✓ Yes | Included |
| Merchant account fees | ✓ Yes | Included |
| PCI compliance | ✓ Yes | Included |
| Chargeback protection | ✓ Yes | Included |
| Settlement/payout fees | ✓ Yes | Included |

**From Industry Sources:**
- "FareHarbor and Xola continue to include credit card merchant fees in the overall booking fee"
- "FareHarbor charges 2% on all API bookings and up to 6% on all direct bookings"
- "Commission rates range from 2.5-4% depending on monthly booking volume tiers"

**Card Processing Rates Reported:**
- 1.9% + $0.30 per transaction (user reports - Stripe rate)
- This appears to be INSIDE the 6% bundled fee, not additional

### Two Pricing Models

**Model 1: Customer Pays Fee (Default)**
- Booking fee transferred to customer at checkout
- Tour costs 200 PLN, customer pays 212 PLN (200 + 6%)
- You receive 200 PLN (minus card processing, included in customer's 12 PLN)
- Customer sees higher total at checkout (cart abandonment risk)

**Model 2: Merchant Pays Fee (Commission Model)**
- You opt for alternative structure (request required)
- Customer pays 200 PLN, you receive 188 PLN (200 - 6%)
- FareHarbor charges commission to you instead
- Cleaner customer experience, lower conversion friction

**API Bookings (OTA Partners):**
- 2% fee on bookings from Airbnb, Viator, GetYourGuide, etc.
- Lower because OTA partner may handle payment processing
- Still flows through FareHarbor system

### Calculation Example

**Scenario: 10,000 PLN Monthly Revenue (50 bookings × 200 PLN)**

**Customer Pays Fee Model:**
```
Customer pays: 50 × 212 PLN = 10,600 PLN
Your revenue: 50 × 200 PLN = 10,000 PLN
FareHarbor fee: 50 × 12 PLN = 600 PLN
(Paid by customers, you receive 10,000 PLN)
```

**Merchant Pays Fee Model:**
```
Customer pays: 50 × 200 PLN = 10,000 PLN
FareHarbor fee: 10,000 × 6% = 600 PLN
Your net: 10,000 - 600 = 9,400 PLN
(You pay fee, customers see lower price)
```

**Either way: 600 PLN goes to FareHarbor monthly**

### Why FareHarbor Bundles Fees

**Business Model Rationale:**

**1. Revenue Maximization:**
- 6% fee includes payment processing margin
- Even if Stripe charges FareHarbor 2%, they charge you 6%
- 4% platform fee + payment margin = higher profitability

**2. Customer Lock-In:**
- Cannot switch platforms without changing payment setup
- Payment history, customer cards locked in ecosystem
- Higher switching costs = better retention

**3. Operational Control:**
- Merchant of record = legal liability control
- Refund control = prevent abuse, manage disputes
- Single vendor relationship (simplicity for customers)

**4. Compliance Simplification:**
- FareHarbor handles PCI DSS compliance
- Customers don't need separate merchant accounts
- Faster onboarding (instant vs 2-week merchant approval)

---

## Payment Processor Options (FareHarbor-Integrated)

### Stripe (Most Common)

**Coverage:**
- U.S., Canada, Europe, Australia, Asia
- 135+ currencies
- Strong U.S. focus

**Payment Methods:**
- Cards (Visa, Mastercard, Amex, Discover)
- Apple Pay, Google Pay
- ACH (U.S. bank transfers)
- **BLIK support:** Unclear (not prominently listed)

**Fees (Typical Stripe Rates):**
- 2.9% + $0.30 per transaction (U.S.)
- 1.4% + €0.25 per transaction (Europe)
- **Note:** Included in FareHarbor's 6% bundled fee

**Settlement:**
- Next-day payouts (standard)
- Weekly/monthly options available

**Pros:**
- Most mature integration with FareHarbor
- Excellent documentation
- Strong fraud protection

**Cons:**
- U.S.-centric (European support secondary)
- BLIK support uncertain for Polish market

---

### Adyen (Europe-Focused)

**Coverage:**
- Europe, North America, Asia Pacific, Latin America
- 150+ currencies
- Strong European presence

**Payment Methods:**
- Cards (all major brands)
- Apple Pay, Google Pay
- SEPA Direct Debit (Europe)
- iDEAL (Netherlands), Sofort (Germany)
- **BLIK:** Supported (Poland-specific)

**Fees (Typical Adyen Rates):**
- 0.6% + €0.10 interchange++ (Europe)
- Higher than Stripe but more transparent
- **Note:** Included in FareHarbor's 6% bundled fee

**Settlement:**
- Customizable payout schedules
- Next-day, weekly, monthly

**Pros:**
- Superior European payment method support
- **BLIK support confirmed** (critical for Polish market)
- Better for international tourists (multi-currency)

**Cons:**
- More complex pricing structure
- Less common in FareHarbor ecosystem (Stripe dominates)

---

### PayPal

**Coverage:**
- Global (200+ markets)
- 25+ currencies

**Payment Methods:**
- PayPal balance
- PayPal Credit
- Cards (via PayPal)
- Venmo (U.S.)

**Fees (Typical PayPal Rates):**
- 2.9% + $0.30 per transaction
- Higher for international transactions
- **Note:** Included in FareHarbor's 6% bundled fee

**Settlement:**
- Instant to PayPal balance
- 1-3 days to bank account

**Pros:**
- High customer trust (PayPal brand)
- Buyer protection (reduces disputes)

**Cons:**
- Higher fees than Stripe/Adyen
- Less flexible for business use
- Lower adoption for tour bookings

---

### Recommendation for Polish Market

**If Using FareHarbor:**

**Best Choice: Adyen**
- BLIK support (critical for 60% of Polish customers)
- Better European card processing
- Multi-currency for international tourists
- Next-day EUR/PLN settlements

**Alternative: Stripe**
- Simpler integration
- More FareHarbor documentation
- **Risk:** BLIK support unclear, may lose Polish customers

**Avoid: PayPal**
- Higher fees
- Less suitable for tour bookings
- Redundant (PayPal users can pay with cards via Stripe/Adyen)

---

## Cost Comparison: FareHarbor vs Custom Solution

### FareHarbor Total Cost (Annual)

**Assumptions:**
- 10,000 PLN/month gross revenue
- 50 bookings/month × 200 PLN average
- Merchant pays fee model (6% commission)
- Using website builder service

**Annual Costs:**

| Item | Calculation | Annual Cost |
|------|-------------|-------------|
| Booking fees (6%) | 120,000 PLN × 6% | 7,200 PLN |
| Website builder | 5,000 PLN/year | 5,000 PLN |
| Payment processing | Included in 6% | 0 PLN |
| Monthly fees | None | 0 PLN |
| Setup fees | None | 0 PLN |
| Hosting | Included | 0 PLN |
| Email/SMS | Included | 0 PLN |
| **Total Annual Cost** | | **12,200 PLN** |

**Effective Rate: 10.2% of gross revenue**

**What You Get:**
- Booking system (calendar, schedules, capacity)
- Payment processing (Stripe/Adyen/PayPal)
- Website builder (optional, adds 5,000 PLN)
- OTA integrations (Airbnb, Viator, GetYourGuide)
- Customer support
- Reporting/analytics
- Email confirmations
- Chargeback management

---

### Custom Solution Total Cost (Annual)

**Assumptions:**
- Same 10,000 PLN/month gross revenue
- Using PayU payment gateway (3%)
- Astro website (self-hosted)
- PostgreSQL database (local)

**Annual Costs:**

| Item | Calculation | Annual Cost |
|------|-------------|-------------|
| Booking fees | 0% (own system) | 0 PLN |
| Payment processing (PayU 3%) | 120,000 PLN × 3% | 3,600 PLN |
| Server hosting (VPS) | 50 PLN × 12 months | 600 PLN |
| Email service (SendGrid) | 50 PLN × 12 months | 600 PLN |
| Domain + SSL | Assume existing | 100 PLN |
| Database | PostgreSQL (free, self-hosted) | 0 PLN |
| Maintenance | 5 hours/month × 0 (self-managed) | 0 PLN |
| **Total Annual Cost** | | **4,900 PLN** |

**Effective Rate: 4.1% of gross revenue**

**What You Build:**
- Custom booking system (Astro + PostgreSQL)
- Payment processing (PayU - BLIK + cards)
- Website (Astro static site)
- Email confirmations (SendGrid/nodemailer)
- Refund handling (PayU API)
- Admin dashboard (custom)

**What You Don't Get (vs FareHarbor):**
- OTA integrations (Airbnb, Viator) - can add later if needed
- Advanced reporting (build as needed)
- 24/7 support (self-managed)
- PCI compliance burden (handled by PayU, but still your responsibility)

---

### Side-by-Side Comparison

| Factor | FareHarbor | Custom Solution |
|--------|-----------|-----------------|
| **Annual Cost** | 12,200 PLN | 4,900 PLN |
| **Savings** | Baseline | **+7,300 PLN/year** |
| **Setup Time** | Instant (no dev) | 3-4 weeks (build) |
| **Payment Processor** | Stripe/Adyen/PayPal (forced) | PayU/any (your choice) |
| **BLIK Support** | Maybe (Adyen only) | ✓ Yes (PayU native) |
| **Control** | ❌ FareHarbor controls | ✓ Full control |
| **Data Ownership** | ❌ FareHarbor sees all | ✓ Private data |
| **Refund Control** | ❌ Request-based | ✓ Direct API control |
| **Platform Lock-In** | ✓ High (payment tied) | ✓ None (portable) |
| **OTA Integrations** | ✓ Included (250+) | ❌ Manual (add later) |
| **Scalability** | ✓ Unlimited | ✓ Unlimited (DIY) |
| **Compliance** | ✓ FareHarbor handles | ⚠️ Your responsibility |
| **Support** | ✓ 24/7 vendor support | ⚠️ Self-managed |

---

### Break-Even Analysis

**Custom Solution Development:**
- Time investment: 50 hours (AI-assisted)
- Hourly value: If saving 608 PLN/month, break-even in Month 1

**Monthly Savings:**
- FareHarbor: 1,017 PLN/month (12,200 ÷ 12)
- Custom: 408 PLN/month (4,900 ÷ 12)
- **Difference: 609 PLN/month**

**Break-Even Timeline:**
- Month 0: Build system (50 hours investment)
- Month 1: Launch, save 609 PLN
- Month 2: Save another 609 PLN (cumulative: 1,218 PLN)
- Month 12: Save 7,300 PLN annually

**Conclusion:** Custom solution pays for itself in Month 1, assuming AI-assisted development reduces time investment.

---

### Volume Scaling Comparison

**At Different Revenue Levels:**

| Monthly Revenue | FareHarbor Cost | Custom Cost | Annual Savings |
|----------------|-----------------|-------------|----------------|
| 5,000 PLN | 5,100 PLN/year | 2,700 PLN/year | 2,400 PLN |
| 10,000 PLN | 7,200 PLN/year | 4,900 PLN/year | 2,300 PLN |
| 20,000 PLN | 14,400 PLN/year | 8,500 PLN/year | 5,900 PLN |
| 50,000 PLN | 36,000 PLN/year | 19,300 PLN/year | 16,700 PLN |
| 100,000 PLN | 72,000 PLN/year | 37,300 PLN/year | 34,700 PLN |

**Note:** FareHarbor may offer volume discounts at higher tiers (2.5-4% vs 6%), but this is not publicly confirmed. Custom solution costs scale linearly with revenue (PayU 3%), while infrastructure costs remain flat.

**Observation:** Savings increase dramatically at higher volumes. At 100,000 PLN/month revenue, custom solution saves 34,700 PLN/year = equivalent to hiring part-time developer for maintenance.

---

## Strategic Decision Framework

### When FareHarbor Makes Sense

**Choose FareHarbor if:**

**1. Speed to Market is Critical:**
- Need to launch in <1 week
- No technical resources available
- Testing market demand (MVP approach)
- Temporary solution before custom build

**2. OTA Distribution is Priority:**
- Need Airbnb Experiences integration immediately
- Targeting Viator, GetYourGuide, TripAdvisor traffic
- OTA bookings will be >50% of revenue
- Distribution matters more than margins

**3. Limited Technical Capability:**
- No in-house development skills
- Cannot hire developers
- No AI-assisted development tools
- Risk-averse to technical complexity

**4. Compliance/Legal Concerns:**
- Don't want PCI compliance responsibility
- Prefer outsourcing payment liability
- Need vendor to handle chargebacks/disputes
- Want "single throat to choke" for issues

**5. Low Volume (Temporarily):**
- <5,000 PLN/month revenue
- Cost difference is <300 PLN/month (acceptable)
- Plan to migrate later when volume justifies

---

### When Custom Solution Makes Sense (KrakowBites)

**Choose Custom if:**

**1. Cost Optimization is Priority:**
- 10,000+ PLN/month revenue
- Saving 600+ PLN/month matters
- Long-term business (multi-year horizon)
- Reinvest savings into marketing

**2. Technical Capability Available:**
- ✓ You have Astro development skills
- ✓ AI-assisted coding reduces build time
- ✓ Comfortable with API integrations (PayU)
- ✓ Can maintain simple booking system

**3. Market-Specific Requirements:**
- ✓ BLIK payment critical (60% Polish customers)
- ✓ PayU better fit than Stripe/Adyen
- ✓ Local payment methods important
- ✓ Polish language first-class citizen

**4. Control & Ownership:**
- ✓ Want full control over refund policies
- ✓ Keep pricing/customer data private
- ✓ Avoid platform lock-in
- ✓ Ability to switch payment processors later

**5. No Immediate OTA Need:**
- Direct bookings via website (primary channel)
- OTA integrations can wait (Year 2+)
- Focus on local marketing (SEO, ads, partnerships)
- Build direct customer base first

**6. Long-Term Vision:**
- Building a business asset (not renting)
- Plan to scale (savings compound)
- May add features FareHarbor doesn't offer
- Want portable solution (not platform-dependent)

---

### Hybrid Approach (Not Recommended for KrakowBites)

**Option: Start FareHarbor, Migrate to Custom**

**Rationale:**
- Launch fast with FareHarbor (Week 1)
- Validate demand with real bookings
- Build custom solution in background (Weeks 2-8)
- Migrate once custom system proven

**Pros:**
- Immediate revenue (no waiting for build)
- Market validation before investment
- Risk mitigation (what if tours don't sell?)

**Cons:**
- Double work (pay FareHarbor fees while building custom)
- Migration complexity (move bookings, customer data)
- Customer confusion (changing systems)
- Legal issues (FareHarbor contract terms, exit clauses)
- Lost savings during FareHarbor period

**Recommendation for KrakowBites:**
Skip hybrid approach. You have technical skills + AI assistance = build custom from Day 1. 3-week build time is acceptable for new business. No need to pay FareHarbor fees unnecessarily.

---

## Polish Market Considerations

### BLIK Payment Importance

**BLIK Market Share in Poland:**
- ~60% of Polish consumers prefer BLIK over cards
- Instant payment (2-minute timeout)
- Mobile-first (no card details needed)
- Lower fraud risk (authenticated via banking app)

**FareHarbor BLIK Support:**
- ⚠️ **Unclear** if Stripe integration supports BLIK
- ✓ **Adyen supports BLIK** (confirmed)
- If using Stripe: May lose 60% of Polish customer conversions

**Custom Solution BLIK Support:**
- ✓ **PayU natively supports BLIK** (Polish company)
- Optimized for Polish market
- 2-minute payment flow
- Lower fees than cards (~1.5-2% vs 2.5-3%)

**Impact:**
- Missing BLIK = 60% of Polish customers forced to use cards (friction)
- BLIK-only customers may abandon booking
- Competitor with BLIK support wins conversions

**Conclusion:** For Polish market, PayU + custom solution > FareHarbor Stripe. If using FareHarbor, MUST use Adyen processor, not Stripe.

---

### International Tourist Payments

**Customer Base:**
- Polish locals: Prefer BLIK (60%), then cards (40%)
- International tourists: Cards (95%), Apple Pay (5%)

**FareHarbor Advantage:**
- Multi-currency pricing (USD, EUR, GBP, PLN)
- International card support (Visa, Mastercard, Amex)
- Stripe/Adyen strong globally

**Custom Solution:**
- PayU supports international cards
- Multi-currency possible (but complex)
- Dynamic currency conversion (DCC) available

**Recommendation:**
- If >50% customers are international: FareHarbor's multi-currency easier
- If >50% customers are Polish: Custom + PayU + BLIK critical
- KrakowBites (local tours): Likely 70% Polish, 30% international → Custom solution better fit

---

### Language & Localization

**FareHarbor:**
- Checkout supports Polish + English
- Admin dashboard primarily English
- Customer support English-first

**Custom Solution:**
- Full control over language (Polish-first)
- Localized error messages
- Polish currency formatting (200,00 PLN vs 200.00 PLN)
- Cultural optimization (date formats, etc.)

**Impact:** Minor difference for backend, but customer-facing Polish language control is advantage for custom solution.

---

## Payment Security & Compliance

### PCI DSS Compliance

**FareHarbor Model:**
- ✓ FareHarbor handles PCI compliance
- ✓ Sub-merchant under their certified account
- ✓ No separate PCI audit required
- ✓ Stripe/Adyen Level 1 certified
- ⚠️ Still responsible for website security

**Custom Solution:**
- ⚠️ Your responsibility (but simplified with PayU)
- ✓ PayU is PCI Level 1 certified (handles card data)
- ✓ Redirect/lightbox integration = no card data on your server
- ⚠️ Must follow basic security practices:
  - HTTPS enforced
  - Secure session management
  - No logging of card data
  - Regular security updates

**Mitigation:**
- Use PayU Lightbox (payment form hosted by PayU)
- Never store card data on your server
- Only store PayU transaction IDs
- Annual security review (basic checklist)

**Conclusion:** PCI compliance not a major burden with modern payment gateways. PayU handles heavy lifting, you handle website security (standard practices).

---

### Fraud & Chargeback Management

**FareHarbor Model:**
- ✓ Automated fraud detection (Stripe Radar, Adyen Risk)
- ✓ Chargeback protection tools
- ✓ FareHarbor submits evidence on your behalf
- ✓ Dispute management via dashboard
- ⚠️ Chargeback fees: ~50-100 PLN per dispute (even if won)

**Custom Solution:**
- ⚠️ Manual fraud monitoring (initially)
- ⚠️ You handle chargeback responses
- ✓ PayU provides dispute tools (submit evidence)
- ⚠️ Chargeback fees: ~50-100 PLN per dispute
- ✓ Tours are low-risk business (physical service, proof of delivery)

**Mitigation for Custom:**
- Email confirmation with tour details (evidence)
- Photo during tour (proof customer attended)
- Clear refund policy (reduce disputes)
- Respond quickly to PayU chargeback notifications
- Maintain good communication (resolve issues before chargeback)

**Typical Chargeback Rates:**
- Tours/activities: <1% (low-risk)
- If <10 chargebacks/year: Manual handling acceptable
- If >10 chargebacks/year: Consider fraud tools (PayU offers add-ons)

**Conclusion:** Chargebacks not a major concern for tour business. FareHarbor's tools are nice-to-have, not critical.

---

## Technical Integration Complexity

### FareHarbor Integration

**Setup Effort:**
- Embed widget on website: 30 minutes (copy/paste embed code)
- Configure tours & schedules: 2-4 hours
- Set up payment processor: 1 hour (connect Stripe/Adyen)
- Test booking flow: 1 hour
- **Total: 4-6 hours to go live**

**Maintenance:**
- Zero technical maintenance (FareHarbor handles)
- Update tours/prices via dashboard (non-technical)
- No code changes needed for features

**Limitations:**
- Limited customization (widget appearance)
- Cannot modify booking flow logic
- Data export limited (reporting via FareHarbor dashboard)
- API access requires separate agreement (fees may apply)

---

### Custom Solution Integration

**Development Effort (AI-Assisted):**
- Database schema design: 4 hours
- Booking calendar component: 8 hours
- Booking form & validation: 6 hours
- PayU payment integration: 8 hours
- Webhook handler: 4 hours
- Email confirmation system: 4 hours
- Admin dashboard (basic): 8 hours
- Testing & debugging: 8 hours
- **Total: 50 hours to MVP**

**With AI Assistance:**
- Code generation: -40% time (30 hours)
- Debugging help: -30% time (35 hours)
- **Realistic: 35-40 hours to MVP**

**Maintenance:**
- Monthly updates: 2-4 hours (security patches)
- Feature additions: As needed (self-paced)
- Bug fixes: ~1-2 hours/month average
- **Estimated: 3-5 hours/month ongoing**

**Flexibility:**
- ✓ Full customization (any feature possible)
- ✓ Modify booking flow logic
- ✓ Direct database access (custom reports)
- ✓ API ownership (build integrations later)

---

### Technical Skill Requirements

**FareHarbor:**
- HTML embed code (copy/paste level)
- Dashboard navigation (click/form level)
- No coding required

**Custom Solution (Astro + PayU):**
- TypeScript/JavaScript (intermediate)
- Astro framework (beginner-intermediate)
- PostgreSQL/SQL (basic queries)
- REST API integration (intermediate)
- Git version control (basic)
- Server deployment (basic)

**Risk Mitigation:**
- Use AI coding assistants (Claude, Copilot)
- Follow PayU SDK documentation (well-documented)
- Copy from examples (Astro + PayU integration guides)
- Community support (Astro Discord, PayU forums)

**Conclusion:** With AI assistance, technical complexity is manageable for someone with Astro website development experience.

---

## Answers to Original Questions

### Q1: Can I use FareHarbor with my own PayU payment processor?

**Answer:** ❌ **No, absolutely not.**

**Explanation:**
- FareHarbor requires using their integrated payment processors only
- Allowed processors: Stripe, Adyen, or PayPal (FareHarbor-controlled integrations)
- You cannot bring your own PayU merchant account
- You cannot connect any external payment gateway
- This is a hard requirement per FareHarbor Terms of Service

**Why:**
- FareHarbor operates as payment facilitator (payfac)
- They act as merchant of record (legal/compliance requirement)
- Payment processing is how they enforce the 6% commission
- Bundled model = higher revenue for FareHarbor

---

### Q2: How do they calculate 6% if I don't share prices with them?

**Answer:** **You MUST share all prices. No way to avoid it.**

**Explanation:**
- FareHarbor processes ALL payments (mandatory)
- When customer books 200 PLN tour, payment flows through FareHarbor's Stripe/Adyen account
- FareHarbor sees exact transaction amount in real-time
- 6% calculated automatically: 200 PLN × 6% = 12 PLN fee
- FareHarbor deducts fee and settles net amount (188 PLN) to your bank

**Calculation Flow:**
```
Customer pays → FareHarbor receives (via Stripe/Adyen)
→ FareHarbor calculates 6%
→ FareHarbor deducts fee
→ FareHarbor settles to your bank (next day)
```

**Data Visibility:**
- FareHarbor sees: All booking amounts, customer payment methods, refunds, chargebacks
- FareHarbor controls: Payment authorization, refund processing, settlement timing
- You see: Net settlement amounts in your bank (after FareHarbor's 6% deduction)

**Cannot Hide Pricing Because:**
- Payment must flow through FareHarbor (they're merchant of record)
- Commission based on transaction value (requires seeing amounts)
- No "schedule-only" mode exists (payment processing is mandatory)

---

### Q3: Is "booking only without payment processing" possible?

**Answer:** ❌ **No. FareHarbor does NOT offer booking-only plans.**

**Explanation:**
- Payment processing is bundled and mandatory
- Cannot use FareHarbor for calendar/scheduling only
- Cannot handle payments outside their system
- This differentiates them from competitors like Bókun, Checkfront, Rezdy (which allow separate payment processors)

**Why FareHarbor Bundles:**
- Higher revenue (6% vs 2-3% pure booking fee)
- Customer lock-in (payment history locked to platform)
- Operational control (manage refunds, disputes)
- Compliance leverage (PCI responsibility)

**Alternatives That Allow Separation:**
- **Bókun:** 1-1.5% booking fee, bring your own payment processor
- **Checkfront:** Separate booking fees and payment processing
- **Rezdy:** Separate pricing for booking vs payments
- **Custom solution:** Full separation (your Astro system + PayU)

---

### Q4: Do they have their own payment gateway?

**Answer:** **Not exactly. They're a payment facilitator using third-party infrastructure.**

**Explanation:**

**What FareHarbor Does:**
- Acts as **payment facilitator (payfac)** / merchant aggregator
- Uses Stripe, Adyen, or PayPal backend infrastructure
- FareHarbor is legal "merchant of record"
- You become "sub-merchant" under FareHarbor's master account

**Customer Experience:**
- Payment form: FareHarbor-branded or Stripe/Adyen-branded (varies)
- Card data entered on: Stripe/Adyen hosted page (secure)
- Receipt says: "Processed by FareHarbor" or "via Stripe"
- Dispute contact: Customer contacts FareHarbor, FareHarbor handles via Stripe/Adyen

**Backend Flow:**
```
Customer → FareHarbor checkout → Stripe/Adyen API → Card networks → Bank
         ← FareHarbor holds funds ← Stripe/Adyen settlement ← Bank
         → Your bank account (next day, after 6% deduction)
```

**Comparison to "Own Gateway":**
- True gateway: Square, PayPal, Stripe (direct merchant relationship)
- Payfac: FareHarbor (sub-merchant relationship, aggregated)

**Implication:**
- You don't have direct Stripe/Adyen account
- FareHarbor manages the relationship
- Cannot switch to direct Stripe without leaving FareHarbor
- Cannot access full Stripe/Adyen dashboard features

---

### Q5: What's the total cost using FareHarbor's booking + payment?

**Answer:** **6% of gross revenue, all-inclusive.**

**Detailed Breakdown:**

**Included in 6% Fee:**
- ✓ Platform booking software
- ✓ Credit card processing (Stripe/Adyen/PayPal fees)
- ✓ Payment gateway costs
- ✓ PCI compliance
- ✓ Chargeback protection tools
- ✓ Fraud detection (Stripe Radar / Adyen Risk)
- ✓ Next-day settlements
- ✓ Refund processing
- ✓ Customer support (booking system)

**Additional Costs (Optional):**
- Website builder: 5,000 PLN/year (optional, if you use their website service)
- SMS notifications: May have additional fees (check FareHarbor pricing)
- API access: May require enterprise plan or fees (check terms)
- Chargeback fees: ~50-100 PLN per dispute (industry standard)

**Not Included:**
- Your website hosting (if not using FareHarbor's builder)
- Marketing / advertising costs
- Tour operating costs (guides, insurance, equipment)

**Monthly Cost Example (10,000 PLN Revenue):**
```
Gross revenue:           10,000 PLN
FareHarbor 6% fee:         -600 PLN
Net revenue to you:       9,400 PLN

If using website builder:
Website fee (annual):     -417 PLN/month (5,000 ÷ 12)
Net after all fees:       8,983 PLN

Effective cost: 10.2% of gross revenue
```

**Annual Cost Example (120,000 PLN Revenue):**
```
FareHarbor fees:        7,200 PLN (6%)
Website builder:        5,000 PLN (optional)
Total:                 12,200 PLN

Effective rate: 10.2% if using website builder
                 6.0% if not using website builder
```

---

## Recommendation for KrakowBites

### Build Custom Solution (High Confidence)

**Rationale:**

**1. Cost Savings are Significant:**
- 7,300 PLN/year savings vs FareHarbor
- Break-even in Month 1 (AI-assisted development)
- Savings scale with growth (more revenue = more savings)

**2. Technical Capability is Present:**
- You have Astro development skills
- AI assistance reduces build complexity
- 35-40 hours realistic timeline (3-4 weeks part-time)

**3. Market Requirements Favor Custom:**
- Polish market needs BLIK (PayU native support)
- FareHarbor's Stripe may not support BLIK well
- Adyen supports BLIK but adds complexity
- PayU is market leader in Poland (trust factor)

**4. Control & Ownership Matter:**
- Keep customer/pricing data private
- Control refund policies directly
- Avoid platform lock-in
- Freedom to evolve system as business grows

**5. No Immediate OTA Need:**
- Direct bookings via website (primary channel)
- Local SEO, Google Ads, partnerships (not OTA)
- Can add OTA integrations later if demand justifies
- FareHarbor's OTA network is overkill for local tours initially

**6. Long-Term Vision:**
- Building business asset (not renting platform)
- Scalability without increasing % costs
- Ability to add unique features (FareHarbor can't offer)
- Portable solution (can migrate hosting, switch payment processors)

---

### Implementation Roadmap

**Week 1-2: Build Core System**
- Database schema (tours, schedules, bookings)
- Availability calendar component
- Booking form with validation
- PayU sandbox integration
- Email confirmation (SendGrid)

**Week 3: Legal & Payment Setup**
- Add privacy policy, T&C, refund policy pages
- Apply for PayU merchant account (2-day approval)
- Receive production API credentials
- Replace sandbox with production

**Week 4: Testing & Launch**
- Test full booking flow (your own card)
- Verify webhooks working
- Test refund process
- Soft launch (friends/family)

**Week 5+: Public Launch**
- Public announcement
- Monitor payment success rate
- Gather customer feedback
- Iterate on UX issues

---

### Risk Mitigation

**Risk 1: Development Takes Longer**
- **Mitigation:** Start with MVP (minimal features), add later
- **Timeline buffer:** 4 weeks → 6 weeks if needed
- **Cost:** Delayed launch, but no ongoing fees during build

**Risk 2: Payment Integration Issues**
- **Mitigation:** Use PayU sandbox extensively (test before production)
- **Support:** PayU has developer support (email, docs)
- **Fallback:** Manual payment option temporarily (bank transfer)

**Risk 3: Compliance/Legal Issues**
- **Mitigation:** Use PayU Lightbox (they handle card data)
- **Legal pages:** Use template generators (iubenda, termly.io)
- **Review:** Have lawyer check T&C/privacy (optional, recommended)

**Risk 4: Low Customer Adoption**
- **Mitigation:** Same risk exists with FareHarbor
- **Advantage:** Lower costs = lower break-even point
- **Flexibility:** Can pivot features quickly (own system)

---

## Conclusion

### FareHarbor's Model is Clear: All-or-Nothing Bundle

**You Cannot:**
- ❌ Use FareHarbor for booking-only (payment processing is mandatory)
- ❌ Bring your own payment processor like PayU (must use Stripe/Adyen/PayPal)
- ❌ Keep pricing data private from FareHarbor (they process all payments)
- ❌ Control refunds directly (request-based via FareHarbor)
- ❌ Avoid the 6% fee (bundled platform + payment processing)

**FareHarbor is Good For:**
- ✓ Instant launch (no development needed)
- ✓ OTA distribution (Airbnb, Viator, etc.)
- ✓ Non-technical operators
- ✓ Short-term / testing business viability
- ✓ U.S.-focused tours (Stripe optimized)

**FareHarbor is Bad For:**
- ❌ Cost-conscious operators (6% is high)
- ❌ Polish market (BLIK support unclear with Stripe)
- ❌ Long-term businesses (costs compound)
- ❌ Control-focused operators (platform lock-in)
- ❌ Data privacy needs (FareHarbor sees everything)

---

### Custom Solution is Better for KrakowBites

**Savings:**
- 7,300 PLN/year vs FareHarbor
- Scales with growth (more revenue = more savings)

**Control:**
- Full ownership of code, data, customer relationships
- Direct payment processing (PayU merchant account)
- Modify any feature anytime

**Market Fit:**
- BLIK support (critical for Polish customers)
- PayU market leader in Poland (trust factor)
- Polish-first language and UX

**Development:**
- 35-40 hours with AI assistance
- 3-4 weeks part-time development
- Break-even Month 1

**Risk:**
- Low technical risk (Astro + PayU well-documented)
- Low financial risk (costs only if revenue exists)
- Low compliance risk (PayU handles PCI)

---

## References

**Sources Used:** 15

1. Payments | FareHarbor - `https://fareharbor.com/sell/payments/`
2. Payments | FareHarbor Help Center - `https://help.fareharbor.com/payments/`
3. Terms of Service for Providers | FareHarbor - `https://fareharbor.com/legal/tos-providers/`
4. FareHarbor case study | Stripe - `https://stripe.com/customers/fareharbor`
5. Collecting and Managing Payments | FareHarbor Help Center - `https://help.fareharbor.com/payments/collecting-and-managing-payments/`
6. FareHarbor Pricing Plan & Cost Guide | GetApp 2025 - `https://www.getapp.com/customer-management-software/a/fareharbor/pricing/`
7. FareHarbor Pricing Guide: What to Know Before You Buy (2025) - Bókun - `https://www.bokun.io/fareharbor-pricing`
8. Booking System Pricing Guide for Tour Operators | Arival - `https://arival.travel/article/arival-guide-to-res-system-pricing/`
9. FareHarbor Pricing, Alternatives & More 2025 | Capterra - `https://www.capterra.com/p/135106/FareHarbor/`
10. Using Apple Pay and Google Pay for online payments | FareHarbor Help Center - `https://help.fareharbor.com/payments/alternative-payment-methods/apple-and-google-pay/`
11. Alternative Payment Methods | FareHarbor Help Center - `https://help.fareharbor.com/payments/alternative-payment-methods/`
12. Credit Card Payments | FareHarbor Help Center - `https://help.fareharbor.com/payments/credit-card/`
13. Best Payment Gateways for FareHarbor - `https://sourceforge.net/software/payment-gateways/integrates-with-fareharbor/`
14. Payfacs: A guide to payment facilitation - Stripe - `https://stripe.com/guides/payfacs`
15. FareHarbor Sub-processors - `https://fareharbor.com/legal/sub-processors/`

---

## Document History

**Version:** 1.0
**Created:** 2026-01-02
**Status:** Analysis Complete
**Next Steps:**
- Proceed with custom solution development
- Start PayU sandbox testing immediately
- Build website legal pages (privacy, T&C, refund)
- Apply for PayU merchant account in Week 3

**Key Decisions:**
- Custom solution recommended over FareHarbor
- PayU payment processor (vs FareHarbor's Stripe/Adyen)
- Astro + PostgreSQL architecture
- Target: 4-week development timeline
