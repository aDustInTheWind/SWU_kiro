# Design — Personal Portfolio

## Overview
A single `index.html` file. All styling lives in one `<style>` block in the
`<head>`. The document is a vertical stack: `header` → `main` (three project
cards) → `footer`. No JavaScript, no external assets.

## Document structure
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset>
    <meta name="viewport" content="width=device-width, initial-scale=1">  <- phone support
    <title>
    <style> ... all CSS here ... </style>
  </head>
  <body>
    <header>   name (h1) + role (p)
    <main>     three <article class="card"> project cards
    <footer>   contact links (email, GitHub, LinkedIn)
  </body>
</html>
```

## Visual design

### Colour palette (dark + one accent)
| Role             | Value     | Notes                        |
|------------------|-----------|------------------------------|
| Background       | `#0e1116` | dark base                    |
| Card surface     | `#161b22` | slightly lifted panel        |
| Primary text     | `#e6edf3` | near-white                   |
| Muted text       | `#8b949e` | role line, descriptions      |
| **Accent**       | `#5eead4` | the single accent colour     |

The accent colour is used for links, the role underline/marker, and card
hover/focus borders — nowhere else, to honour the "one accent colour" rule.

### Typography
- System font stack (no web fonts, keeps it zero-dependency).
- Name: large, bold.
- Role: smaller, muted, letter-spaced.
- Readable base size (~1rem / 16px), comfortable line-height (~1.6).

### Spacing
- Centered content column, `max-width` ~720px, auto margins.
- Generous page padding (clamps so it stays comfy on phone and desktop).
- Large vertical gaps between header, cards, and footer.
- Cards have roomy internal padding.

### Project card
Each card: title (h2), one-sentence description (muted), and one link styled in
the accent colour. Subtle border + rounded corners; on hover/focus the border
turns accent for a light touch of interactivity (CSS only).

### Footer
A simple horizontal row of three links (email / GitHub / LinkedIn) that wraps
to stacked on very narrow screens. Links in accent colour with visible
`:focus-visible` outline for keyboard accessibility.

## Responsiveness strategy
- `meta viewport` tag for correct phone scaling.
- Fluid layout using a single centered column — no multi-column grid needed, so
  it naturally collapses on narrow screens.
- `clamp()` for page padding and heading sizes so nothing needs media queries,
  though one small `@media` can tidy footer wrapping.
- No fixed widths; everything is percentage / max-width based → no horizontal
  scroll at 360px.

## Accessibility notes
- Semantic landmarks: `header`, `main`, `footer`, `article`.
- Sufficient contrast of text on dark background.
- `:focus-visible` outlines on all links.
- `lang="en"` on `<html>`.

## Explicitly out of scope
- No analytics, no cookies, no forms, no JS interactivity.
- No dark/light toggle (dark only, as requested).
- No images/favicons required (can be added later if you want).

---

## Revision 2 — Cursor-reactive ASCII background

### Overview
A fixed, full-viewport `<canvas>` (`#ascii-bg`, `z-index: 0`) sits behind the
content (`.wrap`, `z-index: 1`). A small inline vanilla `<script>` paints a grid
of monospace characters onto it each frame. This is the only JavaScript on the
page — no framework, no build step, no external libraries.

### Rendering model
- The viewport is divided into ~16px cells (`cols × rows`), recomputed on resize.
- Each frame, for every cell, we compute `t` ∈ [0, 1] from its distance to the
  (eased) cursor position:
  - outside the influence radius (~160px): `t = 0` → idle.
  - at the cursor: `t ≈ 1` → peak. A squared falloff gives a tight, punchy core.
- `t` selects a glyph from the density ramp `· : + * # @` (idle → peak) and
  interpolates colour from a dim blue-grey to the accent `#5eead4`, with alpha
  ramping from ~0.22 to ~1.0.

### Motion & input
- The glow position eases toward the real pointer (~0.18 lerp) for a smooth
  trailing feel; snaps on first input so it doesn't fly in from the corner.
- Mouse (`mousemove`) and touch (`touchstart`/`touchmove`) supported.
- Pointer leaving the window parks the glow off-screen → field returns to idle.
- `prefers-reduced-motion: reduce` disables the easing (glow tracks instantly,
  no trailing animation).

### Readability
- Cards are made slightly translucent (`rgba(22,27,34,.82)`) with a light
  `backdrop-filter: blur(2px)` so the ASCII field is subtly visible behind them
  without hurting text contrast. Headings/labels switch to a monospace stack to
  match the techy aesthetic.

### Performance notes
- `devicePixelRatio` capped at 2 to limit fill work on high-DPI screens.
- Single `requestAnimationFrame` loop; `clearRect` + `fillText` per cell. Cell
  size keeps the per-frame cell count moderate on typical viewports.

### Accessibility
- Canvas is decorative: `aria-hidden="true"`, not focusable, conveys no content.
- All page information remains in the HTML and is unaffected by the canvas.
