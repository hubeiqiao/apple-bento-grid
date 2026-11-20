---
name: apple-bento-grid
description: |
  Create Apple-inspired bento grid presentation cards for showcasing project stats,
  timelines, and achievements. Generates self-contained HTML files with zero-gap grids,
  stat cards, pill tags, bar charts, and dark quote cards — optimized for screenshot export.
  Supports both light (Apple signature #f5f5f7) and dark (#000) themes.
  Use when the user asks to build stat cards, bento grids, project summary cards,
  dashboard cards, Apple-style presentation layouts, project overviews, achievement
  showcases, or any visual summary of numbers and milestones. Also use when the user
  wants to create slides, infographics, or visual reports with a clean Apple aesthetic.
license: MIT
compatibility: Requires internet for Google Fonts. Optional Playwright for screenshot export.
metadata:
  author: Joe Hu
  version: "1.0.0"
  date: "2026-03-17"
---

# Apple Bento Grid

## Problem

Creating visually polished presentation graphics typically requires design tools like Figma or Keynote. This skill generates Apple-quality bento grid layouts as self-contained HTML files that screenshot into pixel-perfect images — no design tool needed.

## Overview

Generate self-contained HTML files that render Apple-inspired bento card grids. Each output is a single HTML file with inline CSS — no dependencies except Google Fonts. Cards fill a tight CSS grid with minimal gaps, ready for Playwright screenshot export at 2x resolution. Both light and dark themes are available.

## Workflow

1. **Read the design system selectively**: Reference [`design-system.md`](design-system.md) as needed:
   - **Always read**: Section 2 (Zero-Gap Grid) + Section 3 (Layout Templates) — the grid foundation
   - **Read per card type**: Section 5.1–5.7 as needed for the cards you're using
   - **Read for theme**: Section 1 (light tokens) or Section 9 (dark tokens)
   - **Read once for first generation**: Section 8 (HTML Skeleton) as a structural starting point
2. **Gather the user's content** — what stats, milestones, categories, and quotes to showcase
3. **Choose a theme** — suggest light, dark, or both based on the context (see Theme Selection below)
4. **Choose a layout** — pick from the 3 layout templates based on card count and orientation
5. **Compose cards** — select card types and fill with the user's content; prioritize density (no empty-feeling cards)
6. **Ask about logos/images** — if the user has a logo or images, incorporate them (see Logos & Images below)
7. **Generate the HTML** — produce a single self-contained HTML file
8. **Visual review** — open the HTML in a browser and screenshot it to check for issues (see Visual Review below)
9. **Offer a vertical version** — after generating a landscape grid, ask: "Want a vertical (portrait) version for social media?" and generate a Template C adaptation if yes
10. **Screenshot** (optional) — use the Playwright script to capture PNGs at 2x

## Visual Review

After generating the HTML, always view the output to catch visual issues before presenting to the user. Use Playwright, browser MCP tools, or `open` to render the file, then check for:

**Orphan lines** — A single pill tag or word wrapping alone to a new row. Fix by removing one pill so the remaining pills fill evenly, or add enough pills to fill two full rows.

**Empty space** — Cards that look sparse with too much whitespace. Fix by adding a subtitle, badge, or extra description text to fill the card.

**Text overflow** — Numbers or labels that are too long for their card width. Fix by reducing font-size or abbreviating (e.g., "$2.4M" not "$2,400,000").

**Broken grid** — Visible row gaps or misaligned cards. Check: no `align-items: start`, gap is exactly 6px, all grid cells are occupied.

**Font fallback** — Text rendering in system fonts instead of Sora/DM Sans. Ensure Google Fonts link is present and page has network access.

**Screenshot clipping** — Right or bottom edges of cards cut off in the exported PNG. Fix by ensuring viewport width matches the grid CSS width exactly and the clip uses `box.x`/`box.y` from `boundingBox()`, not `x: 0, y: 0`.

If any issues are found, fix the HTML and re-check. Do not present to the user until the output passes visual review.

## Theme Selection

Suggest a theme based on context. When unsure, generate both.

| Context | Suggested Theme |
|---------|----------------|
| Slide deck / presentation | **Dark** — higher visual impact on projectors |
| Social media / portfolio | **Dark** — stands out in feeds |
| Report / document embed | **Light** — matches white page backgrounds |
| Print / PDF | **Light** — saves ink, better legibility |
| User says "Apple style" | **Light** — Apple's signature look |
| User says "modern" or "sleek" | **Dark** — contemporary feel |
| No preference stated | **Both** — generate two files, let user choose |

See design-system.md **Section 9** for complete dark theme tokens, or **Section 1** for light theme tokens.

## Output Format

Always produce a **single self-contained HTML file** with:
- `<!DOCTYPE html>` + `<html lang="en">`
- Google Fonts `<link>` tags in `<head>`
- All CSS in a single `<style>` block
- All content in `<body>` — no JavaScript needed

## Card Types Available

| Card | Use For | Key Feature |
|------|---------|-------------|
| **Hero** | Taglines, headlines | Gradient top-border, spans 2 rows |
| **Stat** | Numbers + labels | Color-coded accent per category |
| **Category** | Grouped items (phases, teams, quarters) | Color label + subtitle + pill tags |
| **Bar Chart** | Growth / comparison over time | Gradient bars, header badge |
| **Badge** | Tool attribution, featured callout | Icon pill + stat number |
| **Quote** | Mission statement, testimonial | Dark bg, white text, green `<em>` |
| **Highlight** | Hero number (3x, 10x, 100%) | Full-gradient background |

## Layout Templates

| Template | Columns | Width | Aspect Ratio | Best For |
|----------|---------|-------|--------------|----------|
| A: Horizontal | 4-col | 1200px | 52/25 | 12-16 cards, slides |
| B: Horizontal | 3-col | 1100px | 52/22 | 8-10 cards, focused |
| C: Vertical | 2-col | 600px | none (content) | 8-14 cards, social |
| D: Social 3:4 | 3-col | 1080×1440 canvas | fixed 3:4 | 8-12 tiles, 小红书 / Instagram portrait |
| E: Bento cover 3:4 | 3-col | 1080×1440 canvas | fixed 3:4 | Headline in top ~1/3 + 6-8 tiles, carousel cover |

Templates D and E are fixed-size canvases, so rows are balanced by hand (see **Premium Style & Social Formats** below) and the grid gap scales with the canvas: 12px at 1080px wide (the 6px rule at 600px × 1.8).

## Critical: Zero-Gap Grid Rules

These 5 rules are mandatory for Apple-like appearance. See design-system.md Section 2 for details.

1. **NEVER** set `align-items: start` — default `stretch` fills cells
2. Use `aspect-ratio` on horizontal layouts to lock container shape
3. Rows: `1fr` for horizontal, `auto` for vertical
4. Gap: `6px` (not 8px)
5. Every grid cell must be occupied — no empty cells

## Screenshot Export

Use the Playwright script at `scripts/screenshot.mjs` to capture pixel-perfect PNGs.

```bash
cd scripts
npm install
npx playwright install chromium
node screenshot.mjs
```

Edit the `pages` array in `screenshot.mjs` to point to your HTML files. Each entry needs: `file` (HTML path), `output` (PNG path), `viewportWidth` (match grid width).

**Critical: Viewport must match grid width.** If the viewport is wider than the grid, the grid gets centered and the clip can cut off the right edge. Always set `viewportWidth` to the exact grid CSS width (1200 for 4-col, 1100 for 3-col, 600 for 2-col).

**Critical: Clip must use element position.** When clipping to the grid element, use `box.x` and `box.y` from `boundingBox()`, not `x: 0, y: 0`. If the grid is centered in a wider viewport, `x: 0` will start the clip before the grid and cut off the right side.

**After screenshotting, always view the output image** to verify no edges are clipped. Check that the rightmost and bottommost cards are fully visible with their border-radius intact.

## Logos & Images

Users can add their own logos or images to bento grid cards. Ask the user if they have any logos or images they'd like included.

**Placement options:**

| Location | How | Best For |
|----------|-----|----------|
| Hero card corner | `<img>` with `position: absolute; top: 20px; right: 20px; width: 40px;` | Company logo |
| Badge card icon | Replace the SVG in `.badge-pill` with an `<img>` tag (`width: 16px; height: 16px; border-radius: 4px;`) | Tool/framework logo |
| Full card background | `background-image: url(...)` with overlay gradient for text readability | Feature screenshots |
| Standalone image card | `<img>` filling the card with `object-fit: cover; border-radius: 18px;` | Product photos |

**Guidelines:**
- Use `<img src="path/to/file.png">` with a local file path — the HTML is for screenshot export, not hosting
- Keep logos small (24–48px) so they don't dominate the card — except the **brand header** below
- For dark theme, ensure logos work on dark backgrounds (use white/light versions)
- Always add `alt` text for accessibility

## Content Adaptation

| User's Data | Recommended Template | Card Mix |
|---|---|---|
| 3–5 stats, no categories | C (2-col vertical) | 1 Hero + 3–5 Stats + 1 Quote |
| 6–8 stats, 1–2 categories | B (3-col horizontal) | 1 Hero + 4–6 Stats + 1–2 Categories + 1 Chart |
| 8–12 stats, 3+ categories | A (4-col horizontal) | Full mix: Hero, Stats, Categories, Chart, Badge, Quote, Highlight |
| Social / portrait format | C (2-col vertical) | Any mix, smaller fonts |

**Density rule**: Every card should feel full. If a card looks sparse, add a subtitle, badge, or pill tags. If the grid has visible empty space, either span a card across cells or add a supporting card.

## Premium Style & Social Formats

Use this for product launches, version updates and social carousels, when flat white cards look too plain. Full CSS in design-system.md **Section 10**; worked examples in `examples/joe-speaking-1-5-bento-3x4.html` and `examples/joe-speaking-1-5-bento-cover-3x4.html`.

**Premium look**
- Soft gradient tiles (a light tint of the brand colour, radial highlight in one corner) instead of flat white everywhere; keep most tiles white so the tinted ones carry weight.
- A subtle glow: a blurred radial blob of the accent colour at low opacity behind a corner of each card.
- Big gradient numbers (`background-clip: text` with the brand gradient) for the 2–3 hero figures.
- Small real-UI vignettes inside tiles — a progress bar with a target marker, a range chip with "还差 0.2", four criterion bars, a week check row with a streak, a mini recommendation card, chat bubbles — drawn the way the product's UI draws them, not generic charts.
- One dark tile and at most one "special" colour tile (e.g. the product's AI/orb colours) for contrast.

**Brand header**
- Use the product's official logo file (from the product repo or press kit); never draw a new mark.
- Centre the title in the header tile; the logo sits left of the title, about 1.3× the title's cap height (≈84–96px for a 50px title), optically centred with the text (nudge 2–4px if the icon has a built-in rim).

**Content rules for a "version update" bento**
- Show **what changed** in the version: one tile per change, titled with the change itself.
- No usage, revenue, business or personal-account statistics presented as data. Numbers inside vignettes are UI illustrations (e.g. "6.8 → 目标 7.5"). A real number is allowed only when the product itself shows it publicly (e.g. an in-app "+0.35 after 5+ practice days").
- No URLs, prices or QR codes on social images.

**Tile titles and row heights**
- Put each tile title on **one line** whenever the tile width allows; shrink the font only as much as needed.
- Balance row heights to content: no empty band under a title, no title/content collision. On fixed canvases set explicit `grid-template-rows` in px and let the last row take `1fr`.
- Before calling it done, **measure** every tile's title-to-content gap and bottom room with the `tile_check.js` snippet (design-system.md 10.6). Targets: gap ≥ 12px (≈16–24px typical), bottom room ≥ 16px, no negative values except deliberately side-by-side content.

**Social formats**
- 小红书 / Instagram portrait: **1080×1440 (3:4)**, rendered at 1× with headless Chrome (`--window-size=1080,1440`).
- **Bento cover**: a big, heavy headline in the top ~1/3 (key phrase in the brand gradient), a small logo + "Product X.Y 更新" line under it, then 6–8 of the strongest tiles below. Always view it at **350px wide** (feed thumbnail size): the headline and the big numbers must read there.

**Fonts for CJK**
- PingFang SC tops out at Semibold. For heavy CJK headlines load **Noto Sans SC 900 (Black)** from Google Fonts (`family=Noto+Sans+SC:wght@900`) and keep PingFang SC as the fallback for body text.

## Customization Checklist

1. Gather content — stats, categories, milestones, quotes
2. Choose theme — light, dark, or both
3. Choose layout — 4-col, 3-col, or 2-col
4. Name grid areas — readable names matching content
5. Select card types — Hero, Stat, Category, Chart, Badge, Quote, Highlight
6. Assign accent colors — one per category, max 3–4 accents
7. Set dimensions — viewport width matches grid width
8. Lock aspect ratio — horizontal only; omit for vertical
9. Verify density — no empty-feeling cards, no unused grid cells, gap 6px
10. Scale for orientation — vertical gets smaller fonts, padding, radius
11. Screenshot — Playwright at 2x, viewport matches grid width

## Verification

After generating a bento grid HTML file:
1. Open in Chrome or Safari — verify all grid cells are filled with no visible row gaps
2. Check font loading — Sora and DM Sans should render (not system fallback)
3. For dark theme — verify card backgrounds are #1a1a1a, not #fff
4. Screenshot at the correct viewport width (1200/1100/600) at 2x resolution
5. Confirm density — no card should have excessive whitespace
6. For fixed canvases (Templates D/E) — run the `tile_check.js` measurement (design-system.md 10.6): no title/content collision, no empty band
7. For social covers — view the PNG at 350px wide and confirm the headline and hero numbers read

## Notes

- Google Fonts requires internet access; grids fall back to system fonts offline
- Playwright screenshot is optional; users can take manual browser screenshots
- `aspect-ratio` is only for horizontal layouts; vertical layouts flow from content height
- Maximum 3–4 accent colors per grid to maintain Apple-like restraint
- This skill produces static HTML for screenshot export, not interactive dashboards
- For grids with fewer than 6 data points, Template C (2-col vertical) usually works best
- CJK headlines that must look heavy need Noto Sans SC 900; PingFang SC stops at Semibold

## Reference Files

- [`design-system.md`](design-system.md) — Complete design tokens (light + dark), card CSS/HTML, layout templates, and skeleton
- [`scripts/screenshot.mjs`](scripts/screenshot.mjs) — Playwright screenshot capture script
- [`examples/joe-speaking-1-5-bento-3x4.html`](examples/joe-speaking-1-5-bento-3x4.html) — Premium light bento, 1080×1440 version-update summary with brand header and UI vignettes
- [`examples/joe-speaking-1-5-bento-cover-3x4.html`](examples/joe-speaking-1-5-bento-cover-3x4.html) — Bento cover: heavy CJK headline on top, 8 tiles below
