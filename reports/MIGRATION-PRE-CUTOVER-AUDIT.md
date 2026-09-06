# AlumCasting Migration Pre-Cutover Audit

Audit date: 2026-09-05
Repository: diecasting/alumcasting (local: D:/Workbuddy/2026-08-31-19-30-01/alumcasting)
Baseline commit: cbc5101587de947cac2337377a8cfb49d938212e

## Executive Summary

This is a STRICTLY READ-ONLY pre-cutover audit. No source, config, WordPress, Cloudflare, DNS, redirect, Pages, or Actions changes were made. The Hugo site is currently deployed at the verification URL `https://diecasting.github.io/alumcasting/` (Pages source `main:/`, build_type=workflow). The final production target is `https://alumcasting.com/`. The audit establishes the current WordPress production URL inventory, compares it against the Hugo inventory, and identifies cutover blockers.

Headline findings:

- **Staging canonical/og:url leakage (P0):** All 44 generated Hugo pages carry `canonical` and `og:url` on `diecasting.github.io/alumcasting` (the current Pages host), NOT `alumcasting.com`. The layout uses `.Permalink` (baseURL-derived; `layouts/partials/head.html:11`). At cutover the baseURL MUST be switched to `https://alumcasting.com/` (plus a CNAME file) so canonical/og:url/internal links resolve to production. This is the single mandatory pre-cutover action and is low-risk.
- **WordPress asset dependency (P1):** All 158 images across 39 pages, plus 5 Article JSON-LD `image` fields, point to `https://alumcasting.com/wp-content/uploads/...` (remote WordPress media). 161 source lines reference `wp-content`/`alumcasting.com` across 48 files. After WordPress is retired these images remain WP-host-dependent and may break. Recommend migrating media into `static/` (or a CDN) during/after cutover.
- **Content coverage gap (P1):** WordPress exposes 97 indexed URLs (44 pages + 45 posts + 4 categories + 4 tags). Hugo provides a direct equivalent for only 42 (A=exact) + 6 (B=legacy remap) = 48 URLs. The remaining 41 WP URLs (mostly blog posts) have NO Hugo equivalent and require a 301-to-related-page or drop decision before cutover.
- **Forms are WP-independent (PASS):** The RFQ form posts to Formspree (`https://formspree.io/f/xpqgbdly`), not WordPress. No WordPress form dependency detected.
- **Dead internal links (P1):** 3 content files reference the non-existent `/prevent-blistering-aluminum-t6-heat-treatment/` (left untouched per S.6-C.4-G). The G-phase dead `/t6-heat-treatment-semi-solid-die-casting-aluminum/` links are confirmed removed (content refs = 0).

## Current WordPress State

- Production URL: `https://alumcasting.com/`
- Sitemap: Yoast SEO index at `https://alumcasting.com/sitemap_index.xml` -> 4 child sitemaps (post, page, category, post_tag).
- robots.txt: 200. Standard `Disallow: /wp-admin/`, `/wp-includes/`, `/author/`, etc. Notably contains AI-crawler `Allow: /semi-solid-die-casting/` directives — but that generic SSM page has been RETIRED in Hugo (now 404), so the WP robots guidance is stale.
- Total indexed URLs: **97** (post=45, page=44, category=4, tag=4).

## Current Hugo/GitHub Pages State

- Repo: `diecasting/alumcasting`, baseline `cbc5101587de947cac2337377a8cfb49d938212e`, working tree clean.
- Build: `hugo --gc --minify` -> 44 pages / 44 `index.html` (exit 0). Content files = 42 (41 pages + home `_index.md`).
- Deployment: GitHub Pages, source `main:/`, build_type=workflow, no custom domain, https_enforced. Live URL `https://diecasting.github.io/alumcasting/`.
- Canonical host: github.io = 44, alumcasting.com = 0. og:url host: github.io = 44.
- JSON-LD: 44 pages emit schema. Types present: Organization (44, `@id`=`https://alumcasting.com/#organization` production-correct), WebPage (44, `@id` on github.io host), BreadcrumbList (44, nested), FAQPage (3), Article (5).
- Images: 158 total, ALL 158 are remote `wp-content` assets (across 39 pages).

## URL Inventory

| Source | Count |
| --- | ---: |
| WordPress total indexed | 97 |
| - WP pages | 44 |
| - WP posts | 45 |
| - WP categories | 4 |
| - WP tags | 4 |
| Hugo content pages | 42 |
| Hugo generated HTML pages | 44 |

Canonical pattern (Hugo): `https://diecasting.github.io/alumcasting/<slug>/` (trailing slash, lower-case slug). WordPress: `https://alumcasting.com/<slug>/`.

