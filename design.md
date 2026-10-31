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
