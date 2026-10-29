# Meridian Dashboard — Design System

A portable spec for building dashboards that match the Meridian prototype's look and feel.
It is **domain-neutral**: nothing here assumes any particular subject matter. Swap in your
own data and labels, keep these tokens and component rules, and the result will visually
match the reference prototype.

> **How to use this:** copy the token block and base styles in §10 into your `<head>`,
> then build screens from the component recipes in §4–§8. Keep the tokens unchanged;
> only change content, labels, and which components you assemble.

---

## 1. Design principles

1. **Calm, light, and clerical.** White surfaces on a near-white page, hairline borders
   instead of heavy shadows, generous whitespace. The UI recedes; the data leads.
2. **One accent, many neutrals.** A single blue primary. Everything structural is greyscale.
   Colour is spent only on the accent and on *semantic status* (success / warning / critical / neutral).
3. **Status is a system, not a one-off.** Four semantic colours used consistently across
   chips, bars, dots, and icons so a reader learns the colour language once.
4. **Tabular, scannable data.** Numbers use tabular figures; tables are airy with uppercase
   micro-headers; rows are clickable and open a right-side drawer rather than a new page.
5. **Soft geometry.** Rounded corners everywhere, `squircle` corner-shape on rectangles,
   true circles kept circular (avatars, dots, status chips).
6. **Empty states are first-class.** Every "no data / not ready / rest" condition has a
   centred icon + title (+ optional one-line subtext), never a blank screen.

---

## 2. Foundations

### 2.1 Typography
- **Family:** `"Inter Display", "Inter", system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`
  - Load Inter from Google Fonts: `https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap`
  - "Inter Display" is the optical-display cut; it falls back to Inter gracefully if unavailable.
- **Numeric:** enable tabular figures on anything with numbers — `font-variant-numeric: tabular-nums` (utility class `.num`). Body sets `font-feature-settings:"tnum" 1,"cv05" 1`.
- **Weights in use:** 400 (body), 500 (controls/labels), 600 (emphasis, nav, table headers), 700 (titles, primary buttons).
- **Type scale (observed):**

  | Role | Size | Weight | Notes |
  |------|------|--------|-------|
  | Page / panel title (`h1`) | 19–22px | 700 | section titles |
  | Body / drawer paragraph | 13.5px | 400 | line-height ~1.5 |
  | Default control text | 13px | 500–600 | buttons, selects, nav |
  | Table / tab label | 12.5–13px | 500–600 | |
  | Micro-header ("eyebrow") | 11px | 600 | UPPERCASE, `letter-spacing:.05–.09em`, `--color-text-soft` |
  | Stat / big number | 19px+ | 700 | tabular |

- **Eyebrow utility:** `font-size:11px; letter-spacing:.09em; text-transform:uppercase; color:var(--color-text-soft)`.

### 2.2 Colour tokens
Light theme only (the prototype does not ship a dark mode). All values are CSS custom properties on `:root`.

**Brand / primary**
```
--color-primary:#2B7FFF;        /* primary buttons, active accents */
--color-primary-text:#027BFC;   /* primary-coloured text + active tab underline */
--color-primary-hover:#1C6EF0;
--color-primary-active:#1559CC;
--color-primary-soft:#EBF3FF;   /* tinted fills / selected day */
--color-primary-tint:#EBF3FF;
--color-primary-border:#B4D1FF;
--color-primary-ink:#027BFC;
```

**Neutrals / surfaces**
```
--color-page:#F6F8FA;           /* app background */
--color-surface:#FFFFFF;        /* cards, bars, drawer, sidebar */
--color-surface-subtle:#F9FAFB; /* hover rows */
--color-surface-muted:#F1F5F9;  /* track backgrounds, chip wells */
--color-border:#E5E7EB;
--color-border-subtle:#EEF2F6;
--color-separator:#E6EAF0;      /* shell dividers (topbar/sidebar) */
```