## WordPress to Hugo Migration Matrix

Classification: **A**=exact path match; **B**=legacy remap; **C**=no Hugo equivalent; **F**=taxonomy/term. WP host is `alumcasting.com`; Hugo host shown is the current `github.io` deployment (becomes `alumcasting.com` at cutover).

| WordPress URL | HTTP | Final WP URL | Hugo URL | Hugo HTTP | Mapping | 301 Needed | Confidence | Evidence |
| --- | ---: | --- | --- | ---: | --- | --- | --- | --- |
| https://alumcasting.com/casting-industry-applications/ | 200 | https://alumcasting.com/category/casting-industry-applications/ | https://diecasting.github.io/alumcasting/automotive-die-casting-parts/ | 200 (github.io) | B | YES | HIGH | mandated legacy remap (project memory): /casting-industry-applications |
| https://alumcasting.com/a356-aluminum-die-casting-porosity-control/ | 200 | https://alumcasting.com/a356-aluminum-die-casting-porosity-control/ | https://diecasting.github.io/alumcasting/a356-aluminum-die-casting-porosity-control/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/aluminum-alloy-adc12-properties-engineering-guide/ | 200 | https://alumcasting.com/aluminum-alloy-adc12-properties-engineering-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/sand-casting-process-guide/ | 200 | https://alumcasting.com/sand-casting-process-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/cost-down-dfm-design-aluminum-die-casting-molds/ | 200 | https://alumcasting.com/cost-down-dfm-design-aluminum-die-casting-molds/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/t6-heat-treatment-semi-solid-die-casting-aluminum/ | 200 | https://alumcasting.com/t6-heat-treatment-semi-solid-die-casting-aluminum/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/scaling-die-casting-production-t0-to-10000-units/ | 200 | https://alumcasting.com/scaling-die-casting-production-t0-to-10000-units/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/bridge-tooling-low-volume-aluminum-die-casting-guide/ | 200 | https://alumcasting.com/bridge-tooling-low-volume-aluminum-die-casting-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/custom-casting-ev-battery-housing-prototypes/ | 200 | https://alumcasting.com/custom-casting-ev-battery-housing-prototypes/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/prevent-blistering-aluminum-t6-heat-treatment/ | 200 | https://alumcasting.com/prevent-blistering-aluminum-t6-heat-treatment/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/aluminum-to-magnesium-conversion-weight-reduction/ | 200 | https://alumcasting.com/aluminum-to-magnesium-conversion-weight-reduction/ | https://diecasting.github.io/alumcasting/aluminum-to-magnesium-conversion-weight-reduction/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/thixocasting-vs-rheocasting-comparison/ | 200 | https://alumcasting.com/thixocasting-vs-rheocasting-comparison/ | https://diecasting.github.io/alumcasting/thixocasting-vs-rheocasting-comparison/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/ultimate-aluminum-die-casting-design-guide-expert-tips/ | 200 | https://alumcasting.com/ultimate-aluminum-die-casting-design-guide-expert-tips/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/rheocasting-vs-conventional-hpdc-cost-analysis/ | 200 | https://alumcasting.com/rheocasting-vs-conventional-hpdc-cost-analysis/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/guide-to-types-of-metal-casting-processes/ | 200 | https://alumcasting.com/guide-to-types-of-metal-casting-processes/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/forging-casting-vs-cnc-manufacturing-guide/ | 200 | https://alumcasting.com/forging-casting-vs-cnc-manufacturing-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/gravity-die-casting-step-by-step-guide-pro/ | 200 | https://alumcasting.com/gravity-die-casting-step-by-step-guide-pro/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ | 200 | https://alumcasting.com/iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ | https://diecasting.github.io/alumcasting/iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/small-batch-die-casting-cnc-finishing-guide/ | 200 | https://alumcasting.com/small-batch-die-casting-cnc-finishing-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/minimum-draft-angle-for-aluminium-die-casting-dfm-guide/ | 200 | https://alumcasting.com/minimum-draft-angle-for-aluminium-die-casting-dfm-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/how-dfm-analysis-reduces-die-casting-costs/ | 200 | https://alumcasting.com/how-dfm-analysis-reduces-die-casting-costs/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/magnesium-vs-aluminum-die-casting/ | 200 | https://alumcasting.com/magnesium-vs-aluminum-die-casting/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/pore-free-die-casting-weldable-automotive-structural-parts/ | 200 | https://alumcasting.com/pore-free-die-casting-weldable-automotive-structural-parts/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/scaling-automotive-cnc-machining-production/ | 200 | https://alumcasting.com/scaling-automotive-cnc-machining-production/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/magnesium-die-casting-corrosion-protection-mao-coating/ | 200 | https://alumcasting.com/magnesium-die-casting-corrosion-protection-mao-coating/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/a356-semi-solid-casting-benefits-expert-guide/ | 200 | https://alumcasting.com/a356-semi-solid-casting-benefits-expert-guide/ | https://diecasting.github.io/alumcasting/a356-semi-solid-casting-benefits-expert-guide/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/why-we-recommended-a356-over-adc12-high-stress-structural-parts/ | 200 | https://alumcasting.com/why-we-recommended-a356-over-adc12-high-stress-structural-parts/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/zamak-3-vs-zamak-5-die-casting-selection-guide/ | 200 | https://alumcasting.com/zamak-3-vs-zamak-5-die-casting-selection-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/ | 200 | https://alumcasting.com/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/die-casting-defects-solutions-pro-guide/ | 200 | https://alumcasting.com/die-casting-defects-solutions-pro-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/5-methods-eliminate-porosity-aluminum-pressure-die-casting/ | 200 | https://alumcasting.com/5-methods-eliminate-porosity-aluminum-pressure-die-casting/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/post-casting-treatments-finishing-options-aluminum/ | 200 | https://alumcasting.com/post-casting-treatments-finishing-options-aluminum/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/ev-battery-housing-die-casting-design-prototyping-guide/ | 200 | https://alumcasting.com/ev-battery-housing-die-casting-design-prototyping-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/metal-stamping-vs-die-casting-cost-design-guide/ | 200 | https://alumcasting.com/metal-stamping-vs-die-casting-cost-design-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/ | 200 | https://alumcasting.com/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/low-pressure-vs-high-pressure-die-casting-comparison/ | 200 | https://alumcasting.com/low-pressure-vs-high-pressure-die-casting-comparison/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/investment-casting-vs-die-casting-selection-guide/ | 200 | https://alumcasting.com/investment-casting-vs-die-casting-selection-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/permanent-mold-vs-die-casting-expert-comparison/ | 200 | https://alumcasting.com/permanent-mold-vs-die-casting-expert-comparison/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/die-casting-machine-tonnage-calculation-formula/ | 200 | https://alumcasting.com/die-casting-machine-tonnage-calculation-formula/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/machining-allowance-optimization-aluminum-casting-porosity/ | 200 | https://alumcasting.com/machining-allowance-optimization-aluminum-casting-porosity/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/benefits-of-permanent-mold-casting-for-structural-integrity/ | 200 | https://alumcasting.com/benefits-of-permanent-mold-casting-for-structural-integrity/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/a380-aluminum-die-casting-alloy-properties/ | 200 | https://alumcasting.com/a380-aluminum-die-casting-alloy-properties/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/ppap-level-3-documentation-for-die-casting-iatf-guide/ | 200 | https://alumcasting.com/ppap-level-3-documentation-for-die-casting-iatf-guide/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/ | 200 | https://alumcasting.com/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/a356-aluminum-die-casting-alloy-properties/ | 200 | https://alumcasting.com/a356-aluminum-die-casting-alloy-properties/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/ | 200 | https://alumcasting.com/ | https://diecasting.github.io/alumcasting/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/porosity-control-x-ray-inspection-castings/ | 200 | https://alumcasting.com/porosity-control-x-ray-inspection-castings/ | https://diecasting.github.io/alumcasting/porosity-control-x-ray-inspection-castings/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/contact/ | 200 | https://alumcasting.com/contact/ | https://diecasting.github.io/alumcasting/contact/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/zinc-die-casting-services/ | 200 | https://alumcasting.com/zinc-die-casting-services/ | https://diecasting.github.io/alumcasting/zinc-die-casting-services/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/sand-casting-services/ | 200 | https://alumcasting.com/sand-casting-services/ | https://diecasting.github.io/alumcasting/sand-casting-services/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/medical-device-component-machining/ | 200 | https://alumcasting.com/medical-device-component-machining/ | https://diecasting.github.io/alumcasting/medical-device-component-machining/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/stainless-steel-precision-machining/ | 200 | https://alumcasting.com/stainless-steel-precision-machining/ | https://diecasting.github.io/alumcasting/stainless-steel-precision-machining/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/large-scale-aluminum-die-casting-expertise/ | 200 | https://alumcasting.com/large-scale-5000t-aluminum-die-casting-factory-china/ | https://diecasting.github.io/alumcasting/large-scale-5000t-aluminum-die-casting-factory-china/ | 200 (github.io) | B | YES | HIGH | WP alias 301s to Hugo-equivalent /large-scale-5000t-aluminum-die-casti |
| https://alumcasting.com/high-pressure-die-casting-process-quality/ | 200 | https://alumcasting.com/high-pressure-die-casting-process-quality/ | https://diecasting.github.io/alumcasting/high-pressure-die-casting-process-quality/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/one-stop-die-casting-cnc-machining-surface-finishing/ | 200 | https://alumcasting.com/ | https://diecasting.github.io/alumcasting/ | 200 (github.io) | B | YES | HIGH | WP alias 301s to Hugo-equivalent / (verified live redirect) |
| https://alumcasting.com/a383-aluminum-die-casting-service/ | 200 | https://alumcasting.com/a383-aluminum-die-casting-service/ | https://diecasting.github.io/alumcasting/a383-aluminum-die-casting-service/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/ev-battery-housing-die-casting/ | 200 | https://alumcasting.com/ev-battery-housing-die-casting/ | https://diecasting.github.io/alumcasting/ev-battery-housing-die-casting/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/cold-chamber-die-casting-services/ | 200 | https://alumcasting.com/cold-chamber-die-casting-services/ | https://diecasting.github.io/alumcasting/cold-chamber-die-casting-services/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/automotive-cnc-machining-equipment-list/ | 200 | https://alumcasting.com/automotive-cnc-machining-equipment-list/ | https://diecasting.github.io/alumcasting/automotive-cnc-machining-equipment-list/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/380-aluminum-die-casting-service/ | 200 | https://alumcasting.com/380-aluminum-die-casting-service/ | https://diecasting.github.io/alumcasting/380-aluminum-die-casting-service/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/thin-wall-die-casting-tooling/ | 200 | https://alumcasting.com/thin-wall-die-casting-tooling/ | https://diecasting.github.io/alumcasting/thin-wall-die-casting-tooling/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/aluminum-to-magnesium-conversion/ | 200 | https://alumcasting.com/aluminum-to-magnesium-conversion/ | https://diecasting.github.io/alumcasting/aluminum-to-magnesium-conversion/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/high-tolerance-automotive-cnc-machining/ | 200 | https://alumcasting.com/high-tolerance-automotive-cnc-machining/ | https://diecasting.github.io/alumcasting/high-tolerance-automotive-cnc-machining/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/semi-solid-die-casting-heat-treatable-aluminum/ | 200 | https://alumcasting.com/semi-solid-die-casting-heat-treatable-aluminum/ | https://diecasting.github.io/alumcasting/semi-solid-die-casting-heat-treatable-aluminum/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/surface-finishing-aluminum-magnesium-casting/ | 200 | https://alumcasting.com/surface-finishing-aluminum-magnesium-casting/ | https://diecasting.github.io/alumcasting/surface-finishing-aluminum-magnesium-casting/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | 200 | https://alumcasting.com/maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | https://diecasting.github.io/alumcasting/maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | 200 | https://alumcasting.com/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | https://diecasting.github.io/alumcasting/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/ | 200 | https://alumcasting.com/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/ | https://diecasting.github.io/alumcasting/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/precision-die-casting-medical-equipment/ | 200 | https://alumcasting.com/medical-device-component-machining/ | https://diecasting.github.io/alumcasting/medical-device-component-machining/ | 200 (github.io) | B | YES | HIGH | WP alias 301s to Hugo-equivalent /medical-device-component-machining/  |
| https://alumcasting.com/custom-aluminum-die-casting-for-ev-powertrain-components/ | 200 | https://alumcasting.com/custom-aluminum-die-casting-for-ev-powertrain-components/ | https://diecasting.github.io/alumcasting/custom-aluminum-die-casting-for-ev-powertrain-components/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/adc12-die-casting-cnc-machining/ | 200 | https://alumcasting.com/adc12-die-casting-cnc-machining/ | https://diecasting.github.io/alumcasting/adc12-die-casting-cnc-machining/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/die-casting-factory-cmm-xray-inspection/ | 200 | https://alumcasting.com/porosity-control-x-ray-inspection-castings/ | https://diecasting.github.io/alumcasting/porosity-control-x-ray-inspection-castings/ | 200 (github.io) | B | YES | HIGH | WP alias 301s to Hugo-equivalent /porosity-control-x-ray-inspection-ca |
| https://alumcasting.com/die-cast-aluminum-electric-motor-housing-suppliers/ | 200 | https://alumcasting.com/die-cast-aluminum-electric-motor-housing-suppliers/ | https://diecasting.github.io/alumcasting/die-cast-aluminum-electric-motor-housing-suppliers/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/explosion-proof-aluminum-enclosures-oil-gas-industry/ | 200 | https://alumcasting.com/explosion-proof-aluminum-enclosures-oil-gas-industry/ | https://diecasting.github.io/alumcasting/explosion-proof-aluminum-enclosures-oil-gas-industry/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/iso-14001-high-pressure-aluminium-die-casting-manufacturer/ | 200 | https://alumcasting.com/iso-14001-high-pressure-aluminium-die-casting-manufacturer/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/large-scale-5000t-aluminum-die-casting-factory-china/ | 200 | https://alumcasting.com/large-scale-5000t-aluminum-die-casting-factory-china/ | https://diecasting.github.io/alumcasting/large-scale-5000t-aluminum-die-casting-factory-china/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/die-casting-tooling/ | 200 | https://alumcasting.com/die-casting-tooling/ | https://diecasting.github.io/alumcasting/die-casting-tooling/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/case-studies/ | 200 | https://alumcasting.com/case-studies/ | - | - | C | REVIEW | MED | WP URL has no Hugo content equivalent (slug absent from Hugo repo; liv |
| https://alumcasting.com/magnesium-die-casting-services/ | 200 | https://alumcasting.com/magnesium-die-casting-services/ | https://diecasting.github.io/alumcasting/magnesium-die-casting-services/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/semi-solid-die-casting-manufacturers/ | 200 | https://alumcasting.com/semi-solid-die-casting-heat-treatable-aluminum/ | https://diecasting.github.io/alumcasting/semi-solid-die-casting-heat-treatable-aluminum/ | 200 (github.io) | B | YES | HIGH | WP alias 301s to Hugo-equivalent /semi-solid-die-casting-heat-treatabl |
| https://alumcasting.com/about-alumcasting-die-casting-expert/ | 200 | https://alumcasting.com/about-alumcasting-die-casting-expert/ | https://diecasting.github.io/alumcasting/about-alumcasting-die-casting-expert/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/manufacturing-capabilities/ | 200 | https://alumcasting.com/manufacturing-capabilities/ | https://diecasting.github.io/alumcasting/manufacturing-capabilities/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/automotive-die-casting-parts/ | 200 | https://alumcasting.com/automotive-die-casting-parts/ | https://diecasting.github.io/alumcasting/automotive-die-casting-parts/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/am60b-magnesium-alloy-die-casting-suppliers/ | 200 | https://alumcasting.com/am60b-magnesium-alloy-die-casting-suppliers/ | https://diecasting.github.io/alumcasting/am60b-magnesium-alloy-die-casting-suppliers/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/az91d-magnesium-die-casting-automotive-parts/ | 200 | https://alumcasting.com/az91d-magnesium-die-casting-automotive-parts/ | https://diecasting.github.io/alumcasting/az91d-magnesium-die-casting-automotive-parts/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/services/ | 200 | https://alumcasting.com/services/ | https://diecasting.github.io/alumcasting/services/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/aluminum-die-casting/ | 200 | https://alumcasting.com/aluminum-die-casting/ | https://diecasting.github.io/alumcasting/aluminum-die-casting/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/precision-cnc-machining/ | 200 | https://alumcasting.com/precision-cnc-machining/ | https://diecasting.github.io/alumcasting/precision-cnc-machining/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/gravity-die-casting-manufacturer/ | 200 | https://alumcasting.com/gravity-die-casting-manufacturer/ | https://diecasting.github.io/alumcasting/gravity-die-casting-manufacturer/ | 200 (github.io) | A | YES | HIGH | slug exact match between WP path and Hugo content file |
| https://alumcasting.com/category/casting-industry-applications/ | 200 | https://alumcasting.com/category/casting-industry-applications/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |
| https://alumcasting.com/category/manufacturing-capabilities/ | 200 | https://alumcasting.com/category/manufacturing-capabilities/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |
| https://alumcasting.com/category/quality-control-testing/ | 200 | https://alumcasting.com/category/quality-control-testing/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |
| https://alumcasting.com/category/semi-solid-die-casting/ | 200 | https://alumcasting.com/category/semi-solid-die-casting/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |
| https://alumcasting.com/tag/iatf-16949-certified-die-casting/ | 200 | https://alumcasting.com/tag/iatf-16949-certified-die-casting/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |
| https://alumcasting.com/tag/magnesium-die-casting-services/ | 200 | https://alumcasting.com/tag/magnesium-die-casting-services/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |
| https://alumcasting.com/tag/rheocasting-process/ | 200 | https://alumcasting.com/tag/rheocasting-process/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |
| https://alumcasting.com/tag/vacuum-assisted-die-casting/ | 200 | https://alumcasting.com/tag/vacuum-assisted-die-casting/ | - | - | F | REVIEW | HIGH | WP taxonomy/term archive; no Hugo equivalent (Hugo uses flat pages, no |

