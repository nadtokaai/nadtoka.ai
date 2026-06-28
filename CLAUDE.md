# CLAUDE.md — Nadtoka AI site

## Who this is for
Every Claude Code session working on this repo reads this file first.
This is the single source of truth for stack, design, and workflow rules.

## Stack
- Astro (latest stable), static output only (`output: 'static'` in astro.config.mjs)
- Vanilla CSS — no Tailwind, no UI libraries, no CSS frameworks
- No database, no backend, no CMS, no client-side framework
- Cloudflare Pages deployment (build: `npm run build`, output: `dist/`)
- Node LTS (see .node-version)

## Workflow rules
- NEVER tell Vlad to preview locally. Every session ends with a push to main.
- ALWAYS commit and push before ending a session.
- Commit message format: `type: short description` (feat / fix / content / style)
- If a task touches multiple concerns, split into multiple commits.
- When adding a blog post: create the .md file in src/content/blog/, commit, push. Done.

## File structure
src/
  layouts/Base.astro       — shared head, nav, footer
  pages/
    index.astro            — homepage
    about.astro
    services.astro
    blog/index.astro       — post list
    blog/[slug].astro      — post template
    contact.astro
  content/blog/            — markdown blog posts
  styles/global.css        — all CSS variables and base styles
public/
CLAUDE.md
astro.config.mjs
wrangler.toml
.node-version

## Design system

### Typefaces (loaded from Google Fonts)
- Display / headings: Playfair Display (700, italic 400) — used for h1, h2, pullquotes
- UI / body: Space Grotesk (400, 500, 700)
- Labels / mono accents: Space Mono (400, 700) — nav, dates, section labels, tags

### Color tokens (define as CSS custom properties in :root)
--color-bg: #f7f4ee
--color-surface: #ffffff
--color-ink: #111111
--color-ink-2: #444444
--color-ink-3: #777777
--color-ink-4: #aaaaaa
--color-ink-5: #cccccc
--color-rule: #e0dcd4
--color-rule-strong: #111111

### Type scale
--text-hero: 52px / Playfair Display / 700 / lh 1.05 / ls -0.02em
--text-h2: 28px / Playfair Display / 700 / lh 1.2
--text-h3: 18px / Playfair Display / 700 / lh 1.3
--text-lede: 20px / Playfair Display / 400 italic / lh 1.45
--text-body: 15px / Space Grotesk / 400 / lh 1.75
--text-body-sm: 13px / Space Grotesk / 400 / lh 1.6
--text-label: 10px / Space Mono / 400 / uppercase / ls 0.14em / color: ink-4
--text-nav: 11px / Space Mono / 400 / uppercase / ls 0.10em

### Layout
- Max-width: 760px, centered, horizontal padding 24px
- Single column on mobile, two-column (1.3fr 1fr) on desktop for content+sidebar
- Sections separated by rules: 2px solid ink for major breaks, 1px solid rule-color for minor

### Structural elements
- Header: border-top 4px solid ink, border-bottom 1px solid ink, logo (Playfair Display) left, nav (Space Mono) right
- Section labels: Space Mono, 10px, uppercase, ls 0.14em, ink-4 — always above section content
- Numbered services: mono number (ink-5) + title (Space Grotesk 700) + description (Space Grotesk 400 ink-2)
- Post rows: border-top/bottom 1px solid rule-color, date (Space Mono ink-4), hed (Playfair Display), dek (italic ink-3)
- CTA bar: border 2px solid ink, padding 18px 20px, italic serif text left, mono link right
- Tags: Space Mono 10px uppercase, border 1px solid ink-5, padding 3px 8px, no border-radius

### Animations
ALL animations must respect `prefers-reduced-motion: reduce` — wrap in:
@media (prefers-reduced-motion: no-preference) { ... }

1. Fade-in on load: page sections fade up (translateY 16px → 0, opacity 0 → 1)
   - Stagger delay per section: 0ms, 100ms, 200ms, 300ms
   - Duration: 500ms, ease-out
   - Use a `.fade-up` class triggered by adding `.visible` on DOMContentLoaded

2. Fade up on scroll: sections below the fold use IntersectionObserver
   - threshold: 0.12
   - Same translateY + opacity transition as above
   - Class: `.scroll-reveal` → `.scroll-reveal.visible`

3. Underline draw on hover: nav links and post titles
   - Use `background: linear-gradient(currentColor, currentColor) no-repeat 0 100%`
   - background-size: 0% 1px → 100% 1px on hover
   - transition: background-size 250ms ease
   - No text-decoration. The drawn line IS the underline.

## Content rules
- No Lorem ipsum. Ever.
- No stock phrases: "leverage", "seamless", "unlock", "empower", "game-changer"
- Blog posts: real, useful content — minimum 400 words, written as if Vlad wrote it
- All copy written in first person where Vlad is speaking
- Contact page: leave [GOOGLE_FORM_LINK] as a visible TODO comment in the code

## What "done" means for any session
1. All files committed with descriptive commit messages
2. Pushed to main
3. Output a one-paragraph summary of what changed and the Cloudflare Pages build command