**Text**
```
--color-text:#111827;
--color-text-muted:#4B5563;
--color-text-soft:#6B7280;      /* captions, micro-headers, secondary */
--color-text-disabled:#9CA3AF;
--color-text-inverse:#FFFFFF;
```

**Semantic status (the 4-state system — rename to your domain, keep the colours)**
```
--c-success:#5CA423; --c-success-soft:#EFFAE6;   /* was "plan met" — green  */
--c-warning:#E58600; --c-warning-soft:#FFEED6;   /* was "partial"  — amber  */
--c-critical:#E50C00; --c-critical-soft:#FFE7E5; /* was "review"   — red    */
--c-neutral:#9CA3AF; --c-neutral-soft:#EEF1F4;   /* was "not observed" — grey */
--color-warning:#D97706; --color-warning-soft:#FEF3C7; --color-warning-ink:#92400E;
```
> In the reference prototype these are named `--c-plan / --c-partial / --c-review / --c-notobs`.
> They mean *success / warning / critical / neutral*. Keep the hexes; relabel freely.

**Chart / bar fills (soft, for stat bars)**
```
--sb-steel:#A3C9FF; --sb-steel2:#A3C9FF; --sb-grey:#AFAFBB;
--sb-green:#9CDE68; --sb-gold:#FFB54D; --sb-clay:#FF8680;
```

### 2.3 Radius
```
--r-sm:6px; --r-md:10px; --r-lg:12px; --r-xl:16px; --r-pill:999px;
```

### 2.4 Elevation (shadows)
```
--shadow-card:0 1px 2px rgba(15,23,42,.04);   /* cards sit almost flat */
--shadow-pop:0 16px 40px rgba(15,23,42,.18);  /* drawers, popovers, menus */
```
Cards rely on borders + the barely-there `--shadow-card`, **not** on drop shadows. Only
floating layers (drawer, date popover, dropdowns) use `--shadow-pop`.

### 2.5 Corner geometry
```
*{ corner-shape:squircle }                 /* soft superellipse corners on rectangles */
.avatar,.pnum,.chip-circle,.dot{ corner-shape:round }  /* keep circles perfectly round */
```

### 2.6 Spacing & layout tokens
```
--side:240px;   /* left sidebar width (0 below 920px) */
--top:52px;     /* fixed top bar height */
--ff: Inter stack (see 2.1)
```
- Content column: `max-width:1420px; margin:0 auto; padding:22px 28px 0`.
- Common gaps: 4 / 6 / 8 / 12 / 14 / 20px. Card internal padding ~14–16px.

### 2.7 Iconography
- **Lucide** (`https://unpkg.com/lucide@latest`), rendered via `<i data-lucide="name"></i>` then `lucide.createIcons()`.
- Line icons, ~`stroke-width:1.7–2`, sized 16–28px. Examples in use: `list`, `alert-circle`,
  `chevron-left/right/down`, `chevrons-up-down`, `calendar-x`, `clock`, `loader`, `bed`,
  `waves`, `map-pin`, `check`, `minus`, `x`, `pencil`, `video`/`video-off`.

---

## 3. App shell / layout

```
┌──────────────────────────────────────────────────────────┐
│ TOP BAR  (fixed, 52px, white, 1px --color-separator底)     │  brand · search · actions
├────────────┬─────────────────────────────────────────────┤
│  SIDEBAR   │  CONTENT (max-width 1420, centred)           │
│  (240px,   │  ┌─ page header (title + actions) ─────────┐ │
│   fixed,   │  ├─ main column ───────────────┬─ right ───┤ │
│   white)   │  │  stat panel                 │  rail     │ │
│            │  │  highlight / banner         │  cards    │ │
│  nav items │  │  table panel (tabs+table)   │           │ │
│            │  └─────────────────────────────┴───────────┘ │
└────────────┴─────────────────────────────────────────────┘
```