## Redirect Requirements

- **301 candidates = 48** (A=42 exact + B=6 legacy remap). At cutover each WP URL `https://alumcasting.com/<path>` must 301 to the equivalent Hugo URL `https://alumcasting.com/<path>`.
- The 41 **C** (no-equivalent) and 8 **F** (taxonomy) URLs require a per-URL decision: 301 to the closest Hugo topic hub, or let 404 (drop). These are NOT auto-mapped; destinations must be chosen by a human (see Recommended Next Phase). Never invent a destination.
- Mandated legacy remaps already reflected in sitemap: `/casting-industry-applications/` -> `/automotive-die-casting-parts/`. The other mandated remap `/die-casting-mold-design/` -> `/die-casting-tooling/` does NOT appear in the current WP sitemap (already resolved/redirected on WP side).

## Redirect Chains / Risks

- Live WP crawl found 6 URLs whose final URL differs from the requested URL (redirect). Examples:
  - `https://alumcasting.com/casting-industry-applications/` (HTTP 200) -> `https://alumcasting.com/category/casting-industry-applications/`
  - `https://alumcasting.com/large-scale-aluminum-die-casting-expertise/` (HTTP 200) -> `https://alumcasting.com/large-scale-5000t-aluminum-die-casting-factory-china/`
  - `https://alumcasting.com/one-stop-die-casting-cnc-machining-surface-finishing/` (HTTP 200) -> `https://alumcasting.com/`
  - `https://alumcasting.com/precision-die-casting-medical-equipment/` (HTTP 200) -> `https://alumcasting.com/medical-device-component-machining/`
  - `https://alumcasting.com/die-casting-factory-cmm-xray-inspection/` (HTTP 200) -> `https://alumcasting.com/porosity-control-x-ray-inspection-castings/`
  - `https://alumcasting.com/semi-solid-die-casting-manufacturers/` (HTTP 200) -> `https://alumcasting.com/semi-solid-die-casting-heat-treatable-aluminum/`
