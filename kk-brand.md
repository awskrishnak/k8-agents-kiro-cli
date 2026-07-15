---
name: kk-brand
description: KK brand design system — colors, typography, spacing, components, and cross-media specs for web, PowerPoint, Word, Excel, PDF. Use when creating documentation, reports, presentations, or any branded deliverable.
---

# KK Brand Design System v3

Apply when creating reports, documentation, presentations, or any branded deliverable.

## Color System

### Primary Colors
- **Indigo** `#4B39FF` — Primary brand color for figures, buttons, links, nav (CMYK: C71 M78 Y0 K0)
- **Gold** `#FDBE1B` — Accent only, sparingly (dots, highlights, badges). Never as text color due to contrast. (CMYK: C0 M25 Y89 K0)
- **Ember Orange** `#F9811F` — Tertiary accent, rarer than gold (CMYK: C0 M48 Y88 K2)

### Text & Neutrals
- **Ink Navy** `#1F1F53` — Logo type, headings, dark sections (CMYK: C62 M62 Y0 K68)
- **Body Ink** `#232336` — Body text
- **Slate** `#666B85` — Muted text, captions, metadata
- **Mist** `#F5F5FC` — Surface backgrounds, cards
- **Line** `#E2E2F0` — Borders, dividers
- **White** `#FFFFFF` — Primary background

