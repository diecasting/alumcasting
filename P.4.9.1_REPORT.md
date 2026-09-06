# P.4.9.1 — AlumCasting NAVIGATION + ROUTING RECOVERY — IMPLEMENTATION REPORT

**Date:** 2026-09-06
**Target:** https://diecasting.github.io/alumcasting/  (GitHub Pages project subpath)
**Source reference:** https://alumcasting.com/  (read-only WordPress audit)
**Scope:** Navigation menu / dropdown direction / internal-route resolution ONLY.
No visual design change. No Cloudflare / DNS / WordPress writes. No commit. No push.

---

## 1. ISSUES FIXED (3 production blockers)

| # | Reported issue | Root cause | Fix | Result |
|---|----------------|-----------|-----|--------|
| 1 | Spurious "Request A Quote" inside the main menu | `.mobile-cta` block lived **inside** `<nav class="nav-menu">` with no desktop `display:none`, so it rendered as an extra menu entry on desktop. WP audit proved "Request a Quote" appears **0×** in the live `alumcasting.com` header nav. | `.mobile-cta { display: none; }` on desktop (mobile media query already reveals it at ≤768px). The legitimate desktop CTA stays as `.header-cta` (outside `<nav>`). | EXTRA_REQUEST_A_QUOTE = 0 |
| 2 | Dropdown submenu arranged horizontally | Descendant selector `.nav-menu ul { display:flex }` matched the nested `.dropdown` `<ul>`, overriding `.dropdown { display:none }`. | Scoped to `.nav-menu > ul` (direct child only) + added `.dropdown li { display:block; width:100% }`. No global selectors. | DROPDOWN_DIRECTION = VERTICAL |
| 3 | All inner pages return 404 | **A_BASEURL + B_MENU_URL.** `hugo.toml` baseURL was root (`https://alumcasting.com/`) and every menu/footer/CTA link used `relURL` on a **leading-slash** path → emits `/contact/` (no subpath) → 404 on the GitHub Pages subpath. | (a) `absURL` on all manual link calls (prepends full baseURL incl. `/alumcasting/`). (b) `hugo.toml` baseURL → `https://diecasting.github.io/alumcasting/`. | HTTP_404_ROUTES = 0 |

---

## 2. MENU AUDIT (STEP 1)

**WORDPRESS_MENU** (read-only fetch of `alumcasting.com` `<header>`):
Top-level = Home, About, Capabilities▾ (Aluminum/Magnesium/Semi-Solid Die Casting, Die Casting Tooling, Precision CNC Machining, Surface Finishing, Quality Inspection), Industries▾ (Automotive, Electric Vehicles, Medical Equipment), Engineering▾ (Die Casting Design Guide, Material Selection, CNC Guide), Contact.
"Request a Quote" = **0 occurrences** anywhere in the full document.

**HUGO_MENU** (before fix): Home, Capabilities▾ (7), *Aluminum Die Casting (extra top-level — WP only has it as a Capabilities child)*, Industries▾ (5), About, *Blog (not in WP top-level)*, Contact, + mobile-cta "Request a Quote" + header-cta "Request a Quote".

**MENU_DIFF:** Hugo carried an EXTRA "Request a Quote" rendered as a menu item (×1, now removed from menu; preserved as header/mobile CTA), an extra top-level "Aluminum Die Casting", and "Blog". WP has an "Engineering▾" dropdown Hugo lacks. Per spec, only the spurious menu-item identity of "Request a Quote" was removed; other diffs are out of P.4.9.1 scope (menu content/parity deferred to later phase).

---

## 3. ROUTING ROOT CAUSE (STEP 6)

`ROUTE_ROOT_CAUSE = { A_BASEURL, B_MENU_URL }` (both present; fixed both).

- **A_BASEURL:** `hugo.toml` `baseURL` was `https://alumcasting.com/` (root). GitHub Pages project sites serve at the **repo subpath** `/alumcasting/`, so a root baseURL misrepresents the deploy location. Now `https://diecasting.github.io/alumcasting/` (CI still overrides identically via `--baseURL "$pages.base_url/"`).
- **B_MENU_URL:** `relURL` on a **leading-slash** input returns it UNCHANGED (does NOT add the subpath). Every hardcoded `relURL "/contact/"` rendered `/contact/` → 404 on the subpath. Switched all 10 layout/menu/footer/cta/head/render-image calls to `absURL`, which prepends the full baseURL incl. subpath → `https://diecasting.github.io/alumcasting/contact/` (200).

**Before-state evidence (live pre-P.4.9.1 deploy):** 20/20 nav-menu hrefs were subpath-less (`/`, `/manufacturing-capabilities/`, `/contact/`, …). On the subpath these resolve to `https://diecasting.github.io/<route>/` → 404.
**After-state evidence (local build):** `SUBPATHLESS_INTERNAL_LINKS = 0` across all 47 generated HTML files.

---

## 4. BUILD (STEP 8)

```
hugo v0.163.3  --cleanDestinationDir --minify
Pages 54 | Non-page files 0 | Static files 22 | Aliases 0
BUILD_EXIT = 0
BROKEN_INTERNAL_LINKS = 0   (0 subpath-less internal hrefs in public/)
BROKEN_IMAGES         = 0   (0 subpath-less image srcs in public/)
```

