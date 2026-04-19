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
- Accent: Amber `#F58021` (one per screen)
- Canvas: Cream `#F3EFE3`
- Typography: ABC Diatype → Inter fallback
- Radii tight: buttons/inputs `4px`, cards `8px`
- Motion: `120ms` default, `cubic-bezier(0.4, 0.0, 0.2, 1)`

## Pending / to review

- **Legal pages content (`terminos.html`, `privacidad.html`)**
  - Data processors are listed generically ("cloud providers", "AI providers"). Replace with concrete names (hosting, model vendors, email, analytics) once infra is stable.
  - Jurisdiction set to Madrid; confirm matches company bylaws.
  - Draft written in-house — recommended to have a lawyer validate before production.
- **Top nav**: legal pages currently only in footer. User hasn't decided whether to surface in top nav.
- **App registration flow**: should link to `https://madobra.com/terminos.html` and `https://madobra.com/privacidad.html` with explicit checkbox at signup (both `app.madobra.com/register` and `/candidato/registro`).

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
