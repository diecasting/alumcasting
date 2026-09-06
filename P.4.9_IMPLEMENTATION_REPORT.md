# P.4.9 — ALUMCASTING WORDPRESS → HUGO TRUE UX/UI RECONSTRUCTION
## IMPLEMENTATION REPORT

**Date:** 2026-09-06
**Scope:** Homepage (`content/_index.md` front matter + `layouts/index.html` + `layouts/partials/home-sections.html` + `static/css/main.css`)
**Source of truth:** `https://alumcasting.com/` (read-only)
**Target:** `https://diecasting.github.io/alumcasting/` (local build, `baseURL: /`, root-relative)
**Git HEAD before change:** `068f154ee6432088e2e8ab21d420dab9471f732c`

---

### FILES_CHANGED
| File | Change | P-items |
|------|--------|---------|
| `layouts/index.html` | Removed rogue `{{ .Content }}` block (P0-A); rebuilt hero as full-width cover band (`hero-band` / `hero-bg` / `hero-overlay` / `hero-content`, NOT left-text/right-image split) (P0-B) | P0-A, P0-B |
| `layouts/partials/home-sections.html` | 13 `<section class="... home-band">` bands with inner `.container`; exact desktop column counts via `grid-2/3/4/5`; `.img-card` for applications/cases; `.section-cta` per band to real routes (P0-C/D/E/F) | P0-C, P0-D, P0-E, P0-F |
| `static/css/main.css` | Hero cover-band system; grid utilities; `.img-card` / `.section-cta`; `.home-band` border-top for band rhythm; `.blog-grid` forced 3-col; `.section-title` scale; responsive 860px/560px collapses (P0-B/P1/P2) | P0-B, P1, P2 |

Working tree diff scope verified: **only these 3 files** (196 insertions, 108 deletions). No other files touched.

---

### P0 STATUS — LAYOUT RECONSTRUCTION
**PASS**
- A. Rogue `{{ .Content }}` homepage block removed; `_index.md` front matter retained for SEO/H1/description; body preserved for future `/overview/`.
- B. Hero rebuilt as full-width cover band (bg image + dark overlay gradient + centered container + H1 + lede + CTAs + trust badges), matching WP `alum-hero` model (was a left-text/right-image split in P.4.8).
- C. Section band system restored: every homepage section is `<section class="... home-band">` with inner centered `.container`; distinct visual boundaries via `.home-band + .home-band { border-top }`.
- D. Multi-column system per WP model: services=4, why=3, applications=3 (image cards), process=5, material=4, quality=3, certification=2-col (image/text split), advanced=2-col (image/text split), cases=3, knowledge=3.
- E. Image + card system reuses `static/images/` real assets (no fabricated images).
- F. CTA density restored: hero CTAs, per-section `.section-cta`, mid-page engineering-review RFQ band, final full-width CTA band — all to real, published routes.

### P1 STATUS — VISUAL FIDELITY
**PASS**
- Typography: H1 `clamp(2rem,4vw,2.7rem)` (~34px), H2 `.section-title` `clamp(1.5rem,2.6vw,1.9rem)` (~29px), body `17px`; hierarchy matches WP.
- Container: `var(--container)` = 1200px.
- Section spacing: `.section { padding-block: var(--space-section) }`; band rhythm via border-top.
- Cards: image cards 16/9 crop, white bg, border + radius + `var(--shadow-sm)`, hover lift; service/material cards with accent top-border.
- Final CTA: full-width navy-gradient band (`cta-band.html`, unchanged, satisfies spec).

### P2 STATUS — MICRO REFINEMENT
**PASS**
- Shadows (`--shadow-sm` / `--shadow-md`), borders (`--color-border`), radius (`--radius-md`), image crop (`aspect-ratio`), card/button alignment (`margin-top:auto` link), desktop→mobile transitions (5 media queries; grids 4→2→1, hero padding clamps).

---

### H1 STATUS
**PASS** — Exactly 1 `<h1>` on homepage; text = `"Aluminum Die Casting Supplier | HPDC & CNC Machining"` (preserved from `_index.md` front matter `.Title`).

