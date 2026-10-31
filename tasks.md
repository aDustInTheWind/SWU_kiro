# Tasks — Personal Portfolio

- [ ] 1. Create `index.html` skeleton
  - `<!doctype html>`, `<html lang="en">`, `<head>` with charset, viewport
    meta, and `<title>`.
  - _Covers: single-file, phone-support, no-JS constraints._

- [ ] 2. Add the inline `<style>` block
  - CSS variables for the dark palette + single accent colour.
  - Base/reset, body background and text colour, centered content column.
  - _Covers: dark background, one accent colour, inline CSS._

- [ ] 3. Build the header
  - `<h1>` name + `<p>` one-line role.
  - _Covers: content (name, role), layout (header)._

- [ ] 4. Build the three project cards
  - `<main>` with three `<article class="card">`: title, one sentence, one link.
  - _Covers: content (3 projects), layout (cards)._

- [ ] 5. Build the footer
  - Email (mailto), GitHub, LinkedIn links.
  - _Covers: content (contact links), layout (footer)._

- [ ] 6. Apply spacing + card styling
  - Generous padding/margins, card borders/rounding, link accent styling,
    `:focus-visible` outlines.
  - _Covers: generous spacing, accent colour, accessibility._

- [ ] 7. Responsive polish
  - `clamp()` sizing; verify no horizontal scroll at 360px; footer wraps.
  - _Covers: must work on a phone._

- [ ] 8. Verify constraints
  - Confirm: no `<script>` tags, no external resources, one accent colour,
    three cards, three contact links.
  - _Covers: acceptance criteria._

## Revision 2 — cursor-reactive ASCII background

- [x] 9. Add fixed full-viewport `<canvas id="ascii-bg">` behind content.
- [x] 10. Inline vanilla `<script>`: cell grid, resize handling, DPR cap.
- [x] 11. Cursor glow: distance → `t` → density ramp (`· : + * # @`) + accent
      colour interpolation with radial falloff.
- [x] 12. Input: mouse + touch; eased trailing glow; fade-out on pointer leave;
      honour `prefers-reduced-motion`.
- [x] 13. Readability: translucent cards + blur so background shows through;
      monospace headings for the techy look.
- [x] 14. Verify: exactly one inline script, no external resources, one accent
      colour, 3 cards, 3 contact links.

## Note on content
Build will use clear placeholders (`YOUR NAME`, project titles, `you@example.com`,
GitHub/LinkedIn URLs). Provide the real values anytime and I'll substitute them —
either before I build or after.
