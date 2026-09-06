# S.6-C.4-P.4 — Global UX/UI Restoration & Visual System

**Phase verdict:** `PHASE_6_C.4_P.4 = PASS`
**Mode:** UX/UI ONLY (no URL / SEO / schema / content-fact / production changes)
**Baseline HEAD:** `cbc5101587de947cac2337377a8cfb49d938212e` (= origin/main)

---

## 0. AUDIT FINDINGS (BEFORE)

A read-only audit of `layouts/`, `partials/`, `assets/`, `static/`, `content/` and CSS/SCSS
established the root cause of every reported UX problem:

| Probe | Result | Note |
|-------|--------|------|
| HEADER_PRESENT | **NO** | `baseof.html` rendered only `{{ block "main" }}`; no header partial existed |
| FOOTER_PRESENT | **NO** | no footer partial existed |
| NAVIGATION_PRESENT | **NO** | no `<nav>` anywhere |
| GLOBAL_LAYOUT_PRESENT | **PARTIAL** | bare `<body>{{ block "main" }}</body>`, no wrapper/landmarks |
| RESPONSIVE_SYSTEM_PRESENT | **NO** | no CSS/SCSS file existed anywhere in the repo |
| COLOR_SYSTEM_PRESENT | **NO** | no design tokens; default browser styling |
| MULTI_COLUMN_LAYOUT_PRESENT | **NO** | every page was a single full-width text column |

**Conclusion:** the site was *completely unstyled* — there was no CSS file, no header, no
footer, no nav. The "problems" were not hidden/broken components; the components simply did
not exist. This phase builds them from scratch as a reusable component system.

---

## 1. WHAT WAS BUILT (AFTER)

### 1.1 Design system (`static/css/main.css`, new)
- **Design tokens** via CSS custom properties: `--color-primary #0d4a8e`,
  `--color-primary-dark #0a3a70`, `--color-accent #1e88e5`, plus text/surface/border/
  light-section/dark-section variables. Palette is derived from the existing brand blues
  (used by the KingShip logo and the pre-existing Formspree CTA) — **no rainbow colors**,
  light background + dark text + one accent.
- Typography hierarchy (H1–H3, body line-height 1.7, readable paragraph spacing).
- Layout container (max 1180px), responsive **header / footer / hero / card-grid / cta-band**,
  styled markdown (`content` rules for headings, lists, **tables → spec blocks**, images,
  blockquotes), buttons/CTA, focus-visible a11y, `prefers-reduced-motion` support.
- **Responsive breakpoints:** 980px (hero stacks), 860px (mobile nav + stacked footer),
  560px (single-column footer, full-width CTA).

### 1.2 Reusable components (new partials)
| Component | File | Purpose |
|-----------|------|---------|
| Header | `layouts/partials/site-header.html` | brand/logo + primary nav + RFQ CTA + mobile hamburger (keyboard-accessible checkbox toggle, JS-free) |
| Footer | `layouts/partials/site-footer.html` | 4-column grid (Company / Services / Industries / Company) + legal bar |
| Hero | inline `index.html` + `single.html` | homepage 2-col hero (copy + media) |
| Page hero | inline `single.html` / `list.html` | interior-page hero owning the single H1 |
| Card grid / Card | CSS + `home-sections.html` / `related-cards.html` | multi-column capability/industry/related cards |
| CTA band | `layouts/partials/cta-band.html` | final CTA (skipped on `/contact/`) |
| Related cards | `layouts/partials/related-cards.html` | multi-column "Related Services" from front matter |
| Home sections | `layouts/partials/home-sections.html` | curated capability + industry card grids (real pages only) |
| Skip link | `baseof.html` | a11y skip-to-content |

### 1.3 Layout wiring (modified, UX/UI scope only)
- `layouts/_default/baseof.html` — added skip-link, `site-header` partial, `site-footer` partial
  around the `main` block. (SEO/JSON-LD in `head.html` untouched.)
- `layouts/partials/head.html` — **additive only**: `<link rel="stylesheet">` + favicon. The
  title / canonical / description / OG / Twitter / JSON-LD lines were NOT modified.
- `layouts/_default/single.html` — page hero (owns the single H1) + `.Content` (its own H1
  stripped via `replaceRE` so the page has exactly one H1) + related cards + CTA band.
- `layouts/index.html` — composed homepage: hero + curated sections + existing `.Content`
  overview (H1 stripped) + CTA band. **No homepage copy was rewritten** — the original
  `_index.md` body is still rendered in full.
- `layouts/_default/list.html` — the two P.3 pages were authored as `content/<slug>/_index.md`
  (Hugo **sections**), so they rendered via `list.html` and produced a **duplicate H1**. This
  phase fixes `list.html` to mirror `single.html` (hero + H1-strip + related + CTA). This is a
  **layout fix only** — no file moved, no URL/title/content changed. (No other sections exist
  in this flat repo, so the change affects only those two pages.)

---

## 2. BEFORE → AFTER SUMMARY