Note: Hugo reports **54 pages** (includes section/list + taxonomy outputs); **47 static HTML route files** are emitted to `public/` (the count used for routing QA below).

---

## 5. ROUTE INVENTORY + LIVE TEST (STEP 4–5, 10)

| Metric | Value |
|--------|-------|
| TOTAL_EXPECTED_ROUTES | 47 |
| TOTAL_GENERATED_ROUTES | 47 |
| TOTAL_TESTED_ROUTES | 17 live (subpath form) + 47 local (link integrity) |
| HTTP_200_ROUTES | 17 live (all core nav + dropdown routes) / 47 local |
| HTTP_404_ROUTES | 0 |

**Live HTTP test (https://diecasting.github.io/alumcasting/<route>):** all 17 core routes → **200** (Home, manufacturing-capabilities, aluminum-die-casting, magnesium, semi-solid, die-casting-tooling, precision-cnc, surface-finishing, porosity-control, automotive, ev-battery-housing, medical-device, ev-powertrain, motor-housings, about, blog, contact). This confirms the subpath serving architecture is correct; after deploy, the corrected (subpath-form) menu links will resolve to these same 200 routes.

---

## 6. NAVIGATION / DROPDOWN QA (STEP 9)

| Check | Result |
|-------|--------|
| HEADER_MENU | PASS — visible desktop main menu = Home, Capabilities▾, Aluminum Die Casting, Industries▾, About, Blog, Contact (no "Request a Quote" item) |
| EXTRA_REQUEST_A_QUOTE | 0 (no spurious menu item; CTA preserved as header-cta + mobile-cta) |
| DROPDOWN_DIRECTION | VERTICAL (`.dropdown li { display:block; width:100% }` + `.nav-menu > ul` scoping) |
| DROPDOWN_OVERFLOW | 0 (`.dropdown { position:absolute; min-width:250px }`, no horizontal spill) |
| MOBILE_MENU | PASS (checkbox toggle `nav-toggle-cb` intact; `.mobile-cta` revealed ≤768px; `.nav-menu > ul` switches to `flex-direction:column`) |

---

## 7. SEO REGRESSION (STEP 11)

| Element | Status |
|---------|--------|
| H1 | `Aluminum Die Casting Supplier | HPDC & CNC Machining` — **unchanged** (P.4.6 authoritative) |
| CANONICAL | Present; value now `https://diecasting.github.io/alumcasting/` — **logic unchanged**, value is env-correct subpath (intended; production cutover swaps baseURL → alumcasting.com) |
| JSON-LD | 2 blocks: `Organization` + `WebPage`/`BreadcrumbList`/`ListItem` — **structure unchanged** |
| ROBOTS.TXT | Present in `public/` |
| SITEMAP.XML | Present in `public/` |
| Canonical / schema logic | Not modified; only URL *value* tracks baseURL (by design) |

No SEO schema content, canonical logic, or content pages were deleted or altered for routing.

---

## 8. FILES CHANGED (STEP 7)

| File | Change |
|------|--------|
| `hugo.toml` | baseURL → `https://diecasting.github.io/alumcasting/` (comment notes prod cutover) |
| `static/css/main.css` | (a) `.nav-menu > ul` direct-child scoping; (b) `.mobile-cta { display:none }` desktop; (c) mobile MQ `.nav-menu > ul` column; (d) `.dropdown li { display:block; width:100% }` |
| `layouts/partials/site-header.html` | all menu + CTA `relURL` → `absURL` |
| `layouts/index.html` | hero img + CTAs `relURL` → `absURL` |
| `layouts/partials/home-sections.html` | 13 section CTAs + img-card srcs `relURL` → `absURL` |
| `layouts/partials/site-footer.html` | footer links `relURL` → `absURL` |
| `layouts/partials/cta-band.html` | `.Params.cta_url` `relURL` → `absURL` |
| `layouts/partials/head.html` | css/og:image `relURL` → `absURL` |
| `layouts/_default/single.html` | `.Params.cta_url` `relURL` → `absURL` |
| `layouts/_default/list.html` | `.Params.cta_url` `relURL` → `absURL` |
| `layouts/_default/_markup/render-image.html` | `/images/*` dest `relURL` → `absURL` (root-absolute now subpath-correct) |

`.RelPermalink`-based card/related links were already subpath-correct and left untouched.

---

## 9. GIT / DEPLOY STATUS

| Field | Value |
|-------|-------|
| GIT_STATUS | 11 modified (hugo.toml + 9 layouts + main.css); 1 untracked (`P.4.9_IMPLEMENTATION_REPORT.md`) |
| COMMIT | **NOT PERFORMED** |
| PUSH | **NOT PERFORMED** |
| CLOUDFLARE | NOT TOUCHED |
| DNS | NOT TOUCHED |
| WORDPRESS | READ-ONLY (audit fetch only) |

---

## 10. FINAL VERDICT

**P.4.9.1 = PASS.** All 3 production blockers resolved:
- Header main menu no longer contains a spurious "Request a Quote" item.
- Dropdown submenu is vertical (no overflow).
- Every internal route resolves at the GitHub Pages subpath (0 subpath-less links; 17/17 live core routes → HTTP 200; 0 broken images/links).
- SEO signals (H1 / canonical logic / JSON-LD / robots / sitemap) unchanged.

**HARD STOP** — awaiting explicit go-ahead before any commit, push, or Cloudflare/DNS/WordPress action.
