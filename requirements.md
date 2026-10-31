# Requirements — Personal Portfolio (single static page)

## Goal
A one-page personal portfolio delivered as a single `index.html` file, with
all CSS inline in a `<style>` block. No framework, no build step, no backend,
no JavaScript. Content is hard-coded directly in the HTML. Must work well on a
phone.

## Content requirements
1. **Name** — displayed prominently.
2. **Role** — a single one-line description, shown under the name.
3. **Three projects**, each with:
   - a title,
   - one sentence describing it,
   - one link.
4. **Contact links** in the footer:
   - email (mailto),
   - GitHub,
   - LinkedIn.

## Layout requirements
- **Header:** name + role.
- **Body:** three project cards.
- **Footer:** contact links.
- Order top to bottom: header → projects → footer.

## Style requirements
- Dark background.
- Exactly one accent colour.
- Generous spacing (comfortable padding/margins, breathing room).

## Hard constraints
- Single file: `index.html`.
- CSS inline inside one `<style>` block in the `<head>`.
- No external frameworks or CSS libraries.
- No build step and no package manager.
- No backend / server code.
- No framework and no external libraries.
- All content written literally in the HTML (not generated).
- Responsive: usable and readable on a phone (narrow viewport).

> **Change (revision 2):** the original "zero JavaScript" rule has been
> relaxed at the user's request to enable the cursor-reactive ASCII
> background. A single small inline vanilla `<script>` is now allowed. There
> is still no framework, no build step, no backend, and no external library.

## Interactive ASCII background (revision 2)
- A full-screen background behind the content made of a grid of monospace
  characters.
- Idle state: every cell is a dim low-contrast dot (`·`).
- Near the cursor, cells ramp up through a density gradient
  (`·` → `:` → `+` → `*` → `#` → `@`), peaking at `@` directly under the
  cursor, and tinted toward the single accent colour.
- The brightening follows a smooth radial falloff around the cursor.
- Works with mouse and touch; fades out when the pointer leaves.
- Respects `prefers-reduced-motion` (no trailing easing for those users).
- Must not harm readability of the page content.

## Acceptance criteria
- Opening `index.html` directly in a browser (file://) renders the full page.
- Page contains exactly one name, one role line, three project cards, and
  three contact links (email, GitHub, LinkedIn).
- At most one inline `<script>` (no external scripts, no framework).
- The ASCII background reacts to the cursor: dim `·` at rest, ramping to a
  bright accent-tinted `@` under the pointer.
- Readable with no horizontal scrolling at 360px width.
- Only one accent colour is used throughout.

## Placeholders
Because you haven't given me the actual text yet, the first build will use
clearly-marked placeholders (e.g. `YOUR NAME`, `you@example.com`) for you to
fill in. Tell me the real values and I'll swap them in.
