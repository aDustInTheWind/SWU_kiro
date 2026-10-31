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

---

## Revision 3 — Interactive playing-card hero (flip → fuse → reveal)

### Overview
The "Projects" area becomes an interactive hero. Four playing-card-shaped cards
(5:7 aspect) start face-down. The user flips them by **click/tap**, by
**keyboard** (Enter/Space), or by **swiping/dragging** across several in one
gesture. Face-up cards auto-fuse and level up (4 → 3 → 2 → 1). The final card
plays an anticipation animation, reveals, the page mood turns light, and a
surprise discount message + CTA appears. A subtle "play again" reset replays it.
This replaces the old static 3-item project list. Still one file; the only JS is
two inline `<script>`s (ASCII background + card game), no framework/build/backend.

### Card structure & 3D flip
- Each `.card` has two `.face` children: `.back` (shown first) and `.front`
  (`rotateY(180deg)`), with `transform-style: preserve-3d` and
  `backface-visibility: hidden` on the faces. Adding `.flipped` rotates the card
  180° to show the front.
- The back uses an accent-tinted hatched pattern + `</>` mark + corner letters so
  it reads as a "playing card". The front shows a level label, title, a
  placeholder sub, and ◆ pips.

### Fusion & leveling (4 → 3 → 2 → 1)
- When two cards are face-up, `resolve()` picks the highest-level card as the
  **survivor** and animates the other (the **absorbed**) translating toward the
  survivor, scaling down, and fading out before it's removed (`.gone`).
- The survivor levels up (`setLevel`): Lv.1 → Lv.2 → Lv.3 → Lv.4, its title
  becomes "Fusion N", pip count grows, and its accent glow intensifies
  (box-shadow alpha ramps — still the single accent colour, only opacity varies).
- A `levelpulse` keyframe gives a satisfying scale bump on each level-up.
- Fusions chain if multiple cards are already face-up (e.g. from a fast swipe).

### Final reveal
- When one card remains, `finalReveal()` runs the `anticipate` keyframe 3× (a
  charging shake/brighten), then a `finalpop`, then adds `.lit` to `<body>`.
- `.lit` swaps the CSS custom properties to a **light palette** (light bg, dark
  text) with a 1.1s transition, and dims the ASCII canvas to 25% opacity.
- The `.surprise` block fades/slides in: heading, the discount line, a pill CTA
  (mailto), and a muted "↺ Play again" reset.

### Input model
- **Click/tap:** delegated `click` on the deck → `flip(card)`.
- **Keyboard:** Enter/Space on a focused card (cards are `role="button"`,
  `tabindex="0"`).
- **Swipe/drag:** Pointer Events (`pointerdown/move/up`) unify mouse + touch +
  pen. While dragging, `elementFromPoint` finds the card under the pointer and
  flips each one once per gesture (a `swiped` map prevents double-flips). The
  deck uses `touch-action: pan-y` so vertical page scroll still works.
- An input lock (`busy`) prevents interaction mid-animation; `finished` guards
  the end state.

### Accessibility / motion
- Cards are focusable buttons with labels; CTA and reset are real `<button>`s.
- `prefers-reduced-motion: reduce` collapses transition/animation durations so
  the sequence resolves near-instantly without heavy motion.

---

## Revision 4 — Two-card collision merge + project reveal + floating CTA

### Deck
Reduced to **2 cards** (since cards fuse in pairs), so the flow is a clean
**2 → 1**.

### Collision merge (replaces absorb)
- When both cards are face-up, `maybeCollide()` computes the **midpoint** between
  the two card centers.
- Each card gets `--cx/--cy` CSS variables and the `.collide` class, animating
  it toward that midpoint (opposite directions, converging) while scaling down.
- On contact, `spawnImpact(mx, my)` creates a fixed-position `.impact` dot that
  runs the `burst` keyframe (an accent shockwave) at the collision point.
- One card is kept as the survivor and re-shown as the **merged** Lv.2 card with
  a `mergein` pop; the other is removed (`.gone`). Then the finale runs.

### Final reveal shows all projects
- The `.surprise` block now contains a `.reveal-projects` grid. Each project is
  an `<a class="proj">` (name, one-line description, "Open project →") that
  **links to that project** and opens in a new tab (`target="_blank"`,
  `rel="noopener"`). Currently `href="#"` placeholders pending real URLs.
- Grid is responsive (`auto-fit, minmax(220px, 1fr)`), so it stacks on phones.

### Floating "Let's talk" CTA
- A `.cta-float` `<button>` is `position: fixed` at bottom-right (`z-index: 5`),
  so it's reachable at any scroll position without scrolling. It mirrors the
  in-reveal CTA; both call `openTalk()` → the mailto link.

### Notes
- Single accent colour preserved; level differences and the impact use accent at
  varied opacity only. Light-mode palette still applies on reveal.
- `prefers-reduced-motion` collapses the collide/merge/impact animations.
