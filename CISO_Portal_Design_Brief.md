# CISO Portal — Design Redesign Brief

## Context and goal

The current UI is functional but visually inconsistent. It was built with default component styles and no unified design system. The result feels like a generic enterprise admin tool from 2015. The goal is to redesign it to feel like a premium, modern B2B SaaS product — clean, minimal, and confident. References: Linear, Vercel dashboard, Stripe, Raycast.

This brief specifies every design decision. Do not deviate from these specs. Do not use libraries like MUI, Ant Design, or Bootstrap. Build everything with Tailwind utility classes or clean hand-written CSS.

---

## What is broken today (diagnosis)

### Colors
- Risk badges use raw red/orange text on white (`color: red`, `color: orange`). This looks like unstyled HTML.
- The KPI numbers (7, 5, 2) are in different colors (red, orange, gray). Multicolor numbers are visually noisy.
- Progress bars use 4 different colors per row. No coherent palette.
- The active sidebar item is a flat harsh blue block with no refinement.
- Colors used throughout are not from the FCI brand palette — navy, near-black, generic grays.

### Typography
- Page titles are too heavy (`font-weight: bold` or 700). Should feel calm, not loud.
- The wrong font — must use Inter Tight for headings, Inter for body.
- KPI numbers are large but not elegant — they have no lightness to them.
- Table rows are too small and cramped — no breathing room.
- Column headers lack visual separation from data rows.

### Layout and spacing
- The sidebar has no visual depth or refinement. It looks like a text list.
- Section labels (DEVICES, POLICIES, SETTINGS) are styled inconsistently.
- The KPI cards have no consistent card treatment — they feel like floated divs.
- There is almost no whitespace anywhere. Everything sits edge-to-edge.

### Components
- Badges/tags for "High Risk", "Low Risk", "Informational" use colored text on white. They need pill-shaped, pastel background treatments.
- The filter chips at the top of the table have no consistent style.
- The top navigation bar mixes separators and styles poorly.

---

## Design system — FCI official brand palette

This is the ONLY palette. Every color used in the UI must come from this list. Do not introduce grays, near-blacks, navy, or any color outside this set.

```css
:root {
  /* Brand blues */
  --blue:        #2563EB;   /* PRIMARY — Electric Blue. Brand mark, hero bg, buttons, active states */
  --blue-dark:   #1D4ED8;   /* Hover state for blue elements */
  --blue-deep:   #1E3A8A;   /* Deepest blue — use for sidebar bg */
  --blue-mid:    #3B82F6;   /* Mid blue — secondary interactive elements */
  --sky:         #0EA5E9;   /* ACCENT — Sky Blue. Section labels, data highlights, indicators. Use sparingly */
  --sky-light:   #BAE6FD;   /* Light sky — borders on sky-colored elements */
  --blue-tint:   #DBEAFE;   /* Callout backgrounds, subtle highlights, info banners */
  --blue-faint:  #EFF6FF;   /* Active chip backgrounds, hover tints */

  /* Surfaces */
  --white:       #FFFFFF;   /* Cards, table rows, top bar */
  --offwhite:    #F8FAFC;   /* Page background, table header bg */
  --rule:        #E2E8F0;   /* All dividers, card borders, input borders */

  /* Text — all blue-tinted, never pure gray or black */
  --text:        #1E3A5F;   /* ALL headlines and primary text — Deep Blue-Slate */
  --text-body:   #475569;   /* Body text, descriptions, secondary labels */
  --text-muted:  #94A3B8;   /* Captions, column headers, placeholder text */
  --text-faint:  #CBD5E1;   /* Disabled states, very subtle labels */
}
```