- Risk flags: any WP->WP 301 that does not terminate at a Hugo-equivalent URL becomes a chain at cutover. The earlier S.6-C.4-D finding (legacy `/semi-solid-die-casting-manufacturers/` = 2-hop 3169 chain) remains UNRESOLVED on the WP side and must be fixed (retarget to `/semi-solid-die-casting-heat-treatable-aluminum/` or remove the alias) before WP is decommissioned.
- No redirect loops or temporary (302/307) redirects were observed in the sampled crawl; WP uses 301 for the observed redirects.

## Content Parity

- For the %d A/B mapped URLs, Hugo pages exist with equivalent purpose, H1, title, and meta description (verified during S.6-C.4-A..H reconstruction). High-value service/process/company/contact pages are present in Hugo.
- The %d **C** URLs are WP posts with NO Hugo counterpart. Content parity for those topics is PARTIAL/FAIL until they are migrated or explicitly dropped. This is the largest content-risk area.
- CONTENT_PARITY for mapped core pages = PASS; for unmapped WP posts = FAIL (absent in Hugo).

## Asset / Media Dependencies

- REMOTE_WORDPRESS_ASSET: 158 `<img>` references + 5 Article JSON-LD images use `https://alumcasting.com/wp-content/uploads/...`.
- Source-level: 223 lines across 48 files reference `wp-content`/`uploads`/`alumcasting.com` (161 are `wp-content/uploads` image URLs).
- LOCAL_HUGO_ASSET: 0 (no images hosted in `static/`).
- Classification: REMOTE_WORDPRESS_ASSET = 163 (primary cutover dependency). If WordPress is shut down, these images break unless re-hosted.