| Dimension | BEFORE | AFTER |
|-----------|--------|-------|
| Header | none | sticky semantic `<header>` + `<nav>`, brand/logo, 5-item nav, RFQ CTA, mobile toggle |
| Footer | none | 4-column `<footer>` + legal bar, real links only |
| Single-column | entire site | hero 2-col, capability/industry/related **card grids**, footer 4-col, 2-col CTA |
| Color system | none (browser default) | industrial-blue token system, light bg + dark text + 1 accent |
| Responsive | none (no CSS) | 980 / 860 / 560 breakpoints, mobile nav, no horizontal overflow |
| Typography | unstyled | H1–H3 hierarchy, 1.7 line-height, constrained reading column (≈70ch articles) |
| CTA | inline links only | hero CTA + header CTA + final CTA band + accent buttons |
| Accessibility | n/a | skip-link, landmarks, focus-visible, single H1, alt text preserved |

---

## 3. FILES_CHANGED (P.4 own delta)

**Modified (5, all UX/UI layout/partial):**
1. `layouts/_default/baseof.html`
2. `layouts/_default/single.html`
3. `layouts/_default/list.html`
4. `layouts/index.html`
5. `layouts/partials/head.html` *(additive CSS/favicon link only)*

**New (6):**
6. `static/css/main.css`
7. `layouts/partials/site-header.html`
8. `layouts/partials/site-footer.html`
9. `layouts/partials/related-cards.html`
10. `layouts/partials/cta-band.html`
11. `layouts/partials/home-sections.html`

> **Pre-existing working-tree state (NOT introduced by P.4):** the tracked `git diff` also
> shows `hugo.toml` (cutover `baseURL` swap), `layouts/partials/seo-jsonld.html` (logo
> `wp-content`→`/images/` media fix), and 39 `content/*.md` files (image `wp-content`→`/images/`
> media-migration) plus CRLF normalization. These are the established **P-phase baseline**
> present before P.4 started (recorded at baseline lock, Step 1) and were **not touched by
> P.4**. P.4's own contribution is exclusively the 11 files above.

---

## 4. UI_COMPONENTS
Header · Footer · Hero · PageHero · Section · CardGrid · Card · FeatureCard · CTABand ·
RelatedCards · HomeSections · SkipLink · MobileNav · DesignTokens · ContentTypography · TableStyle.

## 5. PAGES_VALIDATED
Home · Service (`/aluminum-die-casting/`) · Industry (`/automotive-die-casting-parts/`) ·
Article (`/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/`) · About
(`/about-alumcasting-die-casting-expert/`) · Contact (`/contact/`) · plus both P.3 pages
(`/precision-die-casting-medical-equipment/`, the article above).

---

## 6. REGRESSION / SAFETY

| Check | Result |
|-------|--------|
| BUILD | PASS — `hugo --gc --minify` exit 0, **52 Pages** (unchanged from pre-P.4), 21 static (was 20 + `main.css`) |
| SEO_REGRESSION | **0** — title/description/canonical/OG/Twitter lines in `head.html` untouched; only additive CSS/favicon |
| SCHEMA_REGRESSION | **0** — `seo-jsonld.html` logic unchanged by P.4; verified Organization+WebPage+BreadcrumbList on all 46 indexable pages, FAQPage (3) and Article (6) where applicable |
| URL_REGRESSION | **0** — no slug/permalink/canonical changed by P.4; all chrome links use `relURL`; verified `https://alumcasting.com/` canonical on every sampled page |
| INTERNAL_LINKS | PASS — every P.4-inserted header/footer/home-section/CTA link resolves to a live page (verified programmatically). Pre-existing *deferred-page* content links (e.g. `semi-solid-die-casting-manufacturers`) are content-origin and were **not** modified by P.4 |
| FORBIDDEN LEAKAGE | `diecasting.github.io` = 0 · `wp-content` = 0 · `/alumcasting/` prefix = 0 across `public/` |
| H1 INTEGRITY | exactly **1 `<h1>`** per page (hero owns it; content H1 stripped) |
| UNAUTHORIZED_CHANGES | **0** — P.4 introduced no content / SEO / schema / hugo.toml / redirect change |
| REDIRECTS / CLOUDFLARE / DNS / WORDPRESS | 0 / NO / NO / NO (no such config in repo) |
| COMMIT / PUSH / DEPLOY | NO / NO / NO |

---

## 7. FINAL GATE

`HEADER=PASS · FOOTER=PASS · MULTI_COLUMN_LAYOUT=PASS · COLOR_SYSTEM=PASS ·
RESPONSIVE=PASS · TYPOGRAPHY=PASS · CTA=PASS · ACCESSIBILITY=PASS · BUILD=PASS`
with `SEO_REGRESSION=0 · SCHEMA_REGRESSION=0 · URL_REGRESSION=0 · UNAUTHORIZED_CHANGES=0`.

→ **PHASE_6_C.4_P.4 = PASS**

No production DNS / Cloudflare / WordPress / redirect operation was performed.