### SEO_SCHEMA STATUS
**PASS / UNAFFECTED**
- Canonical: `https://alumcasting.com/` (correct, unchanged).
- JSON-LD: 2 blocks — (1) `Organization` with `#organization` @id; (2) `WebPage` + nested `BreadcrumbList` + `ListItem`. Intact, no SEO/schema logic touched.
- `robots` / `sitemap.xml` present and unchanged after build.
- `_index.md` front matter unchanged; no SEO-affecting markup added.

### RESPONSIVE STATUS
**PASS** — 5 `@media` queries in built CSS; grids collapse `4→2→1` at 860px / 560px; hero padding clamps; nav/header handled by prior phases.

### BROKEN_IMAGES
**0** — 13 image `src` on homepage, all resolve. (3 were initially broken: applications `img-card` filenames lacked the `images/` prefix → `/file.webp` 404; fixed by adding `images/` prefix. Re-verified 0 broken.)

### BROKEN_LINKS
**0** — 20 unique internal page routes on homepage, all resolve to built `public/<slug>/index.html`. No fabricated URLs; every CTA/card targets a published Hugo page.

### ROGUE_HOME_CONTENT
**0** — `home-overview` class absent from built `public/index.html`; none of the distinctive `_index.md` body phrases ("Manufacturing at a Glance", "40,000", "Dongguan Production Base", "Prototype To Mass Production", "Get Engineering Quote", etc.) appear. Homepage no longer renders the linear `_index.md` body.

### LAYOUT_PARITY
**PASS** — 15 `<section>` bands built (hero + 13 content bands + final CTA band), matching the WP multi-band model. Full-width bands + centered container + exact desktop column counts reproduced.

### VISUAL_TREATMENT
**PASS (structural / style-level)** — Cover hero (bg image + overlay + centered), distinct band boundaries, card shadows/borders/radius, image 16/9 crop, navy "trust" + surface "metrics" bands, accent CTAs.
> **Caveat:** `VISUAL_PIXEL_DIFF = NOT_AVAILABLE`. No screenshot / pixel-diff tooling was run against the live `alumcasting.com`. This is a faithful structural + CSS reconstruction of the WP visual system, not a pixel-verified 1:1 copy. Some section copy was refreshed to match the current published page inventory; the band/column/card/CTA *system* is the restored source of truth.

### CTA_PARITY
**PASS** — Hero CTAs (Request a Quote → `/contact/`, Explore Services → `/services/`); per-section CTAs to real routes (`/manufacturing-capabilities/`, `/about-alumcasting-die-casting-expert/`, `/automotive-die-casting-parts/`, `/die-casting-tooling/`, `/semi-solid-die-casting-heat-treatable-aluminum/`, `/porosity-control-x-ray-inspection-castings/`, `/contact/`, `/blog/`); mid-page engineering-review RFQ band; final CTA band.

### IMAGE_PARITY
**PASS** — All homepage images reuse real `static/images/` assets (no deleted/placeholder images): hero 5000T machine; applications (magnesium automotive / 5000T / lightweight magnesium); cert (TF1949 ISO9001); advanced (CNC workshop); cases/knowledge reuse real webp + `Params.image` fallbacks.

### HUGO_BUILD
**PASS** — `hugo --cleanDestinationDir --minify` exit 0; 54 pages, 22 static files.

### GIT_STATUS
- 3 files modified, **uncommitted**.
- **COMMIT = NOT PERFORMED**
- **PUSH = NOT PERFORMED**
- **HARD STOP** — awaiting user authorization to commit/push/deploy.

---

### MANDATORY QA — PER-AXIS SUMMARY
| Axis | Result |
|------|--------|
| HTML_STRUCTURE | PASS |
| CONTENT_PARITY | PASS |
| LAYOUT_PARITY | PASS |
| VISUAL_TREATMENT | PASS (pixel diff NOT_AVAILABLE) |
| RESPONSIVE_LAYOUT | PASS |
| CTA_PARITY | PASS |
| IMAGE_PARITY | PASS |
| H1 | PASS |
| CANONICAL | PASS |
| JSON-LD | PASS |
| ROGUE_HOME_CONTENT | 0 (PASS) |
| BROKEN_IMAGES | 0 (PASS) |
| BROKEN_LINKS | 0 (PASS) |
| HUGO_BUILD | PASS |

**VISUAL_PIXEL_DIFF = NOT_AVAILABLE**
**ROGUE_HOME_CONTENT = 0**
**COMMIT = NOT PERFORMED**
**PUSH = NOT PERFORMED**
