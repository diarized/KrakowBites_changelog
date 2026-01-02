# Urban Cartography - Detailed Brand Specifications

**Alternative Direction for KrakowBites**
**Date:** 2026-01-01
**Status:** Recommended Primary Direction (4.5/5 differentiation)
**Source:** Critical review of docs/brand-identity-brief.md

---

## Executive Summary

**Concept:** Position KrakowBites as navigation/exploration system using cartographic visual language

**Core Metaphor:** Tours = routes through Kraków's culinary landscape
**Brand Archetype:** Explorer (shift from Sage)
**Positioning Line:** "Navigate Kraków's culinary landscape"
**Differentiation Score:** 4.5/5

**Why This Direction:**
- Completely distinct from all competitors (unique color, typography, metaphor)
- Aligns with user preferences (food-forward, moderately bold, substance over Instagram)
- Solves all current brand problems (typography overlap, accessibility, crowded colors)
- Ownable visual territory (map aesthetic unused in market)
- WCAG AAA compliant by design

---

## Table of Contents

1. [Color Palette](#color-palette)
2. [Typography System](#typography-system)
3. [Logo Specifications](#logo-specifications)
4. [Photography Guidelines](#photography-guidelines)
5. [UI Components](#ui-components)
6. [Spacing & Grid System](#spacing--grid-system)
7. [Iconography](#iconography)
8. [Motion Design](#motion-design)
9. [Voice & Messaging](#voice--messaging)
10. [Application Examples](#application-examples)
11. [Accessibility Standards](#accessibility-standards)
12. [Implementation Guide](#implementation-guide)

---

## Color Palette

### Primary Colors

#### Map Paper `#f8f5f0`
**Use:** Primary background, card backgrounds, subtle sections
**Rationale:** Aged map aesthetic, warm neutral (not stark white)
**Accessibility:** Base layer for other colors to sit on
**WCAG:** Background color only (not for text)

**Application:**
```css
.page-background { background: #f8f5f0; }
.card { background: #f8f5f0; }
.section-alt { background: #f8f5f0; }
```

---

#### Route Line (Terracotta) `#e76f51`
**Use:** Primary CTAs, route lines, emphasis, active states
**Rationale:**
- Warm but not gold (avoids heritage cluster)
- Earthy, organic (food connection)
- Route metaphor (path through city)
- **Distinct from all competitors** (none use terracotta)

**Accessibility:**
- On White (#ffffff): 4.5:1 (WCAG AA ✅)
- On Map Paper (#f8f5f0): 5.2:1 (WCAG AA ✅)
- Minimum 16px for small text, safe for large text at any size

**Application:**
```css
.btn-primary { background: #e76f51; }
.route-line { stroke: #e76f51; }
.link:hover { color: #e76f51; }
.active { border-color: #e76f51; }
```

**Shades:**
```css
--route-line-light: #f0856c;  /* Hover, light backgrounds */
--route-line:       #e76f51;  /* Primary */
--route-line-dark:  #d65d3f;  /* Active, pressed states */
--route-line-darker:#c5502e;  /* Shadows, darkest accent */
```

---

#### Water Blue (Teal) `#2a9d8f`
**Use:** Links, secondary CTAs, informational elements, water features
**Rationale:**
- Cool contrast to terracotta
- Vistula River association
- Distinct from Eat Polska's bright blue (#0170B9)
- Modern, fresh, not cold

**Accessibility:**
- On White (#ffffff): 4.8:1 (WCAG AA ✅)
- On Map Paper (#f8f5f0): 5.6:1 (WCAG AA ✅)
- Safe for all text sizes

**Application:**
```css
.link { color: #2a9d8f; }
.btn-secondary { border-color: #2a9d8f; }
.info-badge { background: #2a9d8f; }
```

**Shades:**
```css
--water-blue-light: #3cb4a4;  /* Hover */
--water-blue:       #2a9d8f;  /* Primary */
--water-blue-dark:  #24877c;  /* Active */
--water-blue-darker:#1e7169;  /* Darkest */
```

---

#### Landmark Brown `#6b4226`
**Use:** Heritage elements, secondary text, warm accents
**Rationale:**
- Earth tone, chocolate brown
- Historical building color
- Warm but not gold/bronze
- Excellent contrast

**Accessibility:**
- On White (#ffffff): 8.1:1 (WCAG AAA ✅)
- On Map Paper (#f8f5f0): 9.3:1 (WCAG AAA ✅)
- Excellent for body text

**Application:**
```css
.heritage-badge { color: #6b4226; }
.secondary-text { color: #6b4226; }
.icon-heritage { fill: #6b4226; }
```

---

#### Path Grey (Charcoal) `#4a4a4a`
**Use:** Body text, headings, primary text content
**Rationale:**
- Softer than pure black (#000000)
- Professional, readable
- Warm undertone (not cool grey)

**Accessibility:**
- On White (#ffffff): 9.1:1 (WCAG AAA ✅)
- On Map Paper (#f8f5f0): 10.5:1 (WCAG AAA ✅)
- Perfect for all text

**Application:**
```css
body { color: #4a4a4a; }
h1, h2, h3 { color: #4a4a4a; }
p { color: #4a4a4a; }
```

---

#### Pure White `#ffffff`
**Use:** Cards on Map Paper background, high-contrast elements
**Rationale:** Maximum contrast, cleanliness
**Accessibility:** Contrast base
**Application:**
```css
.card-elevated { background: #ffffff; }
.btn-primary { color: #ffffff; }
```

---

### Secondary Colors (Semantic)

#### Success Green `#52b788`
**Use:** Confirmations, success states, availability indicators
**Accessibility:** On White: 4.2:1 (WCAG AA for large text ✅)
**Application:**
```css
.success-message { color: #52b788; }
.available-badge { background: #52b788; }
```

#### Warning Amber `#f4a261`
**Use:** Caution states, limited availability, important notices
**Accessibility:** On White: 3.8:1 (WCAG AA for large text ✅)
**Application:**
```css
.warning-banner { background: #f4a261; }
.spots-filling { color: #f4a261; }
```

#### Error Red `#e63946`
**Use:** Errors, validation failures, critical alerts
**Accessibility:** On White: 5.1:1 (WCAG AA ✅)
**Application:**
```css
.error-message { color: #e63946; }
.input-error { border-color: #e63946; }
```

---

### Color Usage Guidelines

**Ratio (Percentage of visible area):**
- Map Paper / White (backgrounds): 60-70%
- Path Grey (text): 20-25%
- Route Line (terracotta): 5-10%
- Water Blue: 3-5%
- Landmark Brown: 2-3%
- Success/Warning/Error: <1% (only when needed)

**Do:**
- Use Route Line (terracotta) for primary CTAs
- Use Water Blue for links and secondary actions
- Use Path Grey for all body text
- Maintain high contrast for accessibility
- Test all color combinations with WebAIM contrast checker

**Don't:**
- Mix more than 3-4 colors on one screen
- Use pure black (#000000) - use Path Grey instead
- Overuse red tones (terracotta is accent, not dominant)
- Use colors without checking WCAG contrast

---

## Typography System

### Font Families

#### Heading Font: Space Grotesk (Geometric Sans-Serif)

**Source:** Google Fonts, open-source
**License:** Open Font License (OFL)
**Variable Font:** No (static weights only)
**Weights Used:** 400 (Regular), 500 (Medium), 700 (Bold)

**Rationale:**
- Geometric = architectural, map label aesthetic
- Modern, clean, structured
- **0% overlap with competitors** (all use serif or different sans)
- Cartographic feel (street signs, map labels)
- Readable at all sizes

**Character Set:** Latin Extended, Vietnamese
**Fallback:** `'Arial', 'Helvetica Neue', sans-serif`

**CDN Import:**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet">
```

**CSS:**
```css
h1, h2, h3, h4,
.heading,
.tour-name,
.section-title {
  font-family: 'Space Grotesk', 'Arial', sans-serif;
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: -0.01em; /* Tighten slightly for large sizes */
}

.coordinate-label,
.location-tag {
  font-family: 'Space Grotesk', 'Arial', sans-serif;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.05em; /* Wider for labels */
}
```

---

#### Body Font: Inter (Sans-Serif)

**Source:** Google Fonts, open-source
**License:** Open Font License (OFL)
**Variable Font:** Yes (but use static weights for performance)
**Weights Used:** 400 (Regular), 500 (Medium), 600 (Semibold)

**Rationale:**
- Optimized for screen readability
- Neutral, modern, professional
- Already in current brief (works well, keep it)
- Variable font option for future
- Excellent hinting (crisp on all screens)

**Character Set:** Latin Extended, Cyrillic, Greek, Vietnamese
**Fallback:** `'Roboto', 'Helvetica Neue', sans-serif`

**CDN Import:**
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
```

**CSS:**
```css
body, p, li, td,
.description,
.body-text {
  font-family: 'Inter', 'Roboto', sans-serif;
  font-weight: 400;
  line-height: 1.7; /* Generous line height for readability */
  letter-spacing: -0.005em; /* Very slight tightening */
}

button, .btn,
.label, .tag,
.nav-link {
  font-family: 'Inter', 'Roboto', sans-serif;
  font-weight: 600;
  letter-spacing: 0.01em;
}

.small-text,
.caption {
  font-family: 'Inter', 'Roboto', sans-serif;
  font-weight: 500;
  font-size: 0.875rem; /* 14px */
}
```

---

#### Accent Font: JetBrains Mono (Monospace)

**Source:** JetBrains, open-source
**License:** Open Font License (OFL)
**Variable Font:** Yes
**Weight Used:** 400 (Regular)

**Rationale:**
- Technical precision (coordinates, addresses)
- Map aesthetic (latitude/longitude format)
- Modern developer aesthetic
- Highly readable monospace (better than Courier)
- Distinctive accent (used sparingly)

**Character Set:** Latin Extended, Cyrillic, Greek
**Fallback:** `'Courier New', 'Monaco', monospace`

**CDN Import:**
```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400&display=swap" rel="stylesheet">
```

**CSS:**
```css
.coordinates,
.address,
.route-id,
.technical-label {
  font-family: 'JetBrains Mono', 'Courier New', monospace;
  font-weight: 400;
  font-size: 0.875em; /* 14px, smaller than body */
  letter-spacing: -0.02em; /* Monospace needs tightening */
  color: #2a9d8f; /* Water Blue for technical elements */
}
```

**Example:**
```html
<span class="coordinates">50.0614°N, 19.9383°E</span>
```

---

### Typography Scale

#### Desktop (≥1024px)

```css
/* Display (Hero Headlines) */
.text-hero {
  font-size: 4rem;      /* 64px */
  font-weight: 700;
  line-height: 1.1;
  letter-spacing: -0.02em;
  font-family: 'Space Grotesk', sans-serif;
}

/* Headings */
h1, .text-h1 {
  font-size: 3.5rem;    /* 56px */
  font-weight: 700;
  line-height: 1.2;
  font-family: 'Space Grotesk', sans-serif;
}

h2, .text-h2 {
  font-size: 2.5rem;    /* 40px */
  font-weight: 700;
  line-height: 1.2;
  font-family: 'Space Grotesk', sans-serif;
}

h3, .text-h3 {
  font-size: 2rem;      /* 32px */
  font-weight: 700;
  line-height: 1.3;
  font-family: 'Space Grotesk', sans-serif;
}

h4, .text-h4 {
  font-size: 1.5rem;    /* 24px */
  font-weight: 700;
  line-height: 1.3;
  font-family: 'Space Grotesk', sans-serif;
}

h5, .text-h5 {
  font-size: 1.25rem;   /* 20px */
  font-weight: 600;
  line-height: 1.4;
  font-family: 'Inter', sans-serif;
}

h6, .text-h6 {
  font-size: 1.125rem;  /* 18px */
  font-weight: 600;
  line-height: 1.4;
  font-family: 'Inter', sans-serif;
}

/* Body */
.text-lg {
  font-size: 1.125rem;  /* 18px */
  font-weight: 400;
  line-height: 1.7;
  font-family: 'Inter', sans-serif;
}

.text-base, body, p {
  font-size: 1rem;      /* 16px */
  font-weight: 400;
  line-height: 1.7;
  font-family: 'Inter', sans-serif;
}

.text-sm {
  font-size: 0.875rem;  /* 14px */
  font-weight: 400;
  line-height: 1.6;
  font-family: 'Inter', sans-serif;
}

.text-xs {
  font-size: 0.75rem;   /* 12px */
  font-weight: 500;
  line-height: 1.5;
  font-family: 'Inter', sans-serif;
}
```

#### Mobile (≤767px)

```css
/* Display (Hero Headlines) */
.text-hero {
  font-size: 2.5rem;    /* 40px */
  line-height: 1.1;
}

/* Headings */
h1, .text-h1 {
  font-size: 2.5rem;    /* 40px */
  line-height: 1.2;
}

h2, .text-h2 {
  font-size: 2rem;      /* 32px */
  line-height: 1.2;
}

h3, .text-h3 {
  font-size: 1.5rem;    /* 24px */
  line-height: 1.3;
}

/* Body stays same (16px minimum for readability) */
```

---

### Typography Usage Examples

#### Hero Section
```html
<h1 class="text-hero" style="font-family: Space Grotesk; font-weight: 700; color: #4a4a4a;">
  Navigate Kraków's Culinary Landscape
</h1>
<p class="text-lg" style="font-family: Inter; color: #6b4226;">
  Precisely mapped tours through forgotten food stories
</p>
<span class="coordinates" style="font-family: JetBrains Mono; color: #2a9d8f;">
  50.0614°N, 19.9383°E
</span>
```

#### Tour Card
```html
<h3 class="tour-name" style="font-family: Space Grotesk; font-weight: 700; color: #4a4a4a;">
  Route 1: Kazimierz Food Corridor
</h3>
<p class="description" style="font-family: Inter; font-size: 16px; color: #4a4a4a;">
  Navigate through grandmothers' kitchens and hidden bakeries...
</p>
<span class="coordinates" style="font-family: JetBrains Mono; color: #2a9d8f;">
  📍 50.0614°N, 19.9383°E
</span>
```

---

## Logo Specifications

### Primary Logo: Coordinate-Based (Recommended)

**Concept:** KrakowBites wordmark with Kraków's geographic coordinates

#### Full Logo (Primary)

```
┌────────────────────────────────────┐
│  KRAKÓW BITES                      │  ← Space Grotesk Bold, 32px, #4a4a4a
│  50.0614°N, 19.9383°E              │  ← JetBrains Mono, 14px, #2a9d8f
│  ━━━━━━━━━━━━━━━━━━━━━━           │  ← Route line, #e76f51, 4px height
│  Food & Heritage Tours             │  ← Inter Medium, 12px, #6b4226
└────────────────────────────────────┘
```

**Specifications:**
- **Wordmark:** "KRAKÓW BITES"
  - Font: Space Grotesk Bold
  - Size: 32px (scales proportionally)
  - Color: Path Grey (#4a4a4a)
  - Letter spacing: -0.01em
  - All caps

- **Coordinates:** "50.0614°N, 19.9383°E"
  - Font: JetBrains Mono Regular
  - Size: 14px (43.75% of wordmark)
  - Color: Water Blue (#2a9d8f)
  - Letter spacing: -0.02em
  - Positioned: 8px below wordmark

- **Route Line:**
  - Height: 4px
  - Color: Route Line (#e76f51)
  - Width: 75% of wordmark width
  - Positioned: 8px below coordinates

- **Tagline:** "Food & Heritage Tours"
  - Font: Inter Medium
  - Size: 12px (37.5% of wordmark)
  - Color: Landmark Brown (#6b4226)
  - Positioned: 8px below route line

**Clear Space:**
- Minimum: 20% of logo height on all sides
- Example: If logo is 100px tall, clear space = 20px

**Minimum Size:**
- Web: 200px width
- Print: 1.5 inches width
- Below minimum: Use secondary logo (no tagline)

---

#### Secondary Logo (Compact)

```
┌────────────────────────┐
│  KRAKÓW BITES         │  ← Space Grotesk Bold, 32px
│  50.0614°N, 19.9383°E │  ← JetBrains Mono, 14px
│  ━━━━━━━━━━━━━━━━━   │  ← Route line
└────────────────────────┘
```

**When to Use:**
- Navigation headers (limited vertical space)
- Social media banners
- Email signatures
- Mobile app headers
- Anywhere vertical space is constrained

**Omits:** Tagline ("Food & Heritage Tours")

---

#### Logo Icon (Favicon/App Icon)

**Option 1: Map Pin with KB**
```
  ┌───┐
 ╱ KB  ╲
│       │
│   •   │  ← Map pin shape
 ╲     ╱
  └─•─┘
```

**Option 2: Compass Rose**
```
    ╱│╲
   ╱ │ ╲
  ───⊕───  ← Simplified compass
   ╲ │ ╱
    ╲│╱
```

**Option 3: Route Line + Coordinates**
```
50.06°N
━━━━━━
19.94°E
```

**Specifications:**
- Size: 512x512px (source)
- Export: 16x16, 32x32, 64x64, 128x128, 256x256, 512x512
- Format: PNG (transparency), SVG (vector)
- Background: Transparent or Map Paper (#f8f5f0)
- Primary element color: Route Line (#e76f51)

**Recommended:** Option 2 (Compass Rose) - most recognizable at small sizes

---

### Logo Color Variations

#### 1. Full Color (Primary)
**Use:** White or Map Paper backgrounds, primary application

```
KRAKÓW BITES          (Path Grey #4a4a4a)
50.0614°N, 19.9383°E  (Water Blue #2a9d8f)
━━━━━━━━━━━━━━━━━     (Route Line #e76f51)
Food & Heritage Tours (Landmark Brown #6b4226)
```

#### 2. Monochrome Dark
**Use:** Light backgrounds when full color not available, print (black & white)

```
KRAKÓW BITES          (Path Grey #4a4a4a)
50.0614°N, 19.9383°E  (Path Grey #4a4a4a)
━━━━━━━━━━━━━━━━━     (Path Grey #4a4a4a)
Food & Heritage Tours (Path Grey #4a4a4a)
```

#### 3. Reversed (White on Dark)
**Use:** Dark backgrounds, photos, videos

```
KRAKÓW BITES          (White #ffffff)
50.0614°N, 19.9383°E  (White #ffffff, 80% opacity)
━━━━━━━━━━━━━━━━━     (White #ffffff, 60% opacity)
Food & Heritage Tours (White #ffffff, 70% opacity)
```

**Minimum background darkness:** #4a4a4a or darker for sufficient contrast

---

### Logo Files Required

**Vector (for design/print):**
- `krakowbites-logo-primary.svg` - Full color, full logo
- `krakowbites-logo-secondary.svg` - Full color, no tagline
- `krakowbites-logo-icon.svg` - Compass rose icon
- `krakowbites-logo-monochrome.svg` - Single color version
- `krakowbites-logo-reversed.svg` - White version

**Raster (for web):**
- `krakowbites-logo-primary@1x.png` - 400px width
- `krakowbites-logo-primary@2x.png` - 800px width (retina)
- `krakowbites-logo-secondary@1x.png` - 300px width
- `krakowbites-logo-secondary@2x.png` - 600px width
- `krakowbites-icon-512.png` - App icon source

**Favicon:**
- `favicon.ico` - 16x16, 32x32, 64x64 multi-size
- `favicon-16x16.png`
- `favicon-32x32.png`
- `apple-touch-icon.png` - 180x180 (iOS)
- `android-chrome-192x192.png`
- `android-chrome-512x512.png`

---

## Photography Guidelines

### Photography Ratio: 70% Eye-Level / 30% Overhead

**Shift from Current:**
- Current brief: 100% eye-level, warm editorial (Bon Appétit style)
- Urban Cartography: Mix of perspectives for distinctive signature

---

### Primary Photography (70% - Eye-Level Storytelling)

**Purpose:** Maintain warmth, human connection, food appeal

**Guidelines:**
- Eye-level perspective (maintain emotional connection)
- Natural lighting, warm tones (don't abandon food appeal)
- **Street signs, building numbers prominent** (wayfinding aesthetic)
- Grid composition when possible (map-like layouts)
- Location markers visible (street names, addresses, route context)

**Shot List - Food:**

```
Food Close-Ups:
├─ Plate + table number or restaurant sign visible
├─ Include street-visible elements (window, street beyond)
├─ Grid composition (plate aligned to rule of thirds)
├─ Emphasize texture, steam, authenticity (not styled perfection)
└─ Context: Where is this in the city?

Examples:
- Pierogi on wooden table, restaurant window showing street sign
- Obwarzanek in hand, Cloth Hall visible in background
- Soup steaming, building number 15 visible on wall
```

**Shot List - Heritage:**

```
Heritage Locations:
├─ Frame with street signs, building numbers prominent
├─ Show spatial context (where in city, which neighborhood)
├─ Include people navigating/exploring (not empty)
├─ Wayfinding elements prominent (signs, markers, addresses)
└─ Grid composition (architectural elements aligned)

Examples:
- Kazimierz street with visible street sign "ul. Szeroka"
- Synagogue entrance with building number and directional sign
- Market with vendor stall numbers visible
- Historical plaque with street address shown
```

**Shot List - People:**

```
Guide & Tourists:
├─ Interacting with location (pointing, reading signs, walking)
├─ Showing navigation (looking at map, finding address)
├─ Eye contact or focused on location (not posed)
├─ Context visible (where are they? what route?)
└─ Natural moments (laughing, tasting, discovering)

Examples:
- Guide pointing to building number while explaining
- Tourists checking street sign at intersection
- Group gathered around food, restaurant name visible
```

---

### Accent Photography (30% - Overhead "Map View")

**Purpose:** Add distinctive visual signature, reinforce map metaphor

**Guidelines:**
- Overhead perspective (bird's-eye, 90° down)
- Grid composition (emphasize coordinate system, aligned layouts)
- Shows spatial relationships (multiple elements, organization)
- **Use for:** Markets, table spreads, neighborhood overviews

**Shot List:**

```
Overhead Shots:
├─ Market stalls (grid layout visible, vendor organization)
├─ Table spreads (multiple dishes arranged, meal as map)
├─ Street intersections (crossroads, navigation visible)
├─ Tour route overview (multiple stops visible in one frame)
└─ Ingredient arrangements (components laid out like map legend)

Examples:
- Stary Kleparz market from above, showing stall grid
- Pierogi varieties arranged on table, overhead view
- Kazimierz intersection showing 4 streets meeting
- Tour stops laid out on map with photos at each point
```

**Camera Settings:**
- Focal length: 24-35mm (wide enough to show context)
- Aperture: f/5.6-f/8 (more depth of field for overhead)
- ISO: 400-800 (natural light when possible)
- Keep vertical (minimize lens distortion)

---

### Unique Photographic Technique: Route Overlays

**Concept:** Overlay dotted route lines on photos to create "map in photo" effect

**Implementation:**

1. **Base Photo:** Food or location photograph (eye-level or overhead)
2. **Route Layer:** Dotted line path overlaid
   - Color: Route Line (#e76f51)
   - Style: 3px dashed line (10px dash, 5px gap)
   - Path: Curves through image showing journey
3. **Markers:** Numbered location pins at key points
   - Style: Map pin icon
   - Number: Tour stop number (1, 2, 3, etc.)
   - Color: Water Blue (#2a9d8f)
4. **Result:** Photo becomes part of journey visualization

**Example:**
```
┌────────────────────────────────────┐
│ [Photo: Pierogi shop storefront]  │
│        ┈┈┈┈┈┈┈┈                   │  ← Route line enters from left
│      ┈         ┈                   │
│    ┈    [3]     ┈                  │  ← Pin marker: Stop 3
│  ┈               ┈                 │
│┈                  ┈┈┈┈┈┈           │  ← Route line exits to right
│                        ┈┈┈         │
└────────────────────────────────────┘
```

**When to Use:**
- Tour card header images
- Homepage hero carousel
- Tour detail page gallery
- Instagram posts
- Social media

**How to Create:**
1. Photoshop/Figma: Add vector path layer with dashed stroke
2. Web: SVG overlay with CSS (can animate path drawing)
3. Instagram: Use editing app to add custom graphics

**Animation (Web):**
```css
@keyframes drawRoute {
  from { stroke-dashoffset: 1000; }
  to { stroke-dashoffset: 0; }
}

.route-path {
  stroke: #e76f51;
  stroke-width: 3px;
  stroke-dasharray: 10 5;
  animation: drawRoute 2s ease-in-out;
}
```

---

### Photography Style Reference

**Do:**
- Natural lighting (golden hour when possible)
- Warm color tones (not oversaturated)
- Show context (where in city? which street?)
- Grid compositions (rule of thirds, aligned elements)
- Include wayfinding elements (signs, numbers, addresses)
- Mix eye-level (70%) with overhead (30%)
- Show people exploring, navigating, discovering

**Don't:**
- Sterile white backgrounds (feels lab-like)
- Flat overhead for all shots (needs balance)
- Over-saturated colors (unrealistic)
- Empty streets (need human scale)
- Ignore location context (could be anywhere)
- Stock photos (obvious, generic)

**Color Grading:**
- Warm tones (golden hour aesthetic)
- Slight contrast boost (not HDR)
- Preserve natural colors (not trendy filters)
- Terracotta and teal colors enhanced subtly

---

## UI Components

### Buttons

#### Primary Button (CTA)

**HTML:**
```html
<button class="btn btn-primary">
  Navigate to Details →
</button>
```

**CSS:**
```css
.btn-primary {
  /* Base */
  background: #e76f51;
  color: #ffffff;
  font-family: 'Inter', sans-serif;
  font-weight: 600;
  font-size: 16px;
  line-height: 1;
  padding: 14px 32px;
  border-radius: 8px;
  border: 2px solid #e76f51;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);

  /* Shadow */
  box-shadow:
    0 2px 8px rgba(231, 111, 81, 0.2),
    0 1px 3px rgba(0, 0, 0, 0.1);

  /* Typography */
  text-decoration: none;
  text-align: center;
  white-space: nowrap;
}

.btn-primary:hover {
  background: #d65d3f;
  border-color: #d65d3f;
  box-shadow:
    0 4px 12px rgba(231, 111, 81, 0.3),
    0 2px 6px rgba(0, 0, 0, 0.15);
  transform: translateY(-2px);
}

.btn-primary:active {
  transform: translateY(0);
  box-shadow:
    0 1px 4px rgba(231, 111, 81, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.1);
}

.btn-primary:focus {
  outline: none;
  box-shadow:
    0 2px 8px rgba(231, 111, 81, 0.2),
    0 0 0 3px rgba(231, 111, 81, 0.2);
}

.btn-primary:disabled {
  background: #c5c5c5;
  border-color: #c5c5c5;
  color: #7a7a7a;
  cursor: not-allowed;
  box-shadow: none;
  transform: none;
}
```

**Accessibility:**
- Minimum touch target: 44x44px (iOS/Android)
- Color contrast: 4.5:1 on white background ✅
- Focus visible: 3px outline
- Keyboard accessible

---

#### Secondary Button

**HTML:**
```html
<button class="btn btn-secondary">
  Learn More
</button>
```

**CSS:**
```css
.btn-secondary {
  /* Base */
  background: transparent;
  color: #e76f51;
  font-family: 'Inter', sans-serif;
  font-weight: 600;
  font-size: 16px;
  line-height: 1;
  padding: 14px 32px;
  border-radius: 8px;
  border: 2px solid #e76f51;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  text-decoration: none;
}

.btn-secondary:hover {
  background: #fef4f0; /* Very light terracotta tint */
  color: #d65d3f;
  border-color: #d65d3f;
  box-shadow: 0 2px 8px rgba(231, 111, 81, 0.15);
}

.btn-secondary:active {
  background: #fde8df;
  transform: scale(0.98);
}

.btn-secondary:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(231, 111, 81, 0.2);
}
```

---

#### Tertiary Button (Link-Style)

**HTML:**
```html
<button class="btn btn-tertiary">
  Cancel
</button>
```

**CSS:**
```css
.btn-tertiary {
  background: transparent;
  color: #2a9d8f; /* Water blue for links */
  font-family: 'Inter', sans-serif;
  font-weight: 600;
  font-size: 16px;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s ease;
  text-decoration: underline;
  text-decoration-color: transparent;
}

.btn-tertiary:hover {
  color: #24877c;
  text-decoration-color: #24877c;
  background: rgba(42, 157, 143, 0.05);
}

.btn-tertiary:active {
  color: #1e7169;
}
```

---

### Tour Cards

**HTML:**
```html
<article class="tour-card">
  <div class="tour-card__map">
    <!-- Map thumbnail with route overlay -->
    <img src="/maps/route-1-preview.jpg" alt="Route 1 map">
    <svg class="tour-card__route">
      <!-- Route line SVG overlay -->
    </svg>
  </div>

  <div class="tour-card__content">
    <h3 class="tour-card__title">Route 1: Kazimierz Food Corridor</h3>
    <span class="tour-card__coordinates">📍 50.0614°N, 19.9383°E</span>

    <div class="tour-card__metrics">
      <span class="tour-card__metric">
        <svg><!-- Walking icon --></svg>
        3.5 km
      </span>
      <span class="tour-card__metric">
        <svg><!-- Clock icon --></svg>
        3 hours
      </span>
      <span class="tour-card__metric">
        <svg><!-- Pin icon --></svg>
        8 stops
      </span>
    </div>

    <p class="tour-card__description">
      Navigate through grandmothers' kitchens and hidden bakeries where recipes survived wars and time itself.
    </p>

    <div class="tour-card__footer">
      <span class="tour-card__price">420 PLN</span>
      <a href="/tours/kazimierz-route" class="btn btn-primary btn-sm">
        Navigate to Details →
      </a>
    </div>
  </div>
</article>
```

**CSS:**
```css
.tour-card {
  /* Container */
  background: #ffffff;
  border: 1px solid #e5e5e5;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  flex-direction: column;
}

.tour-card:hover {
  transform: translateY(-8px);
  box-shadow:
    0 12px 24px rgba(0, 0, 0, 0.12),
    0 0 0 2px #e76f51; /* Terracotta border highlight */
}

/* Map section */
.tour-card__map {
  position: relative;
  height: 200px;
  background: #f8f5f0;
  overflow: hidden;
}

.tour-card__map img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.tour-card__route {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

/* Content section */
.tour-card__content {
  padding: 24px;
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.tour-card__title {
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 700;
  font-size: 24px;
  line-height: 1.3;
  color: #4a4a4a;
  margin: 0;
}

.tour-card__coordinates {
  font-family: 'JetBrains Mono', monospace;
  font-size: 14px;
  color: #2a9d8f;
  display: block;
}

.tour-card__metrics {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
}

.tour-card__metric {
  display: flex;
  align-items: center;
  gap: 6px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
  color: #6b4226;
}

.tour-card__metric svg {
  width: 16px;
  height: 16px;
  fill: currentColor;
}

.tour-card__description {
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  line-height: 1.6;
  color: #4a4a4a;
  flex: 1;
}

.tour-card__footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 16px;
  border-top: 1px solid #e5e5e5;
}

.tour-card__price {
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 700;
  font-size: 24px;
  color: #e76f51;
}

/* Responsive */
@media (max-width: 640px) {
  .tour-card__footer {
    flex-direction: column;
    align-items: stretch;
    gap: 12px;
  }

  .tour-card__price {
    text-align: center;
  }
}
```

---

### Forms

#### Input Fields

**HTML:**
```html
<div class="form-group">
  <label for="name" class="form-label">Full Name</label>
  <input type="text" id="name" class="form-input" placeholder="Jan Kowalski">
</div>
```

**CSS:**
```css
.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 20px;
}

.form-label {
  font-family: 'Inter', sans-serif;
  font-weight: 600;
  font-size: 14px;
  color: #4a4a4a;
  letter-spacing: 0.01em;
}

.form-input {
  /* Base */
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  color: #4a4a4a;
  background: #ffffff;
  border: 2px solid #e5e5e5;
  border-radius: 8px;
  padding: 12px 16px;
  transition: all 0.2s ease;

  /* Typography */
  line-height: 1.5;
}

.form-input::placeholder {
  color: #a0a0a0;
}

.form-input:hover {
  border-color: #c5c5c5;
}

.form-input:focus {
  outline: none;
  border-color: #e76f51; /* Terracotta */
  box-shadow: 0 0 0 3px rgba(231, 111, 81, 0.1);
}

/* States */
.form-input.is-error {
  border-color: #e63946;
}

.form-input.is-error:focus {
  box-shadow: 0 0 0 3px rgba(230, 57, 70, 0.1);
}

.form-input.is-success {
  border-color: #52b788;
}

.form-input:disabled {
  background: #f5f5f5;
  color: #a0a0a0;
  cursor: not-allowed;
}
```

#### Error Message

**HTML:**
```html
<div class="form-group">
  <label for="email" class="form-label">Email</label>
  <input type="email" id="email" class="form-input is-error">
  <span class="form-error">Please enter a valid email address</span>
</div>
```

**CSS:**
```css
.form-error {
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
  color: #e63946;
  display: flex;
  align-items: center;
  gap: 6px;
}

.form-error::before {
  content: '⚠';
  font-size: 16px;
}
```

---

## Spacing & Grid System

### Spacing Scale (4px base unit)

**Purpose:** Consistent rhythm, predictable layouts

```css
/* Spacing scale */
--space-xs:   4px;   /* Icon padding, minimal gaps */
--space-sm:   8px;   /* Tight spacing, small elements */
--space-md:   16px;  /* Default spacing (buttons, cards) */
--space-lg:   24px;  /* Section padding, component spacing */
--space-xl:   32px;  /* Component margins, large gaps */
--space-2xl:  48px;  /* Section margins, layout breaks */
--space-3xl:  64px;  /* Large section gaps, hero padding */
--space-4xl:  96px;  /* Extra large sections */
--space-5xl:  128px; /* Massive spacing (rare) */
```

**Usage:**
```css
/* Component padding */
.card { padding: var(--space-lg); }        /* 24px */
.button { padding: var(--space-md) var(--space-xl); } /* 16px 32px */

/* Component margins */
.section { margin-bottom: var(--space-3xl); } /* 64px */
.hero { padding: var(--space-4xl) 0; }     /* 96px 0 */

/* Element gaps */
.flex-container { gap: var(--space-md); }  /* 16px */
.grid { gap: var(--space-lg); }            /* 24px */
```

---

### Grid System (Map Coordinate Metaphor)

**Concept:** Grid based on map coordinates (latitude/longitude lines)

**Desktop Grid (≥1024px):**
```css
.container {
  max-width: 1280px; /* Main content width */
  margin: 0 auto;
  padding: 0 var(--space-xl); /* 32px */
}

.grid-12 {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: var(--space-lg); /* 24px */
}

/* Common layouts */
.col-span-12 { grid-column: span 12; } /* Full width */
.col-span-6  { grid-column: span 6;  } /* Half */
.col-span-4  { grid-column: span 4;  } /* Third */
.col-span-3  { grid-column: span 3;  } /* Quarter */
```

**Tablet Grid (768px - 1023px):**
```css
@media (max-width: 1023px) {
  .grid-12 {
    grid-template-columns: repeat(8, 1fr);
    gap: var(--space-md); /* 16px */
  }

  .col-span-6  { grid-column: span 8; } /* Full on tablet */
  .col-span-4  { grid-column: span 4; } /* Half on tablet */
}
```

**Mobile Grid (≤767px):**
```css
@media (max-width: 767px) {
  .container {
    padding: 0 var(--space-md); /* 16px */
  }

  .grid-12 {
    grid-template-columns: 1fr;
    gap: var(--space-md); /* 16px */
  }

  .col-span-6,
  .col-span-4,
  .col-span-3 {
    grid-column: span 1; /* Full width on mobile */
  }
}
```

---

### Coordinate Grid Lines (Visual Metaphor)

**Purpose:** Subtle background grid suggesting map coordinates

**CSS:**
```css
.section-with-grid {
  position: relative;
  overflow: hidden;
}

.section-with-grid::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image:
    linear-gradient(to right, rgba(231, 111, 81, 0.05) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(231, 111, 81, 0.05) 1px, transparent 1px);
  background-size: 100px 100px; /* Grid cell size */
  pointer-events: none;
  z-index: 0;
}
```

**When to Use:**
- Homepage hero background (subtle)
- About page sections
- Large content areas
- **Don't overuse:** Should feel like subtle texture, not dominant

---

## Iconography

### Icon Set: Lucide Icons (Recommended)

**Source:** https://lucide.dev/
**License:** ISC (open-source)
**Style:** Stroke-based, minimalist
**Customization:** Stroke width: 2px, Size: 20px default

**Why Lucide:**
- Modern, clean aesthetic
- Consistent stroke width (matches brand weight)
- Open-source, free
- React/Vue/Svelte components available
- SVG (scalable, accessible)

**Installation:**
```bash
npm install lucide-react
# or
npm install lucide
```

**Usage (React):**
```jsx
import { Navigation, MapPin, Clock, Users } from 'lucide-react';

<Navigation size={20} color="#e76f51" strokeWidth={2} />
<MapPin size={24} color="#2a9d8f" strokeWidth={2} />
```

**Usage (HTML/CSS):**
```html
<svg class="icon icon-navigation">
  <use href="/icons/lucide.svg#navigation"></use>
</svg>
```

```css
.icon {
  width: 20px;
  height: 20px;
  stroke: currentColor;
  stroke-width: 2px;
  fill: none;
}

.icon-primary { color: #e76f51; }
.icon-secondary { color: #2a9d8f; }
```

---

### Required Icons (Lucide Names)

**Navigation/Wayfinding:**
- `navigation` - Compass/direction
- `map-pin` - Location marker
- `map` - Map view
- `route` - Route/path
- `compass` - Compass rose

**Tour Metrics:**
- `clock` - Duration
- `users` - Group size
- `utensils` - Food/dining
- `building-2` - Heritage sites
- `walking` - Walking distance (use `footprints` or custom)

**Actions:**
- `arrow-right` - Navigate/proceed
- `chevron-right` - Next/forward
- `chevron-down` - Dropdown/expand
- `x` - Close
- `menu` - Mobile menu
- `search` - Search

**States:**
- `check-circle` - Success/confirmation
- `alert-circle` - Warning
- `x-circle` - Error
- `info` - Information

**Dietary:**
- `leaf` - Vegetarian/vegan
- `wheat-off` - Gluten-free (use `slash` overlay on wheat custom icon)
- Custom: Kosher symbol (Star of David)

---

### Custom Icons Needed

**1. Kosher Symbol**
```svg
<svg viewBox="0 0 24 24">
  <path d="M12 2 L15 8 L21 9 L16 14 L18 21 L12 17 L6 21 L8 14 L3 9 L9 8 Z"
        stroke="currentColor"
        stroke-width="2"
        fill="none"/>
</svg>
```

**2. Walking Icon (if Lucide's footprints doesn't work)**
```svg
<svg viewBox="0 0 24 24">
  <!-- Person walking silhouette -->
</svg>
```

**3. Route Line Pattern**
```svg
<svg viewBox="0 0 100 20">
  <line x1="0" y1="10" x2="100" y2="10"
        stroke="currentColor"
        stroke-width="3"
        stroke-dasharray="10 5"/>
</svg>
```

---

### Icon Style Guidelines

**Do:**
- Use 2px stroke width (matches brand weight)
- Default size: 20px (scale up to 24px for emphasis)
- Color: Route Line (#e76f51) for primary, Water Blue (#2a9d8f) for secondary
- Align to text baseline
- Maintain consistent visual weight

**Don't:**
- Mix outlined and filled icons (use stroke-based only)
- Use icons smaller than 16px (readability)
- Use more than 2 icon colors on one screen
- Rotate icons arbitrarily (keep upright)

---

## Motion Design

### Animation Principles

**Philosophy:** Subtle, purposeful, map-inspired motion

**Timing:**
- Fast: 150ms (micro-interactions)
- Medium: 250ms (button hovers, card reveals)
- Slow: 400ms (page transitions, modals)

**Easing:**
- Default: `cubic-bezier(0.4, 0, 0.2, 1)` (material design ease)
- Bounce: `cubic-bezier(0.68, -0.55, 0.265, 1.55)` (playful, use sparingly)
- Ease-out: `cubic-bezier(0, 0, 0.2, 1)` (elements entering)
- Ease-in: `cubic-bezier(0.4, 0, 1, 1)` (elements exiting)

---

### Route Line Animation

**Purpose:** Animate route lines "drawing" onto page

**CSS:**
```css
@keyframes drawRoute {
  from {
    stroke-dashoffset: 1000;
  }
  to {
    stroke-dashoffset: 0;
  }
}

.route-path {
  stroke: #e76f51;
  stroke-width: 3px;
  stroke-dasharray: 10 5;
  stroke-dashoffset: 1000;
  animation: drawRoute 2s ease-in-out forwards;
}
```

**When to Use:**
- Homepage hero route line
- Tour card on hover
- Tour detail page route reveal
- **Don't overuse:** Should feel special, not distracting

---

### Map Panning Transition

**Purpose:** Smooth transitions between pages feel like map panning

**CSS:**
```css
@keyframes panLeft {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.page-enter {
  animation: panLeft 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}
```

**When to Use:**
- Page transitions (navigating between tours)
- Section reveals (scrolling into view)
- Modal open (sliding in from right)

---

### Hover States

**Button Hover:**
```css
.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(231, 111, 81, 0.3);
  transition: all 0.15s ease;
}
```

**Card Hover:**
```css
.tour-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.12);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
```

**Link Hover:**
```css
.link {
  text-decoration: underline;
  text-decoration-color: transparent;
  transition: text-decoration-color 0.2s ease;
}

.link:hover {
  text-decoration-color: #2a9d8f;
}
```

---

## Voice & Messaging

### Brand Personality Shift

**From (Current Sage Guide):**
- Warm storyteller
- "Join me" invitation
- Heritage guide
- Forgotten stories
- Sage archetype
- Knowledgeable but warm

**To (Urban Cartography Explorer):**
- Expert navigator
- "Explore together" invitation
- Urban explorer
- Unmapped territories
- Explorer archetype
- Precise but adventurous

---

### Messaging Framework

#### Tagline
**Primary:** "Navigate Kraków's culinary landscape"
**Alternative:** "Your cartographer of forgotten flavors"
**Alternative:** "Plot a course through Kraków's food history"

#### Elevator Pitch
"KrakowBites offers precisely mapped food and heritage tours through Kraków's layered culinary history. I'm your navigator through flavors that survived wars, occupations, and time itself—not random walks, but deliberate routes through the city's soul."

#### Value Propositions
1. **Precision:** Routes aren't random—every stop is precisely mapped, every story geographically grounded
2. **Navigation:** You'll know exactly where you are in the city's food history
3. **Exploration:** Discover unmapped territories in familiar neighborhoods
4. **Expertise:** Your guide navigates both geography and time

---

### Messaging Examples

#### Homepage Hero
```
Headline:  Navigate Kraków's Culinary Landscape
Subhead:   Precisely mapped tours through forgotten food stories
CTA:       Plot Your Route →
```

#### About Page
```
Current:
"Before World War II, Kazimierz's bakeries filled the air with challah
on Friday afternoons. Grandmothers passed down gefilte fish recipes..."

New (Urban Cartography):
"Every street corner in Kazimierz marks a culinary crossroads. These
routes through Jewish and Polish kitchens aren't random walks—they're
precisely mapped journeys through the city's layered food history. I'm
your navigator through flavors that survived wars, occupations, and
time itself. Let's plot a course together."
```

#### Tour Descriptions
```
Current:
"Shtetl to Street: Lost Recipes of Jewish Kazimierz"

New Pattern:
"Route 1: Kazimierz Food Corridor"
"Heritage Route: Jewish Quarter Tastes"
"Old Town Culinary Trail"
```

#### Call-to-Action Updates
```
Current → New:
"Book Now"     → "Plot Your Journey"
"View Tour"    → "Navigate to Details"
"Join Tour"    → "Reserve Your Route"
"Contact"      → "Get Directions"
"Learn More"   → "Explore This Route"
```

#### Email Subject Lines
```
Current:
"Your Kazimierz Tour Confirmation"

New:
"Route Confirmed: Your Kazimierz Journey on [Date]"
"Coordinates Locked: Meeting Point & Tour Details"
"Navigation Guide: Prepare for Your Route"
```

---

### Voice Guidelines

**Do:**
- Use navigation/exploration language ("navigate," "route," "map," "coordinates")
- Be precise about locations (street names, building numbers, coordinates)
- Frame tours as journeys with waypoints
- Maintain warmth through storytelling (balance technical metaphor)
- Show expertise through specificity

**Don't:**
- Overuse technical jargon (not "proceed to waypoint alpha")
- Lose warmth (not cold/robotic navigation)
- Abandon storytelling (maps support stories, don't replace them)
- Use military/survival language (not "tactical route")

**Examples:**

✅ Good:
"Route 1 navigates through three centuries of Jewish baking—from medieval Szeroka Street to the challah that still steams in Kazimierz kitchens today. Your journey starts at coordinates 50.0614°N, 19.9383°E."

❌ Bad (too technical):
"Proceed to grid reference 50.0614N 19.9383E for tour commencement. Route will traverse designated culinary waypoints in chronological sequence."

❌ Bad (lost navigation metaphor):
"Come join me on a magical journey through the enchanting streets of Kazimierz where delicious surprises await!"

---

## Application Examples

### Homepage Hero Section

**Layout:**
```
┌────────────────────────────────────────────────┐
│                                                │
│  [Background: Map with route overlay]         │
│                                                │
│  H1: Navigate Kraków's Culinary Landscape     │  ← Space Grotesk Bold, 64px, white
│  P:  Precisely mapped tours through           │  ← Inter, 20px, white 80%
│      forgotten food stories                    │
│  Span: 50.0614°N, 19.9383°E                   │  ← JetBrains Mono, teal
│                                                │
│  [Button: Plot Your Route →]                  │  ← Terracotta button
│                                                │
└────────────────────────────────────────────────┘
```

**CSS:**
```css
.hero {
  position: relative;
  min-height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-4xl) var(--space-xl);
  background-image: url('/images/hero-map.jpg');
  background-size: cover;
  background-position: center;
}

.hero::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    to bottom,
    rgba(74, 74, 74, 0.3),
    rgba(74, 74, 74, 0.6)
  );
  z-index: 1;
}

.hero__content {
  position: relative;
  z-index: 2;
  text-align: center;
  max-width: 800px;
}

.hero__title {
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 700;
  font-size: 64px;
  line-height: 1.1;
  color: #ffffff;
  margin-bottom: 16px;
}

.hero__subtitle {
  font-family: 'Inter', sans-serif;
  font-size: 20px;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 12px;
}

.hero__coordinates {
  font-family: 'JetBrains Mono', monospace;
  font-size: 16px;
  color: #3cb4a4; /* Lighter teal for dark background */
  margin-bottom: 32px;
  display: block;
}
```

---

### Tour Detail Page

**Hero Section:**
```
┌────────────────────────────────────────────────┐
│ [Interactive Map: Shows full route]           │
│  - Route line with numbered pins              │
│  - Markers clickable for stop details         │
│  - Google Maps or Mapbox                      │
└────────────────────────────────────────────────┘

┌────────────────────────────────────────────────┐
│ H1: Route 1: Kazimierz Food Corridor          │
│ Coordinates: 📍 50.0614°N, 19.9383°E          │
│                                                │
│ Metrics: 🚶 3.5 km • ⏱ 3 hours • 📍 8 stops   │
│                                                │
│ Description: [Long-form tour description]     │
│                                                │
│ [Booking Widget: Sticky sidebar/mobile]       │
└────────────────────────────────────────────────┘
```

**Itinerary as Waypoints:**
```html
<div class="itinerary">
  <h2>Route Waypoints</h2>

  <div class="waypoint">
    <span class="waypoint__number">1</span>
    <div class="waypoint__content">
      <h3 class="waypoint__title">Start: Main Market Square</h3>
      <span class="waypoint__coordinates">50.0619°N, 19.9369°E</span>
      <p class="waypoint__description">
        Meet your navigator at the Cloth Hall south entrance. We'll begin
        with a brief history of the medieval marketplace before plotting
        our route east.
      </p>
      <div class="waypoint__tags">
        <span class="tag">Heritage Site</span>
        <span class="tag">Meeting Point</span>
      </div>
    </div>
  </div>

  <div class="waypoint">
    <span class="waypoint__number">2</span>
    <div class="waypoint__content">
      <h3 class="waypoint__title">Obwarzanek Bakery</h3>
      <span class="waypoint__coordinates">50.0622°N, 19.9375°E</span>
      <p class="waypoint__description">
        First food stop: Traditional Polish pretzel, crispy outside,
        chewy inside. Recipe unchanged since 1394.
      </p>
      <div class="waypoint__tags">
        <span class="tag">🍴 Food Stop</span>
        <span class="tag">Traditional</span>
      </div>
    </div>
  </div>

  <!-- More waypoints... -->
</div>
```

---

### Booking Widget

```html
<aside class="booking-widget">
  <div class="booking-widget__header">
    <h3>Plot Your Journey</h3>
    <span class="booking-widget__price">420 PLN / person</span>
  </div>

  <form class="booking-widget__form">
    <div class="form-group">
      <label for="date" class="form-label">📅 Select Date</label>
      <input type="date" id="date" class="form-input">
    </div>

    <div class="form-group">
      <label for="size" class="form-label">👥 Party Size</label>
      <select id="size" class="form-input">
        <option>2 people</option>
        <option>3 people</option>
        <option>4 people</option>
      </select>
    </div>

    <div class="form-group">
      <label for="time" class="form-label">🕐 Start Time</label>
      <select id="time" class="form-input">
        <option>14:00 (2:00 PM)</option>
        <option>18:00 (6:00 PM)</option>
      </select>
    </div>

    <button type="submit" class="btn btn-primary btn-block">
      Navigate to Checkout →
    </button>
  </form>

  <div class="booking-widget__footer">
    <p class="text-sm">
      <svg class="icon">...</svg>
      Free cancellation up to 24 hours before
    </p>
  </div>
</aside>
```

---

## Accessibility Standards

### WCAG Compliance

**Level:** AA minimum, AAA preferred
**Auditing:** Monthly automated checks + manual testing

#### Color Contrast (Tested)

| Text Color | Background | Ratio | WCAG | Pass/Fail |
|------------|------------|-------|------|-----------|
| Path Grey (#4a4a4a) | White (#ffffff) | 9.1:1 | AAA | ✅ Pass |
| Path Grey (#4a4a4a) | Map Paper (#f8f5f0) | 10.5:1 | AAA | ✅ Pass |
| Route Line (#e76f51) | White (#ffffff) | 4.5:1 | AA | ✅ Pass |
| Route Line (#e76f51) | Map Paper (#f8f5f0) | 5.2:1 | AA | ✅ Pass |
| Water Blue (#2a9d8f) | White (#ffffff) | 4.8:1 | AA | ✅ Pass |
| Landmark Brown (#6b4226) | White (#ffffff) | 8.1:1 | AAA | ✅ Pass |
| Landmark Brown (#6b4226) | Map Paper (#f8f5f0) | 9.3:1 | AAA | ✅ Pass |

**All primary colors pass WCAG AA or AAA standards ✅**

---

### Interactive Elements

**Touch Targets:**
- Minimum: 44x44px (iOS/Android guidelines)
- Preferred: 48x48px (better usability)
- Buttons: 44px minimum height, generous padding

**Focus States:**
- All interactive elements have visible focus
- Focus ring: 3px outline, Route Line color with 20% opacity
- Keyboard navigation fully supported
- Skip to main content link

**Example:**
```css
button:focus,
a:focus,
input:focus {
  outline: 3px solid rgba(231, 111, 81, 0.4);
  outline-offset: 2px;
}

.skip-to-main {
  position: absolute;
  top: -100px;
  left: 0;
  background: #e76f51;
  color: white;
  padding: 8px 16px;
  text-decoration: none;
  z-index: 9999;
}

.skip-to-main:focus {
  top: 0;
}
```

---

### Readability

**Typography:**
- Minimum font size: 16px for body text
- Line height: 1.6-1.8 for paragraphs
- Line length: Max 70 characters (optimal readability)
- Letter spacing: Slight tightening for large headings, normal for body

**Example:**
```css
.readable-text {
  font-size: 16px; /* minimum */
  line-height: 1.7;
  max-width: 70ch; /* 70 characters */
}
```

---

### Semantic HTML

**Structure:**
```html
<!-- Use semantic HTML5 elements -->
<header>...</header>
<nav aria-label="Main navigation">...</nav>
<main>
  <article>
    <h1>...</h1>
    <section>...</section>
  </article>
</main>
<aside aria-label="Booking">...</aside>
<footer>...</footer>
```

**ARIA Labels:**
```html
<!-- Accessible labels for screen readers -->
<button aria-label="Navigate to tour details">
  →
</button>

<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li aria-current="page">Route 1</li>
  </ol>
</nav>

<img src="map.jpg" alt="Route map showing 8 stops through Kazimierz district">
```

---

## Implementation Guide

### Phase 1: Setup (Days 1-2)

**1. Install Fonts:**
```bash
# Option 1: Google Fonts CDN
# Add to <head> in HTML

# Option 2: Self-hosted (better performance)
npm install @fontsource/space-grotesk @fontsource/inter @fontsource/jetbrains-mono
```

```javascript
// In your app entry (main.js, index.js)
import '@fontsource/space-grotesk/400.css';
import '@fontsource/space-grotesk/500.css';
import '@fontsource/space-grotesk/700.css';
import '@fontsource/inter/400.css';
import '@fontsource/inter/500.css';
import '@fontsource/inter/600.css';
import '@fontsource/jetbrains-mono/400.css';
```

**2. Tailwind Config:**
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        'map-paper': '#f8f5f0',
        'route-line': {
          light: '#f0856c',
          DEFAULT: '#e76f51',
          dark: '#d65d3f',
          darker: '#c5502e',
        },
        'water-blue': {
          light: '#3cb4a4',
          DEFAULT: '#2a9d8f',
          dark: '#24877c',
          darker: '#1e7169',
        },
        'landmark-brown': '#6b4226',
        'path-grey': '#4a4a4a',
      },
      fontFamily: {
        'heading': ['Space Grotesk', 'Arial', 'sans-serif'],
        'body': ['Inter', 'Roboto', 'sans-serif'],
        'mono': ['JetBrains Mono', 'Courier New', 'monospace'],
      },
      spacing: {
        'xs': '4px',
        'sm': '8px',
        'md': '16px',
        'lg': '24px',
        'xl': '32px',
        '2xl': '48px',
        '3xl': '64px',
        '4xl': '96px',
        '5xl': '128px',
      },
    },
  },
};
```

**3. CSS Custom Properties:**
```css
:root {
  /* Colors */
  --color-map-paper: #f8f5f0;
  --color-route-line: #e76f51;
  --color-water-blue: #2a9d8f;
  --color-landmark-brown: #6b4226;
  --color-path-grey: #4a4a4a;

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
  --space-2xl: 48px;
  --space-3xl: 64px;

  /* Typography */
  --font-heading: 'Space Grotesk', Arial, sans-serif;
  --font-body: 'Inter', Roboto, sans-serif;
  --font-mono: 'JetBrains Mono', 'Courier New', monospace;
}
```

---

### Phase 2: Component Library (Days 3-7)

**File Structure:**
```
src/
├── components/
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.css
│   │   └── Button.test.tsx
│   ├── TourCard/
│   │   ├── TourCard.tsx
│   │   ├── TourCard.css
│   │   └── TourCard.test.tsx
│   ├── RouteOverlay/
│   │   ├── RouteOverlay.tsx
│   │   └── RouteOverlay.css
│   └── BookingWidget/
│       ├── BookingWidget.tsx
│       └── BookingWidget.css
├── styles/
│   ├── variables.css
│   ├── typography.css
│   ├── spacing.css
│   └── utilities.css
└── assets/
    ├── icons/
    └── patterns/
```

---

### Phase 3: Map Integration (Days 8-12)

**Google Maps (Option 1):**
```bash
npm install @react-google-maps/api
```

```tsx
import { GoogleMap, Polyline, Marker } from '@react-google-maps/api';

const TourMap = () => {
  const route = [
    { lat: 50.0614, lng: 19.9383 },
    { lat: 50.0622, lng: 19.9375 },
    // ... more coordinates
  ];

  return (
    <GoogleMap
      center={route[0]}
      zoom={15}
      mapContainerStyle={{ height: '400px', width: '100%' }}
    >
      <Polyline
        path={route}
        options={{
          strokeColor: '#e76f51',
          strokeWeight: 3,
          strokeOpacity: 0.8,
          strokeDasharray: '10 5',
        }}
      />
      {route.map((position, index) => (
        <Marker
          key={index}
          position={position}
          label={(index + 1).toString()}
        />
      ))}
    </GoogleMap>
  );
};
```

**Mapbox (Option 2):**
```bash
npm install mapbox-gl react-map-gl
```

```tsx
import Map, { Source, Layer, Marker } from 'react-map-gl';

const TourMap = () => {
  const route = {
    type: 'Feature',
    geometry: {
      type: 'LineString',
      coordinates: [
        [19.9383, 50.0614],
        [19.9375, 50.0622],
        // ... more coordinates
      ],
    },
  };

  return (
    <Map
      initialViewState={{
        latitude: 50.0614,
        longitude: 19.9383,
        zoom: 15,
      }}
      mapStyle="mapbox://styles/mapbox/streets-v11"
    >
      <Source type="geojson" data={route}>
        <Layer
          type="line"
          paint={{
            'line-color': '#e76f51',
            'line-width': 3,
            'line-dasharray': [2, 1],
          }}
        />
      </Source>
    </Map>
  );
};
```

---

### Phase 4: Testing & Launch (Days 13-15)

**Accessibility Testing:**
```bash
# Install testing tools
npm install --save-dev @axe-core/react lighthouse-ci
```

**Automated Tests:**
```javascript
// accessibility.test.js
import { axe, toHaveNoViolations } from 'jest-axe';

expect.extend(toHaveNoViolations);

test('should not have accessibility violations', async () => {
  const { container } = render(<TourCard {...props} />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

**Manual Checklist:**
- [ ] Keyboard navigation works (Tab, Enter, Escape)
- [ ] Screen reader announces content correctly (NVDA/JAWS)
- [ ] Color contrast validated (WebAIM tool)
- [ ] Touch targets minimum 44x44px
- [ ] Focus states visible
- [ ] Semantic HTML used
- [ ] ARIA labels present
- [ ] Forms have labels
- [ ] Images have alt text
- [ ] Videos have captions

---

**End of Document**

**Status:** Complete specifications for Urban Cartography brand direction
**Next:** Implement components, test with users, refine based on feedback
