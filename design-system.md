# Apple Bento Grid — Design System Reference

Complete design tokens, card components, layout templates, and HTML skeleton for generating Apple-inspired bento grid presentations. Supports both light and dark themes.

## Table of Contents

1. [Design Tokens](#1-design-tokens) — Colors, typography, spacing (light theme)
2. [Zero-Gap Grid Technique](#2-zero-gap-grid-technique) — The 5 mandatory rules
3. [Layout Templates](#3-layout-templates) — 4-col, 3-col, 2-col
4. [Base Card CSS](#4-base-card-css) — Reset, card foundation, .num/.label
5. [Card Components](#5-card-components) — Hero, Stat, Category, Bar Chart, Badge, Quote, Highlight
6. [Pill / Tag Component](#6-pill--tag-component) — Reusable tag styling
7. [Background Texture](#7-background-texture) — Optional crosshatch overlay
8. [Complete HTML Skeleton](#8-complete-html-skeleton) — Copy-paste starter template (light)
9. [Dark Theme](#9-dark-theme) — Complete dark theme tokens, card overrides, and skeleton

---

## 1. Design Tokens

### Color Palette

Assign one accent color per stat category for visual distinction. Max 3-4 accents per grid.

| Role | Hex | When to Use |
|------|-----|-------------|
| Page background | `#f5f5f7` | `body` background — Apple's signature light gray |
| Card surface | `#ffffff` | All card backgrounds |
| Primary text | `#1d1d1f` | Headings, hero taglines, dark card backgrounds |
| Secondary text | `#86868b` | Labels, subtitles, chart axis labels |
| Tertiary text | `#aeaeb2` | Dates, footnotes, lightest text |
| Pill background | `#f0f0f2` | Tag/pill backgrounds |
| Pill text | `#6e6e73` | Tag/pill text, badge labels |
| Blue | `#0071e3` | Primary stats, counts, feature numbers |
| Cyan | `#00b4d8` | Secondary stats, code metrics, alt accent |
| Red/Coral | `#ff6b6b` | Cost figures, warnings, negative numbers |
| Green (medium) | `#34d399` | Growth stats, quote emphasis, bar chart mid |
| Green (dark) | `#059669` | Badges, darkest bar gradient stop |
| Green (light) | `#6ee7b7` | Mid bar gradient stop |
| Green (lightest) | `#a7f3d0` | Lightest bar gradient stop |

**Color assignment strategy:**
- Blue `#0071e3` — primary metrics (revenue, count, features)
- Cyan `#00b4d8` — secondary metrics (users, lines, volume)
- Red `#ff6b6b` — cost/money figures, warnings
- Green `#34d399` — growth, savings, positive outcomes

### Typography

| Font | Role | Weights |
|------|------|---------|
| **Sora** | Display headings, large numbers, hero taglines, chart values | 400, 600, 700, 800 |
| **DM Sans** | Body text, labels, pills, captions, descriptions | 400, 500, 600, 700 (+ italic 400) |

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700;1,400&family=Sora:wght@400;600;700;800&display=swap" rel="stylesheet">
```

### Spacing & Surface

| Property | Horizontal Layout | Vertical Layout |
|----------|------------------|-----------------|
| Grid padding | 28px | 18–20px |
| Grid gap | 6px | 6px |
| Card padding | 20px 22px | 16px 20px |
| Card border-radius | 18px | 14–16px |
| Card box-shadow | `0 1px 8px rgba(0,0,0,0.05)` | Same |
| Number letter-spacing | -0.03em | Same |

---

## 2. Zero-Gap Grid Technique

This is the **critical technique** that makes bento grids look Apple-like.

### Five Mandatory Rules

```css
.grid {
  display: grid;
  gap: 6px;                        /* Rule 1: tight gap — 6px, not 8px */
  /* Rule 2: NEVER set align-items: start — default stretch fills cells */
}
```

1. **Cards FILL their grid cells** — default `align-items: stretch` makes every card expand to fill its row height. Never override this.
2. **Container has a locked shape** — horizontal layouts use `aspect-ratio` to prevent the grid from collapsing
3. **Rows use `1fr`** for horizontal layouts (proportional), `auto` for vertical — stretch still works within each row
4. **Gap is 6px** — tighter than typical 8px for Apple-like density
5. **Every grid cell must be occupied** — no empty cells; span one card across multiple cells if needed

### Horizontal Layout (locked shape)

```css
.grid {
  grid-template-rows: repeat(N, 1fr);  /* N = number of rows; proportional */
  aspect-ratio: 52 / 25;               /* locks container shape */
}
```

### Vertical Layout (content-driven)

```css
.grid {
  grid-template-rows: auto auto auto;  /* content-driven, one per row */
  /* No aspect-ratio — height flows from content */
}
```

### Common Mistake

```css
/* WRONG — causes visible row gaps */
.grid {
  align-items: start;           /* prevents cards from stretching */
  grid-template-rows: auto;    /* rows collapse to content height */
}
```

---

## 3. Layout Templates

Replace grid area names with your own content names.

### Template A: 4-Column Horizontal

Best for: 12-16 cards, landscape slides, full project overviews.
Width: **1200px**, Aspect ratio: **52/25** (~1200x577)

```css
body {
  margin: 0;
  background: #f5f5f7;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  position: relative;
}

.grid {
  display: grid;
  width: 1200px;
  padding: 28px;
  gap: 6px;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(4, 1fr);
  grid-template-areas:
    "a1  a2  a3  a4"
    "a1  a5  a6  a4"
    "a7  a8  a9  a9"
    "a10 a11 a12 a12";
  aspect-ratio: 52 / 25;
}
```

Notes: `a1` and `a4` span 2 rows (hero + chart). `a9` and `a12` span 2 columns.

### Template B: 3-Column Horizontal

Best for: 8-10 cards, focused topics.
Width: **1100px**, Aspect ratio: **52/22** (~1100x466)

```css
.grid {
  display: grid;
  width: 1100px;
  padding: 28px;
  gap: 6px;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: repeat(4, 1fr);
  grid-template-areas:
    "a1 a2 a3"
    "a1 a4 a3"
    "a5 a6 a6"
    "a7 a7 a7";
  aspect-ratio: 52 / 22;
}
```

### Template C: 2-Column Vertical

Best for: 8-14 cards, portrait format, social media.
Width: **600px**, Height: **content-driven** (no aspect-ratio)

```css
.grid {
  display: grid;
  width: 600px;
  padding: 18px;
  gap: 6px;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto auto auto auto auto;
  grid-template-areas:
    "a1 a1"
    "a2 a3"
    "a4 a5"
    "a6 a6"
    "a7 a7";
}
```

### Custom Grid Areas

Name areas after your content for readability:

```css
grid-template-areas:
  "hero   revenue  users   chart"
  "hero   growth   arpu    chart"
  "q1     q2       q3      q3"
  "tools  roi      quote   quote";
```

Then assign each card: `.card-hero { grid-area: hero; }` etc.

---

## 4. Base Card CSS

### Reset

```css
*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
```

### Card Foundation

```css
/* Horizontal layout */
.card {
  background: #fff;
  border-radius: 18px;
  box-shadow: 0 1px 8px rgba(0,0,0,0.05);
  padding: 20px 22px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

/* Vertical layout — reduce radius and padding */
/* .card { border-radius: 14px; padding: 16px 20px; } */

.num {
  font-family: 'Sora', sans-serif;
  font-weight: 800;
  letter-spacing: -0.03em;
  line-height: 1;
}

.label {
  font-family: 'DM Sans', sans-serif;
  font-weight: 500;
  color: #86868b;
  line-height: 1.25;
}
```

---

## 5. Card Components

### 5.1 Hero Card

Large tagline text with gradient top-border accent. Typically spans 2 rows.

**Pattern:** 2-3 lines of big text + optional subtitle in muted color + optional date/metadata.

**CSS:**

```css
.card-hero {
  grid-area: hero;  /* replace with your area name */
  align-self: stretch;
  padding: 24px;
}

/* Gradient accent bar at top */
.card-hero::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 4px;
  background: linear-gradient(90deg, #1d1d1f, #6e6e73, #1d1d1f);
  border-radius: 18px 18px 0 0;
}

.hero-tagline {
  font-family: 'Sora', sans-serif;
  font-size: 36px;       /* vertical: 28-32px */
  font-weight: 700;
  color: #1d1d1f;
  letter-spacing: -0.03em;
  line-height: 1.1;
}

/* Muted subtitle line */
.hero-sub {
  font-family: 'Sora', sans-serif;
  font-size: 36px;       /* match tagline size */
  font-weight: 700;
  color: #86868b;        /* muted gray */
  letter-spacing: -0.03em;
  line-height: 1.1;
}

/* Small metadata (dates, context) */
.hero-date {
  font-family: 'DM Sans', sans-serif;
  font-size: 12px;
  font-weight: 500;
  color: #aeaeb2;
  margin-top: 10px;
}
```

**HTML:**

```html
<div class="card card-hero">
  <div class="hero-tagline">Your Bold</div>
  <div class="hero-tagline">Headline Here.</div>
  <div class="hero-sub">Subtitle Line.</div>
  <div class="hero-date">Date range or context</div>
</div>
```

**Variant — Eyebrow hero** (for section/category focus):

```css
/* Change gradient to match section color */
.card-hero::before {
  background: linear-gradient(90deg, #00b4d8, #0071e3, #00b4d8);
}

.hero-eyebrow {
  font-family: 'DM Sans', sans-serif;
  font-size: 12px;
  font-weight: 600;
  color: #00b4d8;            /* match section accent */
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 10px;
}
```

```html
<div class="card card-hero">
  <div class="hero-eyebrow">Section Label</div>
  <div class="hero-tagline">The main</div>
  <div class="hero-tagline">statement</div>
  <div class="hero-sub">goes here.</div>
</div>
```

---

### 5.2 Stat Card

The workhorse — a single large number with a short label. One accent color per card.

**Pattern:** `.num` (large colored number) + `.label` (gray description).

**CSS:**

```css
.card-stat {
  grid-area: /* your area name */;
}

/* Size variants — pick based on number length */
.card-stat .num {
  font-size: 44px;       /* large stats (1-4 digits): 44px */
                         /* medium stats (5-7 chars): 40px */
                         /* compact stats (8+ chars): 32px */
                         /* vertical layout: reduce by ~25% */
  color: #0071e3;        /* assign per category: blue/cyan/red/green */
  margin-bottom: 4px;
}

.card-stat .label {
  font-size: 13px;       /* compact: 12px */
  line-height: 1.3;
}

/* For labels with inline emphasis */
.card-stat .label strong {
  color: #1d1d1f;
  font-weight: 600;
}
```

**HTML:**

```html
<div class="card card-stat" style="grid-area: revenue">
  <div class="num" style="color: #0071e3">$2.4M</div>
  <div class="label">Annual Revenue</div>
</div>

<div class="card card-stat" style="grid-area: users">
  <div class="num" style="color: #00b4d8">12,500</div>
  <div class="label">Active Users</div>
</div>

<div class="card card-stat" style="grid-area: cost">
  <div class="num" style="color: #ff6b6b; font-size: 32px">$180K</div>
  <div class="label">Infrastructure <strong>saved</strong> per year</div>
</div>
```

---

### 5.3 Category Card

Color-coded category name + focus subtitle + pill tags. Use for timelines, phases, departments, or any grouping.

**Pattern:** `.category-name` (colored label) + `.category-focus` (subtitle) + `.pills-wrap` (tags).

**CSS:**

```css
.card-category {
  grid-area: /* your area name */;
  padding: 18px 20px;
}

.category-name {
  font-family: 'Sora', sans-serif;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: -0.01em;
  margin-bottom: 4px;
  color: #0071e3;      /* assign unique color per category */
}

.category-focus {
  font-family: 'DM Sans', sans-serif;
  font-size: 11px;
  font-weight: 600;
  color: #1d1d1f;
  margin-bottom: 6px;
}
```

**HTML:**

```html
<div class="card card-category" style="grid-area: phase1">
  <div class="category-name" style="color: #0071e3">Phase 1</div>
  <div class="category-focus">Research & Discovery</div>
  <div class="pills-wrap">
    <span class="pill">User Interviews</span>
    <span class="pill">Market Analysis</span>
    <span class="pill">Competitive Audit</span>
    <span class="pill">Wireframes</span>
  </div>
</div>
```

---

### 5.4 Bar Chart Card

Vertical bar chart showing growth/comparison across periods. Typically spans 2 rows in horizontal layouts.

**Pattern:** Header (title + badge) + bars (value + bar + label per group).

**CSS:**

```css
.card-chart {
  grid-area: /* your area name */;
  align-self: stretch;
  padding: 18px 20px;
}

.chart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.chart-title {
  font-family: 'DM Sans', sans-serif;
  font-size: 10px;
  font-weight: 600;
  color: #86868b;
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.chart-badge {
  font-family: 'Sora', sans-serif;
  font-size: 16px;
  font-weight: 800;
  color: #059669;
}

.chart-bars {
  display: flex;
  align-items: flex-end;
  gap: 10px;
}

.bar-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  flex: 1;
}

.bar-val {
  font-family: 'Sora', sans-serif;
  font-size: 10px;
  font-weight: 700;
  color: #34d399;
}

.bar {
  width: 100%;
  border-radius: 6px;
}

.bar-lbl {
  font-family: 'DM Sans', sans-serif;
  font-size: 10px;
  font-weight: 600;
  color: #86868b;
}
```

**Bar sizing:** Height encodes value, gradient encodes intensity. Calculate proportionally: `height = (value / maxValue) * maxBarHeight`. Horizontal: maxBarHeight ~140px. Vertical: ~55px.

**Green gradient scale** (growth):
```css
.bar-1 { height: 32px;  background: linear-gradient(180deg, #a7f3d0, #6ee7b7); }
.bar-2 { height: 70px;  background: linear-gradient(180deg, #6ee7b7, #34d399); }
.bar-3 { height: 140px; background: linear-gradient(180deg, #34d399, #059669); }
```

**Alternative color scales:**
- Blue: `#bfdbfe` → `#60a5fa` → `#2563eb`
- Red: `#fecaca` → `#f87171` → `#dc2626`

**HTML:**

```html
<div class="card card-chart">
  <div class="chart-header">
    <div class="chart-title">Monthly Revenue</div>
    <div class="chart-badge">3x</div>
  </div>
  <div class="chart-bars">
    <div class="bar-group">
      <div class="bar-val">$12K</div>
      <div class="bar bar-1"></div>
      <div class="bar-lbl">Q1</div>
    </div>
    <div class="bar-group">
      <div class="bar-val">$28K</div>
      <div class="bar bar-2"></div>
      <div class="bar-lbl">Q2</div>
    </div>
    <div class="bar-group">
      <div class="bar-val">$45K</div>
      <div class="bar bar-3"></div>
      <div class="bar-lbl">Q3</div>
    </div>
  </div>
</div>
```

---

### 5.5 Badge Card

Icon badge pill + prominent stat. Use for tool attribution, partnerships, or featured callouts.

**Pattern:** `.badge` (pill with icon + label) + `.badge-num` (large number) + `.badge-lbl` (description).

**CSS:**

```css
.card-badge {
  grid-area: /* your area name */;
  padding: 20px 22px;
}

.badge {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  background: #f0f0f2;
  border-radius: 100px;
  padding: 4px 12px;
  margin-bottom: 8px;
  width: fit-content;
}

.badge span {
  font-family: 'DM Sans', sans-serif;
  font-size: 11px;
  font-weight: 600;
  color: #6e6e73;
}

.badge-num {
  font-family: 'Sora', sans-serif;
  font-size: 32px;
  font-weight: 800;
  letter-spacing: -0.03em;
  color: #34d399;        /* or any accent color */
}

.badge-lbl {
  font-family: 'DM Sans', sans-serif;
  font-size: 11px;
  color: #86868b;
  margin-top: 2px;
}
```

**HTML:**

```html
<div class="card card-badge" style="grid-area: tools">
  <div class="badge">
    <!-- Replace SVG with your own icon (16x16) -->
    <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
      <circle cx="8" cy="8" r="8" fill="#0071e3"/>
      <path d="M5 8l2 2 4-4" stroke="#fff" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
    <span>Built with React</span>
  </div>
  <div class="badge-num">$0</div>
  <div class="badge-lbl">Framework cost — open source</div>
</div>
```

**Variant — Inline stat** (number + label on same line):

```css
.inline-row {
  display: flex;
  align-items: baseline;
  gap: 8px;
  margin-bottom: 3px;
}

.inline-num {
  font-family: 'Sora', sans-serif;
  font-size: 32px;
  font-weight: 800;
  letter-spacing: -0.03em;
  color: #ff6b6b;       /* assign accent */
}

.inline-lbl {
  font-family: 'DM Sans', sans-serif;
  font-size: 13px;
  color: #86868b;
}

.inline-sub {
  font-family: 'DM Sans', sans-serif;
  font-size: 12px;
  color: #aeaeb2;
  margin-top: 2px;
}
```

```html
<div class="card" style="grid-area: savings">
  <div class="inline-row">
    <div class="inline-num">$50K</div>
    <div class="inline-lbl">saved annually</div>
  </div>
  <div class="inline-sub">Compared to previous vendor</div>
</div>
```

---

### 5.6 Quote Card

Dark background card with white text and colored emphasis. Typically spans full width or 2+ columns.

**CSS:**

```css
.card-quote {
  grid-area: /* your area name */;
  background: #1d1d1f;
  padding: 22px 28px;
  justify-content: center;
}

.quote-text {
  font-family: 'Sora', sans-serif;
  font-size: 20px;        /* vertical: 16-18px */
  font-weight: 600;
  color: #fff;
  letter-spacing: -0.01em;
  line-height: 1.35;
}

/* Colored emphasis — wrap key phrases in <em> */
.quote-text em {
  color: #34d399;          /* or any accent color */
  font-style: normal;
}
```

**HTML:**

```html
<div class="card card-quote">
  <div class="quote-text">"Ship <em>faster</em>, measure <em>everything</em>, iterate daily."</div>
</div>
```

---

### 5.7 Highlight Card

Full-gradient background for a single bold stat or multiplier.

**CSS:**

```css
.card-highlight {
  grid-area: /* your area name */;
  text-align: center;
  justify-content: center;
  align-items: center;
  align-self: stretch;
  background: linear-gradient(145deg, #059669 0%, #34d399 60%, #6ee7b7 100%);
  padding: 28px;
}

.card-highlight .num {
  font-size: 72px;        /* vertical: 48-56px */
  color: #fff;
  margin-bottom: 4px;
}

.card-highlight .label {
  font-size: 18px;        /* vertical: 14-16px */
  color: rgba(255,255,255,0.85);
  font-weight: 600;
}

.card-highlight .sub {
  font-family: 'DM Sans', sans-serif;
  font-size: 13px;
  color: rgba(255,255,255,0.6);
  margin-top: 6px;
}
```

**HTML:**

```html
<div class="card card-highlight">
  <div class="num">10x</div>
  <div class="label">Return on Investment</div>
  <div class="sub">$5K invested → $50K saved</div>
</div>
```

**Alternative gradients:**
- Blue: `linear-gradient(145deg, #1d4ed8 0%, #3b82f6 60%, #93c5fd 100%)`
- Cyan: `linear-gradient(145deg, #0e7490 0%, #06b6d4 60%, #67e8f9 100%)`
- Red: `linear-gradient(145deg, #b91c1c 0%, #ef4444 60%, #fca5a5 100%)`

---

## 6. Pill / Tag Component

Reusable tags for category cards, badge cards, or any tag list.

```css
.pills-wrap {
  display: flex;
  flex-wrap: wrap;
  gap: 0;  /* margin on pills handles spacing */
}

/* Default size — horizontal layouts */
.pill {
  display: inline-block;
  font-family: 'DM Sans', sans-serif;
  font-size: 10px;
  font-weight: 600;
  color: #6e6e73;
  background: #f0f0f2;
  border-radius: 100px;
  padding: 3px 9px;
  margin: 2px 2px 2px 0;
  white-space: nowrap;
}

/* Compact — vertical layouts */
/* .pill { font-size: 9.5px; padding: 3px 8px; margin: 1.5px 2px 1.5px 0; } */

/* Spacious — large cards or few tags */
/* .pill { font-size: 12px; padding: 4px 12px; margin: 2px 3px 2px 0; } */
```

```html
<div class="pills-wrap">
  <span class="pill">Tag One</span>
  <span class="pill">Tag Two</span>
  <span class="pill">Tag Three</span>
</div>
```

---

## 7. Background Texture

Subtle crosshatch overlay. Optional but recommended for presentation-quality output.

```css
body {
  position: relative;
}

body::before {
  content: '';
  position: absolute;
  inset: 0;
  background: repeating-conic-gradient(
    rgba(0,0,0,0.015) 0% 25%,
    transparent 0% 50%
  ) 0 0 / 4px 4px;
  pointer-events: none;
  z-index: 10;
}
```

---

## 8. Complete HTML Skeleton

Copy this skeleton and fill in your own grid areas, cards, and content. This is a 4-column horizontal layout — adapt the `.grid` CSS for other templates.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bento Grid</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700;1,400&family=Sora:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: #f5f5f7;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    min-height: 100vh;
    position: relative;
  }

  /* Optional crosshatch texture */
  body::before {
    content: '';
    position: absolute;
    inset: 0;
    background: repeating-conic-gradient(rgba(0,0,0,0.015) 0% 25%, transparent 0% 50%) 0 0 / 4px 4px;
    pointer-events: none;
    z-index: 10;
  }

  /* === GRID — customize areas, columns, rows === */
  .grid {
    display: grid;
    width: 1200px;           /* 1200 = 4-col, 1100 = 3-col, 600 = 2-col */
    padding: 28px;           /* 28px horizontal, 18px vertical */
    gap: 6px;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
    grid-template-areas:
      "hero  stat1  stat2  chart"
      "hero  stat3  stat4  chart"
      "cat1  cat2   cat3   cat3"
      "badge inline quote  quote";
    aspect-ratio: 52 / 25;   /* horizontal only; omit for vertical */
  }

  /* === CARD BASE === */
  .card {
    background: #fff;
    border-radius: 18px;     /* 14px for vertical */
    box-shadow: 0 1px 8px rgba(0,0,0,0.05);
    padding: 20px 22px;      /* 16px 20px for vertical */
    display: flex;
    flex-direction: column;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }

  .num {
    font-family: 'Sora', sans-serif;
    font-weight: 800;
    letter-spacing: -0.03em;
    line-height: 1;
  }

  .label {
    font-family: 'DM Sans', sans-serif;
    font-weight: 500;
    color: #86868b;
    line-height: 1.25;
  }

  /* === HERO === */
  .card-hero { grid-area: hero; align-self: stretch; padding: 24px; }
  .card-hero::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 4px;
    background: linear-gradient(90deg, #1d1d1f, #6e6e73, #1d1d1f);
    border-radius: 18px 18px 0 0;
  }
  .hero-tagline {
    font-family: 'Sora', sans-serif;
    font-size: 36px; font-weight: 700;
    color: #1d1d1f; letter-spacing: -0.03em; line-height: 1.1;
  }
  .hero-sub {
    font-family: 'Sora', sans-serif;
    font-size: 36px; font-weight: 700;
    color: #86868b; letter-spacing: -0.03em; line-height: 1.1;
  }
  .hero-date {
    font-family: 'DM Sans', sans-serif;
    font-size: 12px; font-weight: 500;
    color: #aeaeb2; margin-top: 10px;
  }

  /* === STATS — assign grid-area and color per card === */
  .card-stat1 { grid-area: stat1; }
  .card-stat2 { grid-area: stat2; }
  .card-stat3 { grid-area: stat3; }
  .card-stat4 { grid-area: stat4; }

  /* === CATEGORIES === */
  .card-cat1 { grid-area: cat1; padding: 18px 20px; }
  .card-cat2 { grid-area: cat2; padding: 18px 20px; }
  .card-cat3 { grid-area: cat3; padding: 18px 20px; }

  .category-name {
    font-family: 'Sora', sans-serif;
    font-size: 13px; font-weight: 700;
    letter-spacing: -0.01em; margin-bottom: 4px;
  }
  .category-focus {
    font-family: 'DM Sans', sans-serif;
    font-size: 11px; font-weight: 600;
    color: #1d1d1f; margin-bottom: 6px;
  }

  /* === PILLS === */
  .pills-wrap { display: flex; flex-wrap: wrap; gap: 0; }
  .pill {
    display: inline-block;
    font-family: 'DM Sans', sans-serif;
    font-size: 10px; font-weight: 600;
    color: #6e6e73; background: #f0f0f2;
    border-radius: 100px;
    padding: 3px 9px; margin: 2px 2px 2px 0;
    white-space: nowrap;
  }

  /* === BAR CHART === */
  .card-chart { grid-area: chart; align-self: stretch; padding: 18px 20px; }
  .chart-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
  .chart-title {
    font-family: 'DM Sans', sans-serif;
    font-size: 10px; font-weight: 600;
    color: #86868b; text-transform: uppercase; letter-spacing: 0.1em;
  }
  .chart-badge { font-family: 'Sora', sans-serif; font-size: 16px; font-weight: 800; color: #059669; }
  .chart-bars { display: flex; align-items: flex-end; gap: 10px; }
  .bar-group { display: flex; flex-direction: column; align-items: center; gap: 4px; flex: 1; }
  .bar-val { font-family: 'Sora', sans-serif; font-size: 10px; font-weight: 700; color: #34d399; }
  .bar { width: 100%; border-radius: 6px; }
  .bar-lbl { font-family: 'DM Sans', sans-serif; font-size: 10px; font-weight: 600; color: #86868b; }

  /* === BADGE === */
  .card-badge { grid-area: badge; padding: 20px 22px; }
  .badge-pill {
    display: inline-flex; align-items: center; gap: 5px;
    background: #f0f0f2; border-radius: 100px;
    padding: 4px 12px; margin-bottom: 8px; width: fit-content;
  }
  .badge-pill span { font-family: 'DM Sans', sans-serif; font-size: 11px; font-weight: 600; color: #6e6e73; }
  .badge-num {
    font-family: 'Sora', sans-serif; font-size: 32px; font-weight: 800;
    color: #34d399; letter-spacing: -0.03em;
  }
  .badge-lbl { font-family: 'DM Sans', sans-serif; font-size: 11px; color: #86868b; margin-top: 2px; }

  /* === INLINE STAT === */
  .card-inline { grid-area: inline; padding: 20px 22px; }
  .inline-row { display: flex; align-items: baseline; gap: 8px; margin-bottom: 3px; }
  .inline-num {
    font-family: 'Sora', sans-serif; font-size: 32px; font-weight: 800;
    letter-spacing: -0.03em;
  }
  .inline-lbl { font-family: 'DM Sans', sans-serif; font-size: 13px; color: #86868b; }
  .inline-sub { font-family: 'DM Sans', sans-serif; font-size: 12px; color: #aeaeb2; margin-top: 2px; }

  /* === QUOTE === */
  .card-quote { grid-area: quote; background: #1d1d1f; padding: 22px 28px; justify-content: center; }
  .quote-text {
    font-family: 'Sora', sans-serif;
    font-size: 20px; font-weight: 600;
    color: #fff; letter-spacing: -0.01em; line-height: 1.35;
  }
  .quote-text em { color: #34d399; font-style: normal; }
</style>
</head>
<body>
  <div class="grid">

    <!-- HERO — spans 2 rows -->
    <div class="card card-hero">
      <div class="hero-tagline">Your Bold</div>
      <div class="hero-tagline">Headline.</div>
      <div class="hero-sub">Subtitle Here.</div>
      <div class="hero-date">Jan 2025 — Dec 2025</div>
    </div>

    <!-- STATS — one accent color each -->
    <div class="card card-stat1">
      <div class="num" style="font-size:44px; color:#0071e3; margin-bottom:4px">1,234</div>
      <div class="label" style="font-size:13px">Primary Metric</div>
    </div>
    <div class="card card-stat2">
      <div class="num" style="font-size:44px; color:#0071e3; margin-bottom:4px">567</div>
      <div class="label" style="font-size:13px">Secondary Metric</div>
    </div>
    <div class="card card-stat3">
      <div class="num" style="font-size:40px; color:#00b4d8; margin-bottom:4px">89,000</div>
      <div class="label" style="font-size:13px">Tertiary Metric</div>
    </div>
    <div class="card card-stat4">
      <div class="num" style="font-size:32px; color:#ff6b6b; margin-bottom:3px">$42K</div>
      <div class="label" style="font-size:12px; line-height:1.3">Cost or monetary figure</div>
    </div>

    <!-- CATEGORIES — color-coded groups with pills -->
    <div class="card card-cat1">
      <div class="category-name" style="color:#0071e3">Category A</div>
      <div class="category-focus">Focus area description</div>
      <div class="pills-wrap">
        <span class="pill">Item 1</span>
        <span class="pill">Item 2</span>
        <span class="pill">Item 3</span>
      </div>
    </div>
    <div class="card card-cat2">
      <div class="category-name" style="color:#00b4d8">Category B</div>
      <div class="category-focus">Focus area description</div>
      <div class="pills-wrap">
        <span class="pill">Item 1</span>
        <span class="pill">Item 2</span>
        <span class="pill">Item 3</span>
      </div>
    </div>
    <div class="card card-cat3">
      <div class="category-name" style="color:#34d399">Category C</div>
      <div class="category-focus">Focus area description</div>
      <div class="pills-wrap">
        <span class="pill">Item 1</span>
        <span class="pill">Item 2</span>
        <span class="pill">Item 3</span>
        <span class="pill">Item 4</span>
      </div>
    </div>

    <!-- BAR CHART — spans 2 rows -->
    <div class="card card-chart">
      <div class="chart-header">
        <div class="chart-title">Growth Metric</div>
        <div class="chart-badge">3x</div>
      </div>
      <div class="chart-bars">
        <div class="bar-group">
          <div class="bar-val">120</div>
          <div class="bar" style="height:32px; background:linear-gradient(180deg,#a7f3d0,#6ee7b7)"></div>
          <div class="bar-lbl">Period 1</div>
        </div>
        <div class="bar-group">
          <div class="bar-val">280</div>
          <div class="bar" style="height:70px; background:linear-gradient(180deg,#6ee7b7,#34d399)"></div>
          <div class="bar-lbl">Period 2</div>
        </div>
        <div class="bar-group">
          <div class="bar-val">410</div>
          <div class="bar" style="height:140px; background:linear-gradient(180deg,#34d399,#059669)"></div>
          <div class="bar-lbl">Period 3</div>
        </div>
      </div>
    </div>

    <!-- BADGE -->
    <div class="card card-badge">
      <div class="badge-pill">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
          <circle cx="8" cy="8" r="8" fill="#0071e3"/>
          <path d="M5 8l2 2 4-4" stroke="#fff" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        <span>Your Tool or Brand</span>
      </div>
      <div class="badge-num">$0</div>
      <div class="badge-lbl">Description of the stat</div>
    </div>

    <!-- INLINE STAT -->
    <div class="card card-inline">
      <div class="inline-row">
        <div class="inline-num" style="color:#ff6b6b">$50K</div>
        <div class="inline-lbl">worth of something</div>
      </div>
      <div class="inline-sub">Additional context here</div>
    </div>

    <!-- QUOTE — spans 2 columns -->
    <div class="card card-quote">
      <div class="quote-text">"Your <em>key message</em> with <em>emphasis</em> on important words."</div>
    </div>

  </div>
</body>
</html>
```

---

## 9. Dark Theme

The dark theme inverts the color scheme for high-impact presentations, slide decks, and social media. Cards use subtle gradient backgrounds with radial glow accents for depth.

### 9.1 Dark Color Palette

| Role | Hex | Replaces (Light) |
|------|-----|-----------------|
| Page background | `#000000` | `#f5f5f7` |
| Card surface | `#1a1a1a` | `#ffffff` |
| Card gradient | `linear-gradient(145deg, #1a1a1a, #0d0d0d)` | none |
| Primary text | `#f5f5f7` | `#1d1d1f` |
| Secondary text | `#888888` | `#86868b` |
| Tertiary text | `#555555` | `#aeaeb2` |
| Pill background | `rgba(255,255,255,0.08)` | `#f0f0f2` |
| Pill text | `#aaa` | `#6e6e73` |
| Blue | `#2997ff` | `#0071e3` |
| Cyan | `#64d2ff` | `#00b4d8` |
| Red/Coral | `#ff453a` | `#ff6b6b` |
| Green | `#30d158` | `#34d399` |
| Purple | `#bf5af2` | — (new accent) |
| Quote card bg | `linear-gradient(135deg, #1c1c1e, #111)` | `#1d1d1f` |

**Dark accent colors** are Apple's own dark-mode palette — brighter and more saturated than light theme to pop against black.

### 9.2 Dark Card Overrides

```css
body {
  background: #000;
  color: #f5f5f7;
  -webkit-font-smoothing: antialiased;
}

/* No crosshatch texture in dark theme */

.card {
  background: #1a1a1a;
  border-radius: 20px;
  box-shadow: none;  /* shadows invisible on black; use gradients instead */
  padding: 28px 28px;
}

.num { color: #f5f5f7; }
.label { color: #888; }
```

### 9.3 Dark Card Gradient Accents

Use subtle radial glow effects to add depth to stat cards:

```css
/* Blue-tinted card */
.card-blue {
  background: linear-gradient(145deg, #0a1628, #0d0d0d);
}
.card-blue::after {
  content: '';
  position: absolute;
  top: -30px; right: -30px;
  width: 140px; height: 140px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(41,151,255,0.08) 0%, transparent 70%);
}

/* Green-tinted card */
.card-green {
  background: linear-gradient(145deg, #0a1e0a, #0d0d0d);
}

/* Purple-tinted card */
.card-purple {
  background: linear-gradient(145deg, #1a0a28, #0d0d0d);
}

/* Neutral card (no tint) */
.card-neutral {
  background: linear-gradient(145deg, #1a1a1a, #0d0d0d);
}
```

### 9.4 Dark Badge Component

```css
.badge {
  display: inline-block;
  font-size: 11px;
  font-weight: 600;
  padding: 4px 10px;
  border-radius: 100px;
  margin-top: 4px;
}

.badge-green {
  background: rgba(48, 209, 88, 0.15);
  color: #30d158;
}

.badge-blue {
  background: rgba(41, 151, 255, 0.15);
  color: #2997ff;
}

.badge-purple {
  background: rgba(191, 90, 242, 0.15);
  color: #bf5af2;
}
```

### 9.5 Dark Bar Chart

```css
.bar-1 { height: 32px;  background: linear-gradient(180deg, #2997ff, #0071e3); opacity: 0.5; }
.bar-2 { height: 70px;  background: linear-gradient(180deg, #2997ff, #0071e3); opacity: 0.65; }
.bar-3 { height: 110px; background: linear-gradient(180deg, #2997ff, #0071e3); opacity: 0.8; }
.bar-4 { height: 140px; background: linear-gradient(180deg, #2997ff, #0071e3); opacity: 1; }

.bar-val { color: #fff; }
.bar-lbl { color: #555; text-transform: uppercase; letter-spacing: 0.05em; }
```

### 9.6 Dark Quote Card

```css
.card-quote {
  background: linear-gradient(135deg, #1c1c1e, #111);
  text-align: center;
}

.quote-text {
  font-size: 24px;
  font-weight: 700;
  color: #f5f5f7;
}

/* Quote marks in muted gray */
.quote-text .q { color: #888; }

/* Emphasis — use accent color */
.quote-text em {
  color: #30d158;
  font-style: normal;
}
```

### 9.7 Dark Highlight Card

```css
.card-highlight {
  background: linear-gradient(145deg, #0e7490, #06b6d4, #67e8f9);
  /* Or green: linear-gradient(145deg, #059669, #34d399, #6ee7b7) */
  /* Or blue: linear-gradient(145deg, #1d4ed8, #3b82f6, #93c5fd) */
}

/* Same as light — white text on gradient */
.card-highlight .num { font-size: 72px; color: #fff; }
.card-highlight .label { color: rgba(255,255,255,0.85); }
```

### 9.8 Dark Theme Spacing

| Property | Dark Theme | Difference from Light |
|----------|-----------|----------------------|
| Grid gap | `6px` | Same |
| Card padding | `28px 28px` | Slightly more (was 20px 22px) |
| Card border-radius | `20px` | Slightly more (was 18px) |
| Card box-shadow | none | Replaced by gradient backgrounds |
| Number font-size | Same tiers | Same |

### 9.9 Complete Dark HTML Skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bento Grid — Dark</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700;1,400&family=Sora:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: #000;
    color: #f5f5f7;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    min-height: 100vh;
    -webkit-font-smoothing: antialiased;
  }

  .grid {
    display: grid;
    width: 1200px;
    padding: 28px;
    gap: 6px;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(3, 1fr);
    grid-template-areas:
      "stat1  stat2  stat3  stat4"
      "chart  chart  phases phases"
      "quote  quote  quote  quote";
    aspect-ratio: 52 / 25;
  }

  .card {
    border-radius: 20px;
    padding: 28px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    position: relative;
    overflow: hidden;
  }

  .num {
    font-family: 'Sora', sans-serif;
    font-weight: 800;
    font-size: 48px;
    letter-spacing: -0.03em;
    line-height: 1;
  }

  .label {
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: #888;
    margin-bottom: 12px;
  }

  .sub {
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    color: #666;
  }

  .badge {
    display: inline-block;
    font-family: 'DM Sans', sans-serif;
    font-size: 11px;
    font-weight: 600;
    padding: 4px 10px;
    border-radius: 100px;
    margin-top: 8px;
  }

  /* === STAT CARDS === */
  .card-stat1 {
    grid-area: stat1;
    background: linear-gradient(145deg, #1a1a1a, #0d0d0d);
  }
  .card-stat2 {
    grid-area: stat2;
    background: linear-gradient(145deg, #0a1628, #0d0d0d);
  }
  .card-stat3 {
    grid-area: stat3;
    background: linear-gradient(145deg, #1a1a1a, #0d0d0d);
  }
  .card-stat4 {
    grid-area: stat4;
    background: linear-gradient(145deg, #0a1e0a, #0d0d0d);
  }

  /* === CHART === */
  .card-chart {
    grid-area: chart;
    background: linear-gradient(145deg, #0d0d1a, #0d0d0d);
    padding: 28px;
  }
  .chart-bars { display: flex; align-items: flex-end; gap: 16px; flex: 1; padding-top: 16px; }
  .bar-group { display: flex; flex-direction: column; align-items: center; gap: 8px; flex: 1; }
  .bar { width: 100%; border-radius: 8px; background: linear-gradient(180deg, #2997ff, #0071e3); }
  .bar-val { font-family: 'Sora', sans-serif; font-size: 13px; font-weight: 700; color: #fff; }
  .bar-lbl { font-family: 'DM Sans', sans-serif; font-size: 12px; font-weight: 600; color: #555; text-transform: uppercase; }

  /* === PHASES === */
  .card-phases {
    grid-area: phases;
    background: linear-gradient(145deg, #1a1a1a, #111);
  }
  .phase-row { display: flex; align-items: center; gap: 16px; }
  .phase-number {
    width: 32px; height: 32px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-family: 'DM Sans', sans-serif; font-size: 14px; font-weight: 700;
  }
  .phase-text { font-family: 'DM Sans', sans-serif; font-size: 16px; font-weight: 600; color: #e5e5e7; }
  .phase-connector { width: 2px; height: 12px; background: #333; margin-left: 15px; }

  /* === QUOTE — full width === */
  .card-quote {
    grid-area: quote;
    background: linear-gradient(135deg, #1c1c1e, #111);
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 28px 48px;
  }
  .quote-text {
    font-family: 'Sora', sans-serif;
    font-size: 24px; font-weight: 700;
    color: #f5f5f7; line-height: 1.3;
  }
  .quote-text .q { color: #888; }
  .quote-text em { color: #30d158; font-style: normal; }
</style>
</head>
<body>
  <div class="grid">

    <!-- STAT CARDS — 4 across -->
    <div class="card card-stat1">
      <div>
        <div class="label">Primary Metric</div>
        <div class="num" style="color: #f5f5f7">$2.4M</div>
        <div class="sub">Description here</div>
      </div>
      <div><span class="badge" style="background: rgba(48,209,88,0.15); color: #30d158">+84% YoY</span></div>
    </div>

    <div class="card card-stat2">
      <div>
        <div class="label">Secondary Metric</div>
        <div class="num" style="color: #2997ff">12,500</div>
        <div class="sub">Description here</div>
      </div>
      <div><span class="badge" style="background: rgba(41,151,255,0.15); color: #2997ff">+25%</span></div>
    </div>

    <div class="card card-stat3">
      <div>
        <div class="label">Tertiary Metric</div>
        <div class="num" style="color: #bf5af2">340</div>
        <div class="sub">Description here</div>
      </div>
      <div><span class="badge" style="background: rgba(48,209,88,0.15); color: #30d158">Growth</span></div>
    </div>

    <div class="card card-stat4">
      <div>
        <div class="label">Cost Metric</div>
        <div class="num" style="color: #30d158">$42K</div>
        <div class="sub">Description here</div>
      </div>
      <div><span class="badge" style="background: rgba(48,209,88,0.15); color: #30d158">Optimized</span></div>
    </div>

    <!-- CHART — spans 2 columns -->
    <div class="card card-chart">
      <div class="label">Growth Over Time</div>
      <div class="chart-bars">
        <div class="bar-group">
          <div class="bar-val">3K</div>
          <div class="bar" style="height: 48px; opacity: 0.5"></div>
          <div class="bar-lbl">Q1</div>
        </div>
        <div class="bar-group">
          <div class="bar-val">7K</div>
          <div class="bar" style="height: 112px; opacity: 0.65"></div>
          <div class="bar-lbl">Q2</div>
        </div>
        <div class="bar-group">
          <div class="bar-val">10K</div>
          <div class="bar" style="height: 160px; opacity: 0.8"></div>
          <div class="bar-lbl">Q3</div>
        </div>
        <div class="bar-group">
          <div class="bar-val">12.5K</div>
          <div class="bar" style="height: 200px; opacity: 1"></div>
          <div class="bar-lbl">Q4</div>
        </div>
      </div>
    </div>

    <!-- PHASES — spans 2 columns -->
    <div class="card card-phases">
      <div class="label">Product Journey</div>
      <div style="display: flex; flex-direction: column; gap: 12px; flex: 1; justify-content: center;">
        <div class="phase-row">
          <div class="phase-number" style="background: rgba(191,90,242,0.2); color: #bf5af2">1</div>
          <div class="phase-text">Research & Discovery</div>
        </div>
        <div class="phase-connector"></div>
        <div class="phase-row">
          <div class="phase-number" style="background: rgba(41,151,255,0.2); color: #2997ff">2</div>
          <div class="phase-text">Build & Ship</div>
        </div>
        <div class="phase-connector"></div>
        <div class="phase-row">
          <div class="phase-number" style="background: rgba(48,209,88,0.2); color: #30d158">3</div>
          <div class="phase-text">Scale & Optimize</div>
        </div>
      </div>
    </div>

    <!-- QUOTE — full width -->
    <div class="card card-quote">
      <div class="quote-text"><span class="q">"</span> Your <em>key message</em> with <em>emphasis</em> on important words. <span class="q">"</span></div>
    </div>

  </div>
</body>
</html>
```
