<!-- AGENTS.md — Ristorante Busento Website -->

> This file contains project-specific context for AI coding agents. The human-facing documentation is in `README.md` (currently the generic Astro starter README).

---

## Project Overview

This is a **static marketing website** for **Ristorante Busento**, an Italian restaurant located in Dorsten, Germany. It is built as a pure **Static Site Generation (SSG)** project with Astro and deployed to Netlify.

- **Site URL:** `https://ristorante-busento.de`
- **Language:** German (`lang="de"` on `<html>`)
- **Framework:** Astro `^6.1.8`
- **Styling:** Tailwind CSS `^4.2.2`
- **Runtime:** Node.js `>=22.12.0`
- **Module system:** ESM (`"type": "module"` in `package.json`)

The site has 8 pages (7 content pages plus a custom 404 page), all using a single root layout. There is no backend, no API routes, and no client-side JavaScript framework (React, Vue, Svelte, etc.). Interactivity is implemented with small inline `<script>` blocks inside Astro components.

---

## Project Structure

```
public/                     # Static assets served as-is
├── gallerie/               # 15 gallery images (gallerie-01.jpg … gallerie-15.jpg)
├── images/                 # Additional unprocessed images (hero.jpg, logo.jpeg, dish photos)
├── BusentoSpeisekarte.pdf  # Menu PDF downloadable from /speisekarte
├── favicon.ico
└── favicon.svg

src/
├── assets/                 # Images processed by Astro (astro:assets)
│   └── *.jpg
├── components/             # Reusable Astro components
│   ├── Button.astro        # CTA button/link with variant/size props
│   ├── Container.astro     # Max-width wrapper with size variants
│   ├── CookieBanner.astro  # Cookie consent banner (localStorage-based)
│   ├── Footer.astro        # 3-column footer with contact, opening hours, legal links
│   ├── Header.astro        # Sticky header + mobile burger menu
│   └── Section.astro       # Section wrapper with background variants
├── layouts/
│   └── Layout.astro        # Root layout: SEO meta, fonts, JSON-LD schema
├── pages/                  # File-based routing
│   ├── index.astro         # /
│   ├── speisekarte.astro   # /speisekarte
│   ├── ueber-uns.astro     # /ueber-uns
│   ├── gallerie.astro      # /gallerie
│   ├── kontakt.astro       # /kontakt
│   ├── datenschutz.astro   # /datenschutz
│   ├── impressum.astro     # /impressum
│   └── 404.astro           # /404
└── styles/
    └── global.css          # Tailwind import, @theme, base and utility styles
```

---

## Build & Development Commands

All commands run from the project root:

| Command           | Action                                                      |
| ----------------- | ----------------------------------------------------------- |
| `npm install`     | Install dependencies                                        |
| `npm run dev`     | Start Astro dev server at `localhost:4321`                  |
| `npm run build`   | Production build → `./dist/`                                |
| `npm run preview` | Preview the production build locally                        |
| `npm run astro`   | Run Astro CLI commands (e.g., `astro check`, `astro add`)   |

There is **no testing setup** in this project (no Vitest, Jest, Playwright, etc.).

---

## Technology Stack Details

### Astro Configuration (`astro.config.mjs`)
- `site`: `https://ristorante-busento.de`
- Image service: `sharp` via `astro/assets/services/sharp` with `limitInputPixels: false`
- Vite plugin: `@tailwindcss/vite`

### Tailwind CSS v4 (`src/styles/global.css`)
- **No `tailwind.config.js`** — theming is done with the CSS-native `@theme` block.
- Custom color tokens:
  - `bg` `#0e0e0c`, `bg-light` `#1a1a18`, `bg-lighter` `#252522`
  - `gold` `#d2b87b`, `gold-light` `#e4cf9e`, `gold-dark` `#b89d5e`
  - `text` `#f2efe9`, `text-muted` `#a8a49c`
  - `border` `#2a2a28`
- Custom fonts:
  - `font-serif` — Playfair Display
  - `font-sans` — Lato
  - `font-script` — Great Vibes
- Utility layer mappings replicate Tailwind class names for the custom tokens (e.g., `.text-gold`, `.bg-bg-light`).

### Fonts (via `@fontsource`)
- Playfair Display (400, 500, 700)
- Lato (300, 400, 700)
- Great Vibes (400)

### TypeScript (`tsconfig.json`)
- Extends `astro/tsconfigs/strict`
- Includes `.astro/types.d.ts`
- Excludes `dist`

---

## Code Organization & Conventions