### Extended Data Palette (for charts, series)
Order: Indigo → Gold → Indigo Light (#A79CFF) → Ink Navy → Gold Light (#FFDD85) → Slate → Ember Orange (only if 6 series isn't enough)

### Semantic Colors (never brand-substituted)
- **Success/OK** `#1E8E5A`
- **Warning/Error** `#C4432B`
- **Info** — use Indigo (#4B39FF)

### Ratio Guide
~65% white/mist, 25% indigo, 8% gold, ≤2% ember orange. Gold never carries body text (contrast fail).

## Typography

### Fonts
- **Poppins** (ExtraBold 800 / Bold 700 / SemiBold 600) — Wordmark and headings
- **Inter** (400/500/600/700) — Body and UI
- **IBM Plex Mono** (400/500) — Labels, data, code

### Google Fonts Import
```
Poppins:wght@500;600;700;800
Inter:wght@400;500;600;700
IBM+Plex+Mono:wght@400;500
```

### Type Scale
- **H1** Poppins 800 / 40px
- **H2** Poppins 700 / 28px
- **H3** Poppins 600 / 20px
- **Body** Inter 400 / 16px
- **Label** IBM Plex Mono / 13px (uppercase, letter-spacing: 0.04em)

### Fallback Stack
When fonts can't be embedded:
- Poppins → Montserrat → Trebuchet MS → Calibri
- Inter → Calibri → Arial

## Spacing Grid
8px base unit. Scale: 4px (xs), 8px (sm), 16px (md), 24px (lg), 32px (xl), 64px (2xl)

## Core Components

### Buttons
- **Primary** — Background: Indigo, Text: White, Padding: 11px 22px, Radius: 8px, Font: Inter 600 / 14px
- **Gold** — Background: Gold, Text: Ink Navy
- **Secondary** — Background: Transparent, Border: 1.5px Ink Navy, Text: Ink Navy

### Badges
- **Indigo** — Background: Indigo, Text: White, Font: IBM Plex Mono 11.5px, Padding: 4px 10px, Radius: 20px
- **Gold** — Background: rgba(253,190,27,0.18), Text: #8A6400
- **Outline** — Border: 1px Line, Text: Slate

### Cards
- Background: White, Border: 1px Line, Radius: 8px, Padding: 24px
- Category label: IBM Plex Mono 11.5px uppercase, Indigo
- Heading: Poppins 700 / 18px
- Body: Inter 14px / Slate

### Form Fields
- Label: Inter 600 / 13px, Ink Navy
- Input: Border 1.5px Line, Radius 8px, Padding 10px 12px
- Focus: Outline 2px Indigo, offset 1px

## Excel / Spreadsheet System

### Font
Calibri or Aptos 10-11pt for data, 11pt bold for headers. Don't use Poppins/Inter — breaks on copy/paste.

### Header Row
- Fill: Ink Navy (#1F1F53) or Indigo (#4B39FF)
- Text: White, bold
- Freeze row on sheets over ~20 rows

### Row Banding
Alternate White / Mist (#F5F5FC)

### Chart Series
Use extended data palette from §Extended Data Palette, in order.

### Conditional Formatting
Use semantic red/green (#C4432B / #1E8E5A), not brand colors.

### KPI/Summary Cells
Gold fill, Ink Navy bold text for headline numbers (e.g., total ARR).

## PowerPoint System (16:9)

### Canvas
1920 × 1080px, safe margin 80px all sides.

### Title Slide
- Background: Ink Navy (#1F1F53)
- Title: Poppins 800, White
- Gold underline: 4px beneath title
- Wordmark: bottom-left

### Section Divider
- Background: Indigo (#4B39FF)
- Title: Poppins 700, White
- Slide number: Gold badge top-right

### Content Slide
- Background: White
- Header: Poppins 600 / 28px, Ink Navy
- Body: Inter 400 / 18px minimum, Ink Navy or Slate
- One indigo or gold accent element per slide max

### Data/Charts
- Primary series: Indigo
- Secondary series: Slate
- Gold: for one highlighted data point only, never full series

### Footer
- IBM Plex Mono 10px, Slate
- Left: confidentiality + slide number
- Right: wordmark, 16px height max

## Word / Legal Document System

### Page Setup
A4 or Letter, 1 inch (2.54cm) margins all sides.

### Body Typeface
**Do not use Poppins or Inter for contract body.** Use:
- Times New Roman 11-12pt (formal/court-facing)
- Calibri 11pt (internal/commercial)

### Letterhead
- Wordmark-only, max 28px height, top-left
- Single 2px indigo rule beneath header
- No icon below ~24px

### Headings
Section headings in body typeface, bold, Ink Navy.

### Footer
IBM Plex Mono or Calibri 9px, Slate: company name, page X of Y, confidentiality notice.

### Watermarks
"DRAFT" or "CONFIDENTIAL" — Ink Navy 8-10% opacity, diagonal.

### Signature Block
Plain body typeface, no color, no styling.

## PDF Output

Inherits rules from source:
- **Web/design tool export** — Full brand system, embed fonts
- **Word export** — Inherits §Word/Legal rules
- **Excel export** — Inherits §Excel rules, set print area first
- **Print-intended** — Use CMYK values, not RGB

## Logo Construction

### Icon
Four abstracted "people" — gold circular heads, indigo rounded-rectangle bodies, varying heights.

### Wordmark
Bold, geometric, tight tracking, rounded corners, Ink Navy (#1F1F53).

### Clearspace
Equal to height of one figure in icon. Don't crowd.

### Minimum Size
Icon detail breaks down below ~24px height — use wordmark-only at small sizes.

## Usage Guardrails

### ✓ Correct
- Gold appears once per view as genuine accent (badge, dot, icon detail)
- Indigo carries primary actions
- Ink Navy for all text — gold never as text color

### ✗ Avoid
- Gold body text or gold buttons with white text (contrast fail)
- Overcrowding logo clearspace
- Redrawing icon with equal-height figures (staggered heights intentional)
- Gold for full chart series
- Using brand colors for semantic status (use #1E8E5A / #C4432B)

## Cross-Media Font Handling

### Website
Load Poppins + Inter via Google Fonts `<link>` — renders correctly for all visitors.

### PowerPoint/Word
Google Fonts NOT pre-installed. Either:
1. Embed fonts (File → Options → Save → Embed fonts)
2. Use fallback stack

Don't assume Poppins will render on client's laptop.

## Report Creation Guidelines

When generating reports in Excel format:

1. **Structure**
   - Use header row with Ink Navy or Indigo fill, white bold text
   - Freeze header row
   - Alternate row banding: White / Mist

2. **Typography**
   - Calibri 10-11pt for data cells
   - Calibri 11pt bold for headers
   - Never use Poppins/Inter in Excel

3. **Color Usage**
   - Charts: Use extended data palette in order (Indigo → Gold → Indigo Light → Ink Navy → Gold Light → Slate → Ember Orange)
   - Status: Use semantic colors (#1E8E5A success, #C4432B error)
   - KPI highlights: Gold fill with Ink Navy bold text

4. **Formatting**
   - Borders: Use Line (#E2E2F0) for cell borders
   - Number formats: Standard Excel formats, no custom color-coding beyond conditional formatting
   - Column widths: Auto-fit or minimum 100px for readability

5. **Charts/Graphs**
   - Use brand extended palette for series
   - Keep chart backgrounds white or mist
   - Axis labels: Calibri 10pt, Slate
   - Legend: Bottom or right, never top

6. **Branding**
   - Keep subtle: small wordmark in header/footer area only
   - Don't sacrifice data readability for brand consistency
   - Excel is a data surface first, brand surface second
