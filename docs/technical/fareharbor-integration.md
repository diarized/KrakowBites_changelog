# FareHarbor Integration Guide

**Document Version:** 1.0.0
**Last Updated:** 2026-01-02
**Status:** Active
**Applies To:** KrakowBites website (custom HTML)
**FareHarbor API Version:** v1

---

## Purpose

**What:** Integration methods for embedding FareHarbor booking functionality into KrakowBites website
**Why:** Enable inline booking for multiple city tour activities without leaving the site
**Scope:** Custom HTML implementation, multiple tour types (walking tours, bus tours, food tours, etc.)

---

## Requirements Analysis

**Use Case:** Multiple city tour activities (KrakowBites)

**Requirements:**
- Display type: Inline (not modal/lightbox)
- Platform: Custom HTML (not WordPress/Squarespace)
- Customization: Standard widgets (no custom code)
- Data integration: None (no CRM/analytics API integration)
- Tour count: Multiple activities (many tour types)

**Recommended Solution:** Item Grid embed widget

**Rationale:** FareHarbor documentation states: "When presenting several different options to a customer, item grids are usually easier for customers to navigate than a calendar with all of your activities."

---

## Integration Methods Overview

**Available Methods:**

| Method | Display Type | Best For | Complexity | Inline Support |
|--------|-------------|----------|------------|----------------|
| Item Grid | Visual cards/tiles | Multiple tour types | Low | Yes ✅ |
| Embedded Calendar | Availability calendar | Date-focused booking | Low | Yes ✅ |
| Lightframe Modal | Popup overlay | Keep users on site | Low | No |
| JavaScript API | Custom implementation | Advanced customization | High | Yes |
| REST API | Backend integration | Multi-system integration | Very High | N/A |

---

## Recommended Solution: Item Grid Embed

**Purpose:** Display multiple tour activities as visual grid of cards/tiles

### Features

**Display Characteristics:**
- Clean, professional layout
- Multiple tour options shown simultaneously
- Responsive design (desktop, tablet, mobile)
- Auto-adjusts to container width

**User Flow:**
1. Customer browses tour grid
2. Selects preferred tour
3. Views availability
4. Completes booking inline

**Configuration Options:**
- Display all items (all tours)
- Display specific flow (curated tour group)
- Multiple grids per page (different categories)

### Implementation Steps

**Prerequisites:**
- Active FareHarbor account
- Tour inventory configured in FareHarbor Dashboard
- HTML page where widget will be embedded

**Step 1: Access Embed Generator**

**Path Option A:**
```
FareHarbor Dashboard → Settings → Book Buttons & Embeds
```

**Path Option B:**
```
FareHarbor Dashboard → Items → [Select any item] → Book Buttons & Embeds (sidebar)
```

**Step 2: Configure Widget**

**Actions:**
1. Select widget type: **Item Grid**
2. Choose item selection:
   - Option A: All items (displays all tours)
   - Option B: Specific flow (curated tour group)
3. Configure flow settings (if using specific flow)

**Flow Definition:** A curated group of tours/activities for organized display

**Step 3: Generate Code**

**Process:**
1. Code auto-generates in text box as settings change
2. Click anywhere on code to highlight
3. Copy entire code block

**Code Type:** HTML + JavaScript (FareHarbor account-specific)

**Step 4: Embed in HTML**

**Implementation:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>City Tours - KrakowBites</title>
</head>
<body>
    <!-- Existing page header -->
    <header>
        <h1>Krakow City Tours</h1>
    </header>

    <!-- Main content area -->
    <main>
        <!-- FareHarbor Item Grid Widget -->
        <!-- Paste generated code from FareHarbor Dashboard here -->
        <div id="fareharbor-embed">
            <!-- Auto-generated FareHarbor code goes here -->
        </div>
        <!-- End FareHarbor Widget -->
    </main>

    <!-- Existing page footer -->
    <footer>
        <!-- Footer content -->
    </footer>
