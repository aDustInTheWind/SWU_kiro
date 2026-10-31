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
- **Zero JavaScript** — no `<script>` tags, no inline event handlers.
- All content written literally in the HTML (not generated).
- Responsive: usable and readable on a phone (narrow viewport).

## Acceptance criteria
- Opening `index.html` directly in a browser (file://) renders the full page.
- Page contains exactly one name, one role line, three project cards, and
  three contact links (email, GitHub, LinkedIn).
- No `<script>` tags anywhere in the file.
- Readable with no horizontal scrolling at 360px width.
- Only one accent colour is used throughout.

## Placeholders
Because you haven't given me the actual text yet, the first build will use
clearly-marked placeholders (e.g. `YOUR NAME`, `you@example.com`) for you to
fill in. Tell me the real values and I'll swap them in.