- **Top bar:** `position:fixed; height:var(--top); background:var(--color-surface); border-bottom:1px solid var(--color-separator)`.
- **Sidebar:** `position:fixed; top:var(--top); bottom:0; width:var(--side); background:var(--color-surface); border-right:1px solid var(--color-separator); padding:16px 12px`. Collapses (hidden) below 920px (`--side:0`).
- **Content wrap:** `max-width:1420px; margin:0 auto; padding:22px 28px 0`.
- **Two-column body:** a flexible main column + a fixed-ish right rail of secondary cards.
  Collapse the rail under the main column on narrow screens.

---

## 4. Core components

### 4.1 Buttons
```css
.btn{font-size:13px;font-weight:600;border-radius:var(--r-lg);
  border:1px solid var(--color-border);background:var(--color-surface);
  color:var(--color-text);height:36px;padding:0 14px;
  display:inline-flex;align-items:center;gap:7px;cursor:pointer}
.btn.primary{background:var(--color-primary);border-color:var(--color-primary);color:#fff}
.btn.primary:hover{background:var(--color-primary-hover)}
```
- Default = white "ghost" button with a hairline border. Primary = solid blue.
- Header-area buttons bump to `height:40px`. Icons sit left of the label (`gap:7px`).

### 4.2 Cards / panels
```css
.panel{background:var(--color-surface);border-radius:var(--r-lg);box-shadow:var(--shadow-card)}
```
- Everything content-bearing is a `.panel`: stat block, table container, rail cards.
- Internal padding is set per use (~14–16px). Panels stack with ~20px vertical gaps.

### 4.3 Sidebar nav items
- Row with leading Lucide icon + label, 13px/600, `--color-text-soft` default.
- Active item: `--color-text` + a subtle `--color-surface-muted`/`--color-primary-soft` fill, `border-radius:var(--r-md)`.

### 4.4 Tables
```css
.ptable{width:100%;border-collapse:collapse}
.ptable th{text-align:left;font-size:11px;letter-spacing:.05em;text-transform:uppercase;
  color:var(--color-text-soft);font-weight:600;padding:12px 12px 10px;
  border-bottom:1px solid var(--color-border);white-space:nowrap}
.ptable th.sortable{cursor:pointer;user-select:none}  /* pair with a chevrons-up-down icon */
```
- Uppercase micro-headers, hairline bottom border, airy row padding.
- **Rows are clickable** and open the right-side drawer (don't navigate away).
- Row hover: `background:var(--color-surface-subtle)`. Trailing `chevron-right` affordance.

### 4.5 Segmented control (view switch)
```css
.seg{display:inline-flex;background:var(--color-surface-muted);border-radius:9px;padding:3px}
.segb{border:0;background:none;font-size:12.5px;font-weight:600;color:var(--color-text-soft);
  padding:6px 13px;border-radius:7px;cursor:pointer}
.segb.active{background:var(--color-surface);color:var(--color-text);box-shadow:var(--shadow-card)}
```
Pill group in a muted well; the active segment is a white chip with the card shadow.

### 4.6 Underline tabs (filter tabs)
```css
.ptab{border:0;background:none;font-size:13px;font-weight:500;color:var(--color-text-soft);
  padding:8px 12px 12px;cursor:pointer;border-bottom:2px solid transparent;
  margin-bottom:-1px;display:flex;align-items:center;gap:6px}
.ptab.active{color:var(--color-text);border-bottom-color:var(--color-primary-text);font-weight:600}
```
Optional leading colour dot per tab to echo the status colour it filters to; a count in
parentheses after the label.

---

## 5. The status system (reuse this pattern verbatim)

Four semantic states, each with a **text colour** and a **soft fill**. Used three ways:

**a) Status chip (icon badge)** — a small round chip with an icon:
```css
.chip{width:20px;height:20px;border-radius:50%;display:inline-grid;place-items:center;flex:none}
.chip.success{background:var(--c-success-soft);color:var(--c-success)}   /* check icon  */
.chip.warning{background:var(--c-warning-soft);color:var(--c-warning)}   /* minus icon  */
.chip.critical{background:var(--c-critical-soft);color:var(--c-critical)}/* "!"         */
.chip.neutral{/* hollow grey ring, no fill */}
```