</body>
</html>
```

**Important:** Replace placeholder comment with actual generated code from FareHarbor Dashboard

**Step 5: Verify**

**Testing Checklist:**
- [ ] Widget displays correctly on desktop
- [ ] Widget displays correctly on tablet
- [ ] Widget displays correctly on mobile
- [ ] All tour types appear in grid
- [ ] Clicking tour opens booking flow
- [ ] Booking completes successfully
- [ ] Responsive behavior works correctly

---

## Alternative Solution: Embedded Calendar

**Purpose:** Display availability-focused calendar for tour bookings

### When to Use

**Use Embedded Calendar when:**
- Availability dates are primary concern
- Showing real-time booking slots is critical
- Calendar-first booking experience preferred
- Limited tour options (not many types)

**Use Item Grid when:**
- Multiple tour types need browsing ✅ (KrakowBites use case)
- Tour selection is primary concern
- Visual tour presentation preferred

### Implementation Steps

**Process:** Identical to Item Grid (see above), except:

**Step 2 Difference:**
- Select widget type: **Embedded Calendar** (instead of Item Grid)
- Choose items to include:
  - Default: All tours shown in single calendar
  - Custom: Select flow with specific tour group

**Display Behavior:**
- Shows calendar with available dates/times
- One or more tours displayed in single calendar view
- Customers see availability without clicking

---

## Flow Configuration

**Definition:** Flow = curated group of tours/activities

### Purpose

**Why Use Flows:**
- Organize tours by category (walking, bus, food, etc.)
- Display specific tour groups on different pages
- Create multiple specialized grids
- Improve customer navigation

### Configuration

**Access:**
```
FareHarbor Dashboard → Flows → Create New Flow / Edit Existing Flow
```

**Process:**
1. Create flow (e.g., "Walking Tours", "Food Tours", "Bus Tours")
2. Add specific tours to flow
3. Reference flow in embed generator
4. Generate code for that flow
5. Embed on appropriate page

### Example Structure

**Tour Organization:**

```
Flow: Walking Tours
├── Old Town Walking Tour
├── Jewish Quarter Walking Tour
└── Wawel Castle Walking Tour

Flow: Food Tours
├── Traditional Polish Food Tour
├── Street Food Tour
└── Market Tour

Flow: Bus Tours
├── City Highlights Bus Tour
└── Auschwitz-Birkenau Tour
```

**Implementation:**

```html
<!-- Page: walking-tours.html -->
<div id="walking-tours-grid">
    <!-- FareHarbor code for "Walking Tours" flow -->
</div>

<!-- Page: food-tours.html -->
<div id="food-tours-grid">
    <!-- FareHarbor code for "Food Tours" flow -->
</div>

<!-- Page: all-tours.html -->
<div id="all-tours-grid">
    <!-- FareHarbor code with "All items" selected -->
</div>
```

---

## Advanced: Lightframe Modal (Optional)

**Purpose:** Popup booking overlay that keeps customers on your website

**Use Case:** If inline display is insufficient, Lightframe provides modal experience

### Implementation Steps

**Step 1: Add Lightframe API Script**

**Placement:** Before closing `</body>` tag

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Head content -->
</head>
<body>
    <!-- Page content -->

    <!-- FareHarbor Lightframe API (place before </body>) -->
    <script type="text/javascript" src="https://fareharbor.com/embeds/api/v1/?autolightframe=yes"></script>
</body>
</html>
```

**Step 2: Generate Book Button**

**Process:**
1. Dashboard → Settings → Book Buttons & Embeds
2. Select button/link widget type
3. Configure button settings
4. Copy generated code
5. Paste into HTML

**Step 3: Behavior**

**When Button Clicked:**
1. Lightframe modal opens on top of page
2. Customer completes booking in modal
3. Customer never leaves your website
4. Modal closes after booking

### Lightframe Customization Options

**Opening Targets:**
- Availability calendar view
- Browse by activity page
- Specific item/tour page
- Individual availability slot

---

## Advanced: JavaScript API

**Purpose:** Custom control over Lightframe behavior

**Use Case:** Advanced customization beyond standard widgets

### Available Methods

**API Methods:**

| Method | Purpose | Parameters | Return Value |
|--------|---------|------------|--------------|
| `FH.open()` | Opens Lightframe programmatically | Configuration object | void |
| `FH.close()` | Closes Lightframe programmatically | None | void |

