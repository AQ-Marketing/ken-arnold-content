# Ken Arnold Home Inspection — AutoForge content repo

Per-client **content** for Ken Arnold Home Inspection LLC, in the layout the
AutoForge engine's importer expects. This is the *data* half of the two-repo
model: the engine (plugin `aq-core` + stub theme `aqm-base`) is brand-neutral
and client-agnostic; **this** repo paints Ken Arnold's brand onto it at import
time. Nothing here lives in the engine.

> ⚠️ **Real client data — not a test fixture.** Do not import this into a demo
> or test site, and never copy it into the engine repo.

## Contents

```
content/
  brand.json            NAP, license #587, logo, nav, footer, towns, GHL booking URL, fonts
  design.json           brand colors (navy/copper + forest) + fonts (Merriweather/Inter) + maxWidth
  assets/
    main.css            the EXACT compiled stylesheet (57 KB) — Ken Arnold's real look
    site.js             front-end behavior (sticky bar, mega-menu, FAQ, reveal)
    images/             135 images: 131 referenced by pages + logo.png / logo-light.png (+@320)
  pages/                84 page records (79 section-composed pages + 5 blog posts)
  schema/               the page + components contract (reference only)
```

## Provenance (where each piece came from)

- **pages/** — copied verbatim from the pilot `ken-arnold-wp/content/pages` (the
  already-converted engine-format site). Section catalogue + `field-order.json`
  are **identical** to the current engine, and all 84 pass `normalize-page.mjs`
  with zero errors.
- **assets/main.css + site.js** — the pilot's live `theme/kenarnold/assets`,
  i.e. the real compiled stylesheet, so the look is pixel-faithful.
- **design.json** — Ken Arnold's exact Tailwind tokens (navy `#2B3158`/`#252A46`,
  copper `#F9AB3D`, forest `#5e7d44`; Merriweather/Inter/Poppins/Open Sans),
  recovered from the AutoForge engine's pre-neutralization git history.
- **brand.json** — transcribed faithfully from the pilot's `config/site.php`
  (the "single source of truth for NAP", a direct port of the Astro
  `src/config/site.ts`). Logo references converted to filenames; the exact
  Google Fonts URL wired into `fonts.googleCss`.
- **images/** — pulled from the Astro source repo's `public/images` master
  library (and the pilot blog-image source) by referenced basename.

## How to import (onto the new engine)

1. **Switch the active theme to `aqm-base` first.** The Ken Arnold staging site
   currently runs the legacy `kenarnold` theme, which redeclares the engine's
   `ka_*` helpers and **fatals**. Activating `aqm-base` stops that theme's
   `functions.php` from loading and clears the crash. (Install `aqm-base` via
   Appearance → Themes → Add New → Upload if it isn't there yet.)
2. **AutoForge → Import** → point at this repo (GitHub URL or local path). The
   importer will: sideload the images + logo into the media library, build all
   84 pages, seed `brand.json` into site config (resolving the logo to
   attachment IDs), and copy `main.css`/`site.js` into the `aqm-base` theme.
3. Verify in the browser.

## Verify on first import (known soft spots)

- **Mega-menu panels** (Services / Specialty / Areas) build from the `nav`
  panels + the `towns` list; confirm they populate (the pilot relied on header
  logic, not an explicit `megamenu` block).
- **`email`** in `brand.json` (`ken@kahomeinspectionsllc.com`) was a placeholder
  in the source config — confirm with the client.
- **Blog posts** (the 5 `type:"post"` records) render via the article template,
  not sections — confirm they land under `/blog/`.