**b) Status tag (coloured text + dot)**:
```css
.tag{display:inline-flex;align-items:center;gap:7px;font-size:12.5px;font-weight:600}
.tag .dot{width:6px;height:6px;border-radius:50%;flex:none}
.tag.success{color:var(--c-success)} .tag.warning{color:var(--c-warning)}
.tag.critical{color:var(--c-critical)} .tag.neutral{color:var(--color-text-soft)}
```

**c) Stat bars** (horizontal "mini bar chart" in the stat panel):
```css
.sgrow{display:flex;align-items:center;gap:14px}
.sg-lab{width:…;font-size:13px;color:var(--color-text-soft)}
.sg-track{flex:1;height:10px;border-radius:4px;background:var(--color-surface-muted);overflow:hidden}
.sg-track>i{display:block;height:100%;border-radius:4px}       /* width = value %, bg = --sb-* */
.sg-val{font-weight:700}
```
Group these under short titles (e.g. two groups side by side separated by a thin divider).

> **Mapping guidance for a new domain:** keep the 4 colours and their meanings
> (good / caution / problem / inactive). Only change the words. E.g. an ops dashboard might
> use *Healthy / Degraded / Down / Offline*; a sales one *Won / At-risk / Lost / No-contact*.

---

## 6. Right-side drawer (row detail)

The signature interaction: clicking a table row slides in a drawer instead of a page nav.
```css
.scrim{position:fixed;inset:0;background:rgba(17,24,39,.4);opacity:0;pointer-events:none;
  transition:opacity .2s;z-index:60}
.scrim.open{opacity:1;pointer-events:auto}
.drawer{position:fixed;top:0;right:0;bottom:0;width:440px;max-width:94vw;
  background:var(--color-surface);box-shadow:var(--shadow-pop);
  transform:translateX(100%);transition:transform .22s ease;z-index:61;
  display:flex;flex-direction:column;overflow:auto}
.drawer.open{transform:none}
```
- **Header:** avatar/identity + title + subtitle + a close `x`.
- **Body = stacked sections.** Each section:
  ```css
  .dw-sec .l{font-size:11px;letter-spacing:.07em;text-transform:uppercase;
    color:var(--color-text-soft);font-weight:600;margin-bottom:6px}   /* section label */
  .dw-sec p{font-size:13.5px;line-height:1.5;color:var(--color-text)}
  ```
- Primary action is a full-width solid-ish button; secondary actions are small text buttons.

---

## 7. Modal, toast, popover

- **Modal:** same scrim pattern (`rgba(17,24,39,.4)`), centred white panel, `--shadow-pop`, `--r-lg`/`--r-xl`.
- **Toast:** `position:fixed;left:50%;bottom:26px`, dark pill (`background:var(--color-text);color:#fff`), `--r-md`, slides up + fades; link text inside uses light blue `#9EC5FF`.
- **Popovers (date picker, dropdown menus):** white, `--shadow-pop`, `--r-lg`, hairline border.

---

## 8. Empty / non-data states

Centred, icon-led, never blank. One component, several configs:
```css
.empty{display:flex;flex-direction:column;align-items:center;justify-content:center;
  text-align:center;gap:14px;min-height:calc(100vh - 215px);padding:24px}
.empty .ic{width:64px;height:64px;border-radius:50%;display:grid;place-items:center;
  background:var(--color-surface-muted);color:var(--color-text-soft)}
.empty .ic svg{width:28px;height:28px}
.empty .title{font-size:19px;font-weight:700;color:var(--color-text)}
.empty .sub{font-size:14px;color:var(--color-text-soft);line-height:1.6;max-width:420px}
```
Patterns observed:
| State | Icon | Title | Subtext |
|-------|------|-------|---------|
| No items scheduled | `calendar-x` | "Nothing scheduled" | one line, or none |
| Still processing | `loader` (spins) | "Still processing" | "This usually takes a little while." |
| Not started yet | `clock` | "Hasn't started yet" | time window |
| Inactive / rest | `bed` | "Rest day" | **none** — title only, centred |
| No-work / recovery | `waves` | "Recovery day" | metadata line (date · time · place) as subtext |