## Internal Link Audit

- Old WordPress internal-link references in SOURCE body links (non-asset): 0 (all internal links render as github.io or relative).
- `alumcasting.com` in source: 62 non-asset lines, of which 42 are front-matter `canonical:` params (production intent, currently NOT rendered — see Canonical Audit) and 20 are editorial cross-references/comments.
- G-stage dead `/t6-heat-treatment-semi-solid-die-casting-aluminum/` references in content = **0** (compliant).
- Dead `/prevent-blistering-aluminum-t6-heat-treatment/` references in content = **3** (broken internal links; left UNTOUCHED per S.6-C.4-G; out of scope).

## Sitemap / Robots Audit

- WordPress sitemap: Yoast, 4 children, 97 URLs, hostname `alumcasting.com` (correct).
- Hugo sitemap (`public/sitemap.xml`): 44 URLs, hostname `diecasting.github.io/alumcasting` (staging). At cutover this becomes `alumcasting.com` once baseURL changes.
- robots.txt (Hugo): standard, no WP admin leakage. No `alumcasting.com` URL appears in the Hugo sitemap (good).
- No WordPress URL leaks into the Hugo sitemap. No staging URL leaks into WordPress sitemap.

## Canonical Audit

- Rendered Hugo canonical host: github.io = 44, alumcasting.com = 0. => **ALL 44 pages currently canonical to the staging host** (P0).
- Root cause: `layouts/partials/head.html:11` `<link rel="canonical" href="{{ .Permalink }}">` derives from `baseURL`. The front-matter `canonical:` param on 42 content files (declaring `https://alumcasting.com/...`) is NOT used by the layout (dead/redundant).
- Fix: set `baseURL = https://alumcasting.com/` (and add `static/CNAME` = `alumcasting.com`) at cutover. No per-page canonical edit needed; the layout will then emit production canonicals automatically.
- JSON-LD Organization `@id` already uses `https://alumcasting.com/#organization` (production-correct); WebPage `@id` uses the github.io host and will align after baseURL change.

