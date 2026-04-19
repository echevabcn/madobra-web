# MADOBRA Web — Context for Claude

Static marketing site for MADOBRA (B2B platform for hiring blue-collar workers). Deployed at https://madobra.com.

## Legal entity

- Commercial brand: **MADOBRA**
- Legal entity: **GUT PROJECTS, S.L.** (NIF B26958850) — Cartagena 14, 28028 Madrid
- Rule: `GUT PROJECTS, S.L.` appears **only in legal pages** (`terminos.html`, `privacidad.html`). Marketing copy, schema.org, meta, footers use `MADOBRA` only.

## Design system

Source of truth in `Design/` (gitignored — local reference only, never deployed):
- `madobra-design-tokens.json` — W3C design tokens (colors, type scale, spacing, radii, shadows, motion)
- `madobra-design-system/madobra-design-system.html` — component reference
- `madobra-brand-assets/svg` — logos (primary lockup cream/white, reversed navy, mono, wordmark, seal, favicon)
- `madobra-brand-guidelines-v1.0.pdf` — brand guidelines

The design tokens and rules are already applied to the production pages. If `Design/` is not present locally (e.g., fresh clone), the published HTML still carries the tokens inline.

Brand spine:
- Primary: Navy `#0E2340`
- Accent: Amber `#F58021` (used for focus, CTAs, verified states; one per screen as rule of thumb)
- Canvas: Cream `#F3EFE3`
- Typography: Inter (body) + JetBrains Mono (all metadata, IDs, numbers, eyebrows). ABC Diatype is the licensed display face but Inter is used everywhere — do not swap it.
- Radii tight: buttons/inputs `4px`, cards `8px`
- Motion: `120ms` default, `cubic-bezier(0.4, 0.0, 0.2, 1)`

## Editorial voice (applied across all pages)

The site is styled as an **industrial technical publication** — a field report / spec sheet, not a SaaS landing. This was a deliberate second pass with the `frontend-design` skill. Key motifs, reused consistently:

- **Paper grain** — SVG noise filter at 4% opacity on body (all pages).
- **Scroll progress bar** — 2px amber line on top, scroll-linked via `animation-timeline: scroll(root block)` with `@supports` fallback.
- **§ marginalia** — section marks like `§ 01 · Situación` in JetBrains Mono, left-margin of each section on desktop.
- **Brand stamp** in nav — `ES · 2026`, `CANDIDATOS`, `LEGAL · DOC 01`, etc. — mono, left-bordered by hairline.
- **Blinking caret `|`** — mono caret inside primary CTAs as a terminal-cursor detail.
- **Hand-drawn amber underline** — SVG path with `stroke-dasharray` animation under the accent word in each hero headline (`Capacítalos.` on index, `siempre listo.` on candidatos).
- **Hairline-driven layout** — 0.5px borders do the structural work; shadows reserved for genuine elevation.
- **Colophon footer** — printer's-mark style metadata row in mono at the very bottom (`© 2026 MADOBRA · Madrid · ES · <page-specific tagline>`).
- **`::selection`** — amber highlight with navy text.
- **`prefers-reduced-motion`** — disables all animations across pages.

### Page-specific visual anchors

- **`index.html`** — asymmetric hero with a typographic **"diagnostic spec sheet"** on the right: candidate #00421, five competences with ✓/△/! marks, readiness 78%, plan 14h. Shows the product output through type only, no screenshots.
- **`candidatos.html`** — asymmetric hero with a typographic **"carnet profesional"** on the right: name + initials avatar, sector/ciudad/estado, certificaciones with expiry dates (one warn case shown), completitud 96/100, "Verificado" stamp. Also: certifications cloud (catalog of carnets Madobra tracks) as amber-check pills.
- **`terminos.html` / `privacidad.html`** — document-grade framing. Header block with versioned metadata (`v2026.04.19`, vigente desde, jurisdicción/norma, idioma). Two-column numbered TOC with anchor links. Each `<h2>` has `§ NN` floating in the left margin on desktop. "Volver al inicio" link at the bottom. Document IDs `DOC 01` (privacidad) and `DOC 02` (terminos) stamped in nav and colophon.

### Link naming convention

- Legal link labels: use **"Términos y condiciones"** (not "Términos"). Appears in all footer `Legal` columns.
- Privacy link: **"Privacidad"**.

## Pending / to review

- **Legal pages content (`terminos.html`, `privacidad.html`)**
  - Data processors are listed generically ("cloud providers", "AI providers"). Replace with concrete names (hosting, model vendors, email, analytics) once infra is stable.
  - Jurisdiction set to Madrid; confirm matches company bylaws.
  - Draft written in-house — recommended to have a lawyer validate before production.
  - Versioning convention: header shows `v2026.04.19`. Bump the date string (`v<yyyy.mm.dd>`) in `doc-meta`, `colophon`, and the "Última actualización" references whenever content changes.
- **Top nav**: legal pages currently only in footer. User hasn't decided whether to surface in top nav.
- **App registration flow**: should link to `https://madobra.com/terminos.html` and `https://madobra.com/privacidad.html` with explicit checkbox at signup (both `app.madobra.com/register` and `/candidato/registro`).
- **Hero spec data** (index + candidatos) is fictional (candidate #00421, Juan Martínez, etc.) — purely visual. If brand legal flags this, swap to fully abstract placeholders (`XXX`, `—`) instead of real-looking names and dates.
- **Mobile density** on `candidatos.html` — the carnet card has several rows; check on a 375px viewport that it doesn't feel cramped. Simplify rows if so.
- **OG image** (`/og-image.png`) still reflects the previous palette — regenerate with new brand if social sharing becomes relevant.

## Tracking / analytics

Google Analytics 4 tag `G-8JDXSJ0FJF` is on all pages.

## Pages

- `index.html` — empresa landing (B2B)
- `candidatos.html` — candidato landing
- `terminos.html` — T&C
- `privacidad.html` — privacy policy
- `sitemap.xml`, `robots.txt`, `llms.txt`, `_redirects` (Netlify — static site, no SPA fallback)

## Favicons / icons

- `favicon.svg` — source of truth (Madobra seal: orange circle, navy M, cream checkmark)
- `favicon.ico` — multi-size (16 + 32) PNG-packed ICO for legacy support
- `favicon-16.png`, `favicon-32.png` — explicit PNG sizes linked from HTML
- `apple-touch-icon.png` — 180×180 for iOS
- `logo.png` — 512×512, also referenced by `site.webmanifest`
- `site.webmanifest` — PWA manifest (theme `#0E2340`, background `#F3EFE3`)

All favicons were rasterized from `Design/madobra-brand-assets/png/05-seal-alone.png` (800×800). To regenerate, run `sips -s format png -z <size> <size>` on the source seal PNG. The `favicon.ico` was assembled with a pure-Python struct script embedding the 16 and 32 PNGs.

## Deployment

- Hosted on Netlify (per `_redirects` header).
- Deploy is git-based — anything committed to `main` goes live. `Design/` is in `.gitignore` and stays local.
- Remote: `https://github.com/echevabcn/madobra-web.git`.

## Commit history (high-level)

- `fab19a5` initial B2B landing + candidates page
- `8e9dcc8` SEO foundation
- `ec334f5` GA4 tag
- `f63563c` Madobra brand system applied, legal pages, favicons regenerated
- `528dcea` editorial pass (spec-sheet/carnet heroes, document-framed legal pages, "Términos y condiciones" rename)