Rules for color use:
- Never use `#000000`, `#111111`, `#0F172A`, or any near-black. Use `--text` (#1E3A5F) for all primary text.
- Never introduce colors not in this palette — no generic grays, no warm tones.
- The sidebar uses `--blue-deep` (#1E3A8A) as background — the darkest blue in the palette. NOT navy, NOT #0F172A.
- Status/semantic colors (red for critical, amber for warning) are the ONLY exception and are used only for status badges and dots — never for backgrounds or text outside of badges.

### Status colors — semantic use only

These are the only colors allowed outside the FCI palette, and only for communicating compliance status:

```css
--status-critical-bg:   #FEF2F2;
--status-critical-text: #B91C1C;
--status-critical-dot:  #EF4444;

--status-warning-bg:    #FFFBEB;
--status-warning-text:  #92400E;
--status-warning-dot:   #F59E0B;

--status-info-bg:       #EFF6FF;   /* use blue-faint for info — stays in brand palette */
--status-info-text:     #1D4ED8;
--status-info-dot:      #2563EB;

--status-ok-bg:         #F0FDF4;
--status-ok-text:       #166534;
--status-ok-dot:        #22C55E;
```

Never use status colors for decorative purposes. Use them only to convey meaningful compliance state.

---

## Typography

Fonts: `Inter Tight` for headings (import weight 500, 600), `Inter` for body text (import weight 300, 400, 500). Both from Google Fonts.

```
Page title (H1):      font-family: 'Inter Tight', sans-serif; font-size: 26px; font-weight: 600; letter-spacing: -0.02em; color: var(--text)
Section label (H2):   font-family: 'Inter', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--sky)
Body / table data:    font-family: 'Inter', sans-serif; font-size: 14px; font-weight: 400; color: var(--text); line-height: 1.5
Secondary text:       font-family: 'Inter', sans-serif; font-size: 13px; font-weight: 400; color: var(--text-body)
Caption / label:      font-family: 'Inter', sans-serif; font-size: 12px; font-weight: 400; color: var(--text-muted)
KPI number:           font-family: 'Inter Tight', sans-serif; font-size: 40px; font-weight: 800; letter-spacing: -0.03em; color: var(--text)
Column header:        font-family: 'Inter', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; color: var(--text-muted)
Badge text:           font-family: 'Inter', sans-serif; font-size: 11px; font-weight: 500
```

Rules:
- Never use `font-weight: 700` or `bold` anywhere in the UI. Maximum weight is 600.
- KPI numbers are always weight 300 — lightness signals sophistication.
- Section labels (DEVICES, POLICIES, SETTINGS in the sidebar) use Sky Blue (`--sky`) not muted gray.
- Limit yourself to 3 type sizes per page.

---

## Spacing — 8pt grid

All padding, margin, and gap values must be multiples of 4 or 8.

```
Sidebar width:          220px
Top bar height:         52px
Card padding:           20px 24px
Table row height:       44px (padding: 10px 16px)
Section gap:            24px
Card border-radius:     8px
Badge border-radius:    100px (pill)
Page body padding:      32px
```

### Shadows

```css
/* Card */
box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);

/* Sidebar */
/* No shadow — use border-right: 1px solid rgba(255,255,255,0.08) */
```

---

## Component specifications

### Sidebar

Philosophy: the sidebar is the same color family as the page — it does not create contrast, it creates calm. No dark background. No white-on-dark. The whole UI lives in one light monochromatic space, like Claude, Notion, or Apple Notes.

- Background: `var(--blue-faint)` (#EFF6FF) — the lightest brand blue. Barely distinct from the page.
- Border-right: `1px solid var(--blue-tint)` (#DBEAFE) — the only thing that separates it from the content.
- Width: 216px, full height.
- Logo area: 52px, border-bottom `1px solid var(--blue-tint)`
  - "FCI" — 'Inter Tight', 17px, weight 600, color `var(--blue)` (#2563EB) — Electric Blue on light bg
  - "CISO Portal" — 'Inter', 12px, weight 400, color `var(--text-muted)`
- Section labels: 10px, uppercase, letter-spacing 0.09em, color `var(--sky)` at 70% opacity
- Nav items: 34px tall, padding 0 10px, border-radius 6px, margin 1px 8px
  - Default: color `var(--text-body)`, no background, `border-left: 2px solid transparent`
  - Hover: background `rgba(37,99,235,0.06)`, color `var(--text)`
  - Active: color `var(--blue)`, font-weight 500, `border-left: 2px solid var(--blue)`, background `rgba(37,99,235,0.08)` — Electric Blue text, no filled block
- Icons: 14px Lucide icons, `stroke-width: 1.75`, `opacity: 0.6` default, `opacity: 1` active
- Collapse button: 12px, color `var(--text-faint)`, chevron icon

### Top navigation bar

- Background: `#FFFFFF`, border-bottom: `1px solid var(--rule)`
- Height: 52px, padding 0 24px
- Search: 280px wide, height 32px, border `1px solid var(--rule)`, border-radius 6px, background `var(--offwhite)`, font 13px, placeholder color `var(--text-muted)`
- Company/group selectors: 13px, color `var(--text-body)`, border `1px solid var(--rule)`, border-radius 6px, height 32px, padding 0 10px, chevron icon
- Icon buttons: 32px square, border-radius 6px, color `var(--text-muted)`, hover background `var(--offwhite)`
- Avatar: 32px circle, background `var(--blue)` (#2563EB), white, 12px, weight 600

### KPI summary cards

Three cards in a row. Each:
- White background, border `1px solid var(--rule)`, border-radius 8px, padding 20px 24px
- Shadow: `0 1px 3px rgba(0,0,0,0.06)`
- Label row: small colored dot (8px) + label in 11px uppercase, `var(--text-muted)`, letter-spacing 0.07em
- Number: 40px, 'Inter Tight', weight 800 (extra-bold), `letter-spacing: -0.03em`, color `var(--text)` — SAME color for all three. The dot carries the severity, not the number.
- Watermark icon: position absolute, bottom-right corner of card, 56×56px SVG, `opacity: 0.07`, `pointer-events: none`. Use a triangle-alert for High Risk, circle-info for Low Risk, circle-info (inverted) for Informational. Stroke color matches the severity dot color. This adds visual texture without distracting from the data.
- Sub-label: "devices with issues" — 12px, `var(--text-muted)`

### Data table

Container: white bg, border `1px solid var(--rule)`, border-radius 8px, overflow hidden, shadow.

Column headers:
- Background: `var(--offwhite)` (#F8FAFC)
- Text: 11px, uppercase, weight 600, `var(--text-muted)`, letter-spacing 0.06em
- Border-bottom: `1px solid var(--rule)`
- Padding: 10px 16px

Data rows:
- Height: 44px, padding 10px 16px
- Border-bottom: `1px solid #F1F5F9`
- Hover: background `#F8FAFC`
- Font: 14px, weight 400, color `var(--text)`

### Status badges — CRITICAL

Remove all current badge styles entirely. Replace with:

```css
/* Anatomy: border-radius 100px, padding 2px 8px, font-size 11px, font-weight 500 */

.badge-critical  { background: #FEF2F2; color: #B91C1C; }
.badge-warning   { background: #FFFBEB; color: #92400E; }
.badge-info      { background: var(--blue-faint); color: var(--blue-dark); }  /* stays in brand palette */
```

Never: colored text on plain white, all-caps inside badges, bold text inside badges.

### Progress bars

- Track: `#F1F5F9`, height 4px, border-radius 2px
- Fill: `var(--blue)` (#2563EB) — single color for ALL bars. No per-row color variation.
- Exception only: bars at exactly 100% use `#22C55E` (green) to signal completion
- Percentage text: 13px, weight 500, `var(--text-body)`

### Filter chips

```css
/* Default */
background: transparent; color: var(--text-body); border: 1px solid var(--rule); border-radius: 6px; padding: 4px 12px; font-size: 13px;

/* Active */
background: var(--blue-faint); color: var(--blue); border-color: var(--blue-tint); font-weight: 500;
```

Gap between chips: 6px.

### Buttons

```
Primary:    background var(--blue), color white, border-radius 6px, padding 8px 14px, 14px, weight 500
Secondary:  background white, color var(--text), border 1px solid var(--rule), same padding
Hover:      primary → var(--blue-dark); secondary → background var(--offwhite)
```

---

## Page-level layout

```
[Sidebar 220px #1E3A8A] | [Main flex-1]
                              [Topbar 52px white]
                              [Page body: padding 32px, background var(--offwhite)]
                                [Page header margin-bottom 24px]
                                [KPI row: 3 cards gap 16px, margin-bottom 24px]
                                [Filter chips margin-bottom 16px]
                                [Table]
```

Page header:
- Title: 'Inter Tight', 20px, weight 500, `var(--text)`
- Subtitle: 'Inter', 13px, `var(--text-body)`, margin-top 3px
- No icons, no colored bars, no decorative elements

---

## Explicit do's and don'ts

DO:
- Use whitespace generously — if something feels tight, add 8px.
- Use 'Inter Tight' weight 300 for large KPI numbers.
- Keep status colors as small accents only — dots and badge backgrounds.
- Use Sky Blue (#0EA5E9) for section labels and data indicators, not for backgrounds.
- Use Electric Blue (#2563EB) for all interactive elements — one accent color only.
- Use `var(--text)` #1E3A5F for all primary text — never near-black or pure black.

DO NOT:
- Do not use `font-weight: 700` or `bold` anywhere.
- Do not use dark navy (#0F172A or similar) — it is not in the FCI palette.
- Do not use pure black (#000000) or near-black for text — use #1E3A5F.
- Do not use generic gray tones (#666, #999, #333) — use the FCI text variables.
- Do not color text red or orange directly — use pastel badge backgrounds instead.
- Do not use more than 2 colors for progress bars (blue for in-progress, green for 100%).
- Do not add gradients to any background.
- Do not use MUI, Ant Design, Bootstrap, or Chakra.

---

## Google Fonts import

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter+Tight:wght@300;500;600;800&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
```

---

## Implementation order

1. CSS variables at `:root` — copy the palette block above exactly
2. Google Fonts import
3. Global reset and body styles (background: var(--offwhite), color: var(--text), font: Inter)
4. Sidebar — get this right first, it sets the tone
5. Top navigation bar
6. Page layout shell
7. KPI cards
8. Filter chips
9. Table — headers, rows, badges, progress bars
10. Remaining pages using the same components

Do not start the next component until the previous one matches this brief.