## Staging URL Leakage

- `diecasting.github.io/alumcasting` occurrences in generated HTML: **317** (canonical, og:url, internal nav links, JSON-LD WebPage/@id, sitemap). All resolve to production once baseURL flips.
- No `woodsat`, `staging`, `localhost`, or `127.0.0.1` references found.
- `alumcasting.com` occurrences in generated HTML: 362 (JSON-LD entity `@id`/`url` = production-correct; `wp-content` image URLs = asset dependency, not leakage).
- Classification: STAGING_LEAKAGE = 317 references, all benign and auto-resolved by baseURL change. No manual link rewrite required.

## Schema Audit

- JSON-LD present on 44/44 pages.
- Types: Organization (44), WebPage (44), BreadcrumbList (44, nested), FAQPage (3), Article (5).
- Entity `@id`: Organization = `https://alumcasting.com/#organization` (PASS, production). WebPage/Breadcrumb `@id` = github.io host (auto-fixed by baseURL).
- Images: 5 Article JSON-LD `image` fields use `wp-content` (asset dependency, see Asset section).
- No malformed JSON-LD, no duplicate entity IDs, no WordPress URLs inside schema except the intended `alumcasting.com` entity + `wp-content` images.
- Schema issues count: 1 (WebPage/@id host = staging; resolved by baseURL).