> When a page has *no data at all*, hide the whole page header too so only the centred
> empty card shows, and size it so the viewport doesn't scroll.

---

## 9. Interaction & motion
- Transitions are short and soft: `.2s`–`.22s ease` for drawer/scrim/toast; no bounces.
- Spinner only on genuine "processing" states (`loader` icon, slow linear spin).
- Hover: rows → subtle fill; primary button → darker blue; ghost button → keeps border.
- Keep the accent for *interactive/active* meaning (active tab underline, selected day,
  primary CTA). Don't use blue decoratively.

---

## 10. Copy-paste starter (`:root` + base)

Drop this into a new page's `<head>` to inherit the exact look, then build with §4–§8.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://unpkg.com/lucide@latest"></script>
<style>
:root{
  --color-primary:#2B7FFF; --color-primary-text:#027BFC; --color-primary-hover:#1C6EF0; --color-primary-active:#1559CC;
  --color-primary-soft:#EBF3FF; --color-primary-tint:#EBF3FF; --color-primary-border:#B4D1FF; --color-primary-ink:#027BFC;
  --color-page:#F6F8FA; --color-surface:#FFFFFF; --color-surface-subtle:#F9FAFB; --color-surface-muted:#F1F5F9;
  --color-border:#E5E7EB; --color-border-subtle:#EEF2F6; --color-separator:#E6EAF0;
  --color-text:#111827; --color-text-muted:#4B5563; --color-text-soft:#6B7280; --color-text-disabled:#9CA3AF; --color-text-inverse:#FFFFFF;
  --color-warning:#D97706; --color-warning-soft:#FEF3C7; --color-warning-ink:#92400E;
  /* semantic status: success / warning / critical / neutral */
  --c-success:#5CA423; --c-success-soft:#EFFAE6;
  --c-warning:#E58600; --c-warning-soft:#FFEED6;
  --c-critical:#E50C00; --c-critical-soft:#FFE7E5;
  --c-neutral:#9CA3AF; --c-neutral-soft:#EEF1F4;
  /* soft bar fills */
  --sb-steel:#A3C9FF; --sb-grey:#AFAFBB; --sb-green:#9CDE68; --sb-gold:#FFB54D; --sb-clay:#FF8680;
  --r-sm:6px; --r-md:10px; --r-lg:12px; --r-xl:16px; --r-pill:999px;
  --shadow-card:0 1px 2px rgba(15,23,42,.04); --shadow-pop:0 16px 40px rgba(15,23,42,.18);
  --ff:"Inter Display","Inter",system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
  --side:240px; --top:52px;
}
*{margin:0;padding:0;box-sizing:border-box;corner-shape:squircle}
.avatar,.pnum,.dot,.chip{corner-shape:round}
body{font-family:var(--ff);background:var(--color-page);color:var(--color-text);
  font-feature-settings:"tnum" 1,"cv05" 1;-webkit-font-smoothing:antialiased}
.num{font-variant-numeric:tabular-nums}
button{font-family:var(--ff)}
a{color:inherit;text-decoration:none}
</style>
```

---

## 11. Do / Don't

**Do**
- Keep one blue accent; let neutrals carry the structure.
- Use the 4 status colours consistently (chip, tag, dot, bar) for the *same* meanings.
- Prefer borders + `--shadow-card` over drop shadows for cards.
- Open detail in the right drawer; keep the list in context behind the scrim.
- Give every empty/loading/inactive state an icon + title.
- Use tabular numerals for all figures.

**Don't**
- Introduce a second accent hue or use blue as decoration.
- Add heavy shadows or hard 90° corners (keep `squircle`).
- Reuse a status colour for an unrelated meaning.
- Leave a blank screen when there's no data.

---

*This spec describes the visual language only. It carries no domain content — apply it to
whatever subject you're building.*