### Component Patterns
- **Props interfaces** are defined in the frontmatter of every component for type safety.
- **Variant/size pattern:** Components like `Button.astro`, `Container.astro`, and `Section.astro` use plain objects mapping keys to Tailwind class strings, then concatenate them.
- **Conditional rendering:** `Button.astro` renders an `<a>` if `href` is provided, otherwise a `<button>`.
- **Inline client scripts:** Interactivity (mobile menu toggle, form validation, cookie banner) is implemented with inline `<script>` tags inside components rather than external `.js` files. These scripts use vanilla JS/TS.

### Styling Conventions
- **Utility-first** with Tailwind CSS. Almost no custom CSS exists outside `global.css`.
- **Responsive design** uses standard Tailwind prefixes (`md:`, `lg:`, `max-sm:`).
- **Design system:** Dark background (`#0e0e0c`) with gold accents (`#d2b87b`). Uppercase tracking-widest text is used heavily for labels, buttons, and nav links.

### Asset Handling
- **Images:** Use the `astro:assets` `Image` component with explicit `width` and `height` for optimization. Images live in `src/assets/`.
- **Static files:** PDFs, favicons, gallery images, and other unprocessed assets go in `public/`.

### SEO & Accessibility Conventions
- Every page uses `Layout.astro` which injects:
  - Open Graph and Twitter Card meta tags
  - Canonical URL (`<link rel="canonical">`)
  - Schema.org JSON-LD (`Restaurant` structured data)
  - `robots: index, follow`
- **ARIA labels** are consistently used: `aria-label`, `aria-expanded`, `aria-controls`, `aria-live`, `aria-hidden`.
- **Semantic HTML:** `<header>`, `<main>`, `<footer>`, `<nav>`, `<address>`, `<section>`, `<blockquote>`.

---

## Environment Variables

Copy `.env.example` to `.env` and fill in the real value:

```
PUBLIC_WEB3FORMS_KEY=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

- `PUBLIC_WEB3FORMS_KEY` — API key for the contact form on `/kontakt`, which submits to `https://api.web3forms.com/submit`.

Because the variable is prefixed with `PUBLIC_`, Astro inlines it into the client bundle at build time.

---

## Deployment

**Platform:** Netlify (configured via `netlify.toml`)

- Build command: `npm run build`
- Publish directory: `dist`
- Node version: `22`

**Security headers** are set for all routes:
- `X-Frame-Options: DENY`
- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy: camera=(), microphone=(), geolocation=()`
- `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`

**Caching headers** for static assets:
- `/assets/*`, `*.js`, `*.css` → `public, max-age=31536000, immutable`

There are no CI/CD workflow files (e.g., `.github/workflows/`) in the repository.

---

## External Integrations

- **Web3Forms:** Handles contact form submissions on `/kontakt`. Uses a honeypot field (`name="botcheck"`) for basic spam protection. The form is submitted via client-side `fetch` to `https://api.web3forms.com/submit`.
- **Netlify:** Static hosting with custom security and cache headers.

---

## Known TODOs / Placeholders

The codebase contains a few explicit TODOs and placeholders that should not be forgotten:

- `src/pages/gallerie.astro` — comment: `<!-- TODO: Durch echte Busento-Bilder austauschen -->`
- `src/pages/impressum.astro` — VAT ID placeholder: `[Platzhalter]`
- `src/pages/ueber-uns.astro` — TODO to replace the story image with a real photo of the owner

---

## Security Considerations

- No sensitive server-side logic; it is a fully static site.
- The Web3Forms API key is a **public** env variable (`PUBLIC_WEB3FORMS_KEY`) and is therefore visible in the client bundle. This is acceptable for Web3Forms access keys (they are meant to be public), but do not confuse it with a secret.
- Security headers are enforced at the CDN layer via `netlify.toml`.
- Contact form includes a honeypot field and client-side validation, but no server-side validation (since there is no server).

---

## Editing Guidelines for Agents

- **Language:** All user-facing copy is in **German**. Maintain German text when modifying pages.
- **Keep it static:** Do not introduce server-side rendering, API routes, or a backend framework unless explicitly requested.
- **Use existing components:** Prefer reusing `Button`, `Section`, `Container`, and `Layout` rather than inlining their markup.
- **Images:** Always use `astro:assets` `Image` component with explicit `width`/`height`. Place new images in `src/assets/`. Gallery or other static images belong in `public/`.
- **Styling:** Use Tailwind utility classes. If you need new design tokens, add them to the `@theme` block in `src/styles/global.css` and create matching utility classes in `@layer utilities`.
- **Accessibility:** Preserve ARIA attributes, semantic HTML, and focus states when modifying interactive components.
- **SEO:** When adding new pages, always wrap them in `Layout.astro` and provide `title`, `description`, and optionally `ogImage`.