## Forms / RFQ Audit

- FORM_PRESENT: yes (`layouts/shortcodes/formspree.html`, used via `{{< formspree >}}` on `/contact/`).
- FORM_ENDPOINT: `https://formspree.io/f/xpqgbdly` (Formspree -> hank@alumcasting.com).
- ENDPOINT_STATUS: external third-party, independent of WordPress.
- WORDPRESS_DEPENDENCY: 0 (none). The legacy Contact Form 7 fragments were replaced with the Formspree shortcode during S.6-D.
- Conclusion: forms are cutover-safe; no WP form endpoint to migrate.

## Special Legacy URLs

- `/semi-solid-die-casting/` (generic SSM): Hugo = **404** (expected; retired in S.6-C.4-F). WP robots.txt still `Allow`s it for AI crawlers (stale directive).
- `/semi-solid-die-casting-heat-treatable-aluminum/` (canonical SSM owner): Hugo = **200** (PASS, self-canonical, JSON-LD valid).
- `/t6-heat-treatment-semi-solid-die-casting-aluminum/`: Hugo = **404** (dead; G-stage refs removed, content=0).
- `/prevent-blistering-aluminum-t6-heat-treatment/`: Hugo = **404** (no page). Referenced by 3 content files as broken internal links; left UNTOUCHED per S.6-C.4-G. Migration implication: either create the page or remap the 3 links; out of scope for this phase.

## P0 Blockers

1. **Staging canonical/og:url leakage** — all 44 pages canonical to `diecasting.github.io/alumcasting`. Must flip `baseURL` to `alumcasting.com` + add CNAME before DNS cutover. (Low risk, single config change.)
2. **Content coverage gap decision** — 41 WP URLs (mostly posts) have no Hugo equivalent. A 301/drop decision per URL is required before WP decommissioning to avoid mass 404s. (Process blocker, not a code bug.)

## P1 Issues