### Example Usage

```javascript
// Custom button click handler
document.getElementById('custom-book-btn').addEventListener('click', function() {
    // Open Lightframe programmatically
    FH.open({
        shortname: 'your-company-name',
        flow: 'walking-tours',
        view: 'calendar'
    });
});

// Close Lightframe on custom event
document.getElementById('cancel-btn').addEventListener('click', function() {
    FH.close();
});
```

**Note:** Requires Lightframe API script loaded (see Advanced: Lightframe Modal section)

---

## Advanced: REST API Integration

**Purpose:** Backend integration for custom booking platforms or multi-system integration

**Use Case:** Not applicable for KrakowBites (no backend integration required)

**When Needed:**
- Custom booking platforms
- Multi-vendor marketplaces
- Mobile app integration
- CRM/analytics data synchronization

### Resources

**Official Documentation:**
- GitHub: `https://github.com/FareHarbor/fareharbor-docs`
- Developer Portal: `https://developer.fareharbor.com/api/external/v1/`

**Capabilities:**
- Read booking data
- Write booking data
- OTA (Online Travel Agency) connectivity
- Real-time availability queries

---

## Troubleshooting

### Widget Not Displaying

**Possible Causes:**

| Issue | Symptom | Solution |
|-------|---------|----------|
| Incorrect code | Nothing appears | Regenerate code from Dashboard |
| Container size | Widget collapsed | Set min-width/min-height on container |
| JavaScript blocked | Widget fails to load | Check browser console for errors |
| Account issue | Error message displayed | Verify FareHarbor account status |

**Debugging Steps:**
1. Open browser Developer Tools (F12)
2. Check Console tab for errors
3. Check Network tab for failed requests
4. Verify generated code matches FareHarbor output exactly
5. Test in different browser

### Responsive Issues

**Problem:** Widget doesn't adapt to mobile screens

**Solution:**
- Verify container has no fixed width
- Check parent elements for width constraints
- Test with FareHarbor support for widget-specific issues

**Container Best Practices:**

```html
<!-- Good: Flexible container -->
<div style="max-width: 100%; margin: 0 auto;">
    <!-- FareHarbor widget code -->
</div>

<!-- Bad: Fixed width -->
<div style="width: 1200px;">
    <!-- FareHarbor widget code -->
</div>
```

### Tours Not Appearing

**Problem:** Some tours missing from grid/calendar

**Possible Causes:**
1. Flow configured with subset of tours (check flow settings)
2. Tours not active in FareHarbor Dashboard
3. Tours have no availability (calendar hides unavailable items)

**Solution:**
1. Dashboard → Flows → Verify flow configuration
2. Dashboard → Items → Verify tour status (active/inactive)
3. Dashboard → Calendar → Verify availability exists

---

## Best Practices

### Placement Recommendations

**Recommended Widget Placement:**

| Location | Widget Type | Purpose |
|----------|-------------|---------|
| Tours overview page | Item Grid (all tours) | Browse all offerings |
| Category pages | Item Grid (specific flow) | Browse category-specific tours |
| Header/Navigation | Book button (Lightframe) | Quick access CTA |
| Footer | "Book online now" link | Secondary CTA |
| Floating button | Book button (Lightframe) | Persistent CTA on all pages |

### Performance Optimization

**Minimize Widget Count:**
- Use one comprehensive grid instead of multiple small grids
- Lazy load widgets below the fold
- Use Lightframe for secondary CTAs (lighter weight than full embeds)

**Page Load:**
- Place widget code near where it displays (not in `<head>`)
- Load FareHarbor scripts asynchronously when possible

### User Experience

**Accessibility:**
- FareHarbor widgets are responsive by default
- Works on desktop, tablet, mobile devices
- No additional accessibility configuration required

**Booking Flow:**
- Item Grid → Tour selection → Availability → Booking (inline)
- Embedded Calendar → Date selection → Tour selection → Booking (inline)
- Lightframe → Modal overlay → Complete booking → Return to page

---

## Code Generation Constraints

**Important Limitations:**

**No Generic Code Available:**
- All embed codes are FareHarbor account-specific
- Codes include unique company identifier
- Cannot provide copy-paste examples without account access

**Must Generate From Dashboard:**
- Login to your FareHarbor account required
- Use embed generator for each widget
- Code is automatically customized to your account

**Account-Specific Elements:**
- Company shortname embedded in URLs
- Item IDs specific to your tour inventory
- Flow IDs specific to your flow configuration

---

## Implementation Checklist

**Pre-Implementation:**
- [ ] FareHarbor account created and active
- [ ] Tour inventory configured in Dashboard
- [ ] Flows created (if using categorized display)
- [ ] Target HTML pages identified

**Implementation:**
- [ ] Access embed generator (Dashboard → Settings → Book Buttons & Embeds)
- [ ] Configure widget type (Item Grid recommended)
- [ ] Select items to display (all tours or specific flow)
- [ ] Copy generated code
- [ ] Paste code into HTML page
- [ ] Verify container allows responsive sizing

**Post-Implementation:**
- [ ] Test desktop display
- [ ] Test tablet display
- [ ] Test mobile display
- [ ] Verify all tours appear correctly
- [ ] Test complete booking flow
- [ ] Verify booking confirmation works
- [ ] Monitor initial bookings for issues

---

## Further Investigation Resources

### Official FareHarbor Documentation

**Primary Resources:**

1. **Best Practice for Website Integration**
   - URL: `https://help.fareharbor.com/hc/en-us/articles/42957863615131-Best-Practice-for-Website-Integration`
   - Content: Recommended widget placement, integration strategies

2. **Adding FareHarbor Links and Embeds**
   - URL: `https://help.fareharbor.com/website/integrations/getting-started-with-integrations/links-and-embeds/`
   - Content: Comprehensive guide to all embed types

3. **Best Practice for Integration**
   - URL: `https://help.fareharbor.com/website/integrations/integration-best-practices/`
   - Content: Integration best practices and optimization tips

4. **Lightframe Overview**
   - URL: `https://help.fareharbor.com/website/integrations/lightframe/`
   - Content: Modal overlay implementation guide

5. **Advanced Lightframe API Usage**
   - URL: `https://help.fareharbor.com/website/integrations/lightframe/lightframe-api/`
   - Content: JavaScript API reference and advanced customization

6. **FareHarbor API Documentation (GitHub)**
   - URL: `https://github.com/FareHarbor/fareharbor-docs`
   - Content: REST API documentation for backend integration

7. **FareHarbor Developer Portal**
   - URL: `https://developer.fareharbor.com/api/external/v1/`
   - Content: API integration center

### Third-Party Resources

8. **How to Add FareHarbor Booking Platform (uSkinned)**
   - URL: `https://uskinned.net/support/how-to-add-fareharbor-booking-platform/`
   - Content: Implementation examples for CMS platforms

### WordPress-Specific (Not Applicable to KrakowBites)

9. **FareHarbor WordPress Plugin**
   - URL: `https://wordpress.org/plugins/fareharbor/`
   - Content: WordPress plugin for FareHarbor integration

10. **Add FareHarbor Booking Calendars to WordPress**
    - URL: `https://help.fareharbor.com/website/integrations/tools/wordpress-plugin/`
    - Content: WordPress-specific implementation guide

### Search Queries for Additional Research

**Recommended Search Terms:**
- "FareHarbor item grid customization"
- "FareHarbor embedded calendar configuration"
- "FareHarbor tour operator integration examples"
- "FareHarbor flow setup guide"
- "FareHarbor booking widget troubleshooting"

---

## Version History

**v1.0.0 (2026-01-02):**
- Initial documentation created
- Covers Item Grid and Embedded Calendar integration
- Includes Lightframe modal and API references
- Structured for AI-friendly parsing
- Includes comprehensive resource links

---

## Document Metadata

**Maintained By:** KrakowBites Technical Team
**Review Frequency:** Quarterly or when FareHarbor updates API
**Next Review Date:** 2026-04-02
**Related Documents:** None (initial technical documentation)
**Dependencies:** Active FareHarbor account, configured tour inventory

---

**END OF DOCUMENT**