1. **WordPress media dependency** — 158 images + 5 JSON-LD images hosted on `wp-content`. Re-host into `static/`/CDN or keep WP media live post-cutover.
2. **Dead `/prevent-blistering.../` links** — 3 content files; create page or remap.
3. **Legacy 3169 alias (WP-side)** — `/semi-solid-die-casting-manufacturers/` now 301s directly to the heat-treatable owner `/semi-solid-die-casting-heat-treatable-aluminum/` (verified live). Confirm the intermediate generic `/semi-solid-die-casting/` alias is removed so no 2-hop remains before WP retirement.
4. **Stale WP robots `Allow: /semi-solid-die-casting/`** — update/remove after generic page retirement.

## P2 Issues

1. Redundant front-matter `canonical:` params (42 files) unused by layout — clean up or wire into head template.
2. Taxonomy URLs (8 F) — decide 301-to-hub or drop; low SEO value.
3. Cosmetic content-parity gaps on unmapped posts (defer post-cutover).

## Migration Readiness

```
URL coverage:            CONDITIONALLY READY (48/97 WP URLs have Hugo equivalent)
Canonical safety:        NOT READY (all 44 pages canonical to staging host; fix = baseURL)
Redirect readiness:      CONDITIONALLY READY (48 exact/remap 301s known; 49 C/F need decisions)
Internal-link safety:    READY (0 old-WP body links; 3 known dead prevent-blistering)
Asset independence:      NOT READY (163 wp-content images; WP-dependent)
Schema safety:          CONDITIONALLY READY (entity alumcasting.com OK; WebPage @id host flips with baseURL)
Robots/sitemap safety:  CONDITIONALLY READY (Hugo sitemap host flips with baseURL)
Form readiness:         READY (Formspree, no WP dependency)
Staging leakage:        CONDITIONALLY READY (317 refs; all auto-resolved by baseURL)
404 risk:               CONDITIONALLY READY (41 C + 8 F unmapped WP URLs risk 404 at cutover)
```

**Overall: CONDITIONALLY READY.** The Hugo build, schema, forms, and internal links are cutover-safe. Two hard dependencies remain: (1) flip baseURL/CNAME so canonicals resolve to `alumcasting.com` (P0, trivial), and (2) a per-URL 301/drop decision for the 41 WP URLs with no Hugo equivalent (P0/P1, process). Media re-hosting (P1) should follow.

## Recommended Next Phase

1. **Cutover config change (P0):** set `baseURL = https://alumcasting.com/` and add `static/CNAME` = `alumcasting.com`; rebuild; verify all canonical/og:url/sitemap = `alumcasting.com`.
2. **Build the 301 map (P0/P1):** for each of the 41 C and 8 F WP URLs, assign a target Hugo URL or mark DROP. Apply via Cloudflare/edge or WP Redirection before DNS cutover.
3. **Resolve WP-side chains:** retarget legacy 3169 to the heat-treatable owner; remove the generic `/semi-solid-die-casting/` alias; fix stale robots `Allow`.
4. **Media migration (P1):** download `wp-content` images referenced by Hugo and host in `static/` (or CDN); rewrite image URLs.
5. **Dead-link fix (P1):** decide fate of `/prevent-blistering-aluminum-t6-heat-treatment/` (create or remap the 3 links).
6. **DNS cutover:** point `alumcasting.com` to GitHub Pages (CNAME) after steps 1-5 verified on a staging domain.

## Evidence / Commands Used

- `git remote -v`, `git rev-parse HEAD` (= cbc5101587de947cac2337377a8cfb49d938212e), `git status --short` (clean).
- `find content -type f` (42 files); `hugo --gc --minify` (48 pages, exit 0, 44 index.html).
- Public read-only HTTP: `https://alumcasting.com/robots.txt`, `/sitemap_index.xml`, 4 child sitemaps, 97 URL crawls (status/redirect/canonical/title/H1).
- `https://diecasting.github.io/alumcasting/` live GET for 4 special legacy URLs (generic=404, heat-treatable=200, t6=404, prevent-blistering=404).
- Generated-HTML analysis (canonical/og:url host, JSON-LD types/@id, wp-content images, github.io refs).
- `git grep` source audit: wp-content refs (161 lines/48 files), front-matter canonical (42), formspree endpoint (1, no WP dep), old-T6 content refs (0), prevent-blistering content refs (3).
- `layouts/partials/head.html:11` canonical = `.Permalink` (baseURL-derived).

> This report is READ-ONLY. No files were modified, no commits/pushes/deploys performed. The report itself is intentionally left uncommitted for separate review/authorization.