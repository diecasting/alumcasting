# S.6-C.4-P.3 — Content Gap Resolution (Two Approved REBUILDS)

**Phase:** S.6-C.4-P.3  ·  **Mode:** `CONTENT GAP RESOLUTION (local)`  ·  **Date:** 2026-09-05
**Baseline HEAD:** `cbc5101587de947cac2337377a8cfb49d938212e` (= origin/main, verified)
**Repo:** `D:/Workbuddy/2026-08-31-19-30-01/alumcasting`
**Upstream decision source:** `reports/S.6-C.4-P.2-final-2-content-gap-decision-audit.md` (both = REBUILD, HIGH)

---

## PHASE_6_C.4_P.3 STATUS = **PASS**

Both P.2-approved REBUILD decisions are now executed as local Hugo content: two new pages were created at the exact production URLs, built, and validated. **No redirect was implemented, no production/WP/Cloudflare/DNS change was made, and no commit/push/deploy occurred.**

> **REDIRECT NOTE:** The two old URLs (`/precision-die-casting-medical-equipment/` and `/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/`) currently 301 to *wrong* targets in production. Per the P.3 contract, those 301s are **left untouched** in this phase. The new pages are the future owners; the 301 repoint is deferred to the Q phase.

---

## STEP 0–2 — BASELINE LOCK + EVIDENCE LOADED

| Field | Value |
|---|---|
| HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` |
| ORIGIN/MAIN | `cbc5101587de947cac2337377a8cfb49d938212e` (via `git ls-remote`) |
| INITIAL_WORKTREE | 61 entries (41 P-phase tracked-modified + untracked P/P.1/P.2 reports/images/CNAME) |
| EVIDENCE READ | P.2 audit + P.2 CSV + P.1 audit + P.1 CSVs (T1 WP id 3174; T2 cold-chamber:53 citation) |

Baseline matches → **no HARD STOP**. P.2 is the decision source; pages were built from its evidence, not from the instruction summary alone.

---

## STEP 3–7 — TARGET 1: PRECISION DIE CASTING FOR MEDICAL EQUIPMENT

**New file:** `content/precision-die-casting-medical-equipment/_index.md` (subdir leaf bundle exactly per spec; `url` front matter → `/precision-die-casting-medical-equipment/`).

| Field | Value |
|---|---|
| source URL | `https://alumcasting.com/precision-die-casting-medical-equipment/` |
| new Hugo URL | `https://alumcasting.com/precision-die-casting-medical-equipment/` |
| source WP ID | 3174 ("Precision Die Casting for Medical Equipment", 962 words) |
| source evidence | 6 H2 (medical compliance, technical thresholds, capability matrix, chemical resistance, PPAP L3, FAQ) + 1 matrix table + RFQ CTA |
| search intent | Aluminum/magnesium **precision die casting** for medical equipment — distinct from CNC machining |
| business intent | Distinct service line (compliance, PPAP, traceability, medical-grade quality) |
| content parity | REBUILT from WP die-casting-specific facts (NOT a rewrite of the machining page) |
| technical facts preserved | Material traceability, dimensional discipline, porosity/X-ray control, chemical-resistance surface planning, PPAP Level 3, capability matrix |
| missing facts | None required — no certs/clients/cases fabricated |
| new page status | REBUILT |
| H1 | `Precision Die Casting for Medical Equipment` |
| title | `Precision Die Casting for Medical Equipment` |
| description | unique, search-intent aligned, no keyword stuffing (verified present in `<head>`) |
| canonical | `https://alumcasting.com/precision-die-casting-medical-equipment/` (production, unquoted-minify) |
| schema | WebPage + Organization + nested BreadcrumbList (engine default; **no global schema edit**) |
| internal links | **Inbound:** 1 natural link added to `/medical-device-component-machining/` body. **Outbound:** `/aluminum-die-casting/`, `/precision-cnc-machining/`, `/medical-device-component-machining/`, `/contact/` — all resolve. |
| image dependency | LOCAL: `/images/semi-solid-casting-ssm-zero-porosity-structural-parts.webp`, `/images/X-Ray-Detector.webp` (both from the 18 already-migrated media; no WP download) |
| validation | GENERATED_OUTPUT_VALID |
| confidence | HIGH |

**Content-quality safeguards honored:** page is independently about die casting (uses "aluminum die casting", "HPDC", "cold-chamber", "porosity", "X-ray", "PPAP"); explicitly distinct from the CNC machining sibling; no fabricated ISO 13485/FDA/device registrations/OEM names/tolerances/volumes; the capability matrix is generic process discipline, not client data.

---

## STEP 8–11 — TARGET 2: HOT-CHAMBER VS COLD-CHAMBER ALUMINUM DIE CASTING MYTH

**New file:** `content/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/_index.md` (subdir leaf bundle exactly per spec; `url` front matter → `/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/`).

| Field | Value |
|---|---|
| source URL | `https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/` |
| new Hugo URL | `https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/` |
| source evidence | P.2: live WP (P.1 200, now 301); topic = debunking aluminum hot-chamber gooseneck-dissolution; cited as authority from `/cold-chamber-die-casting-services.md:53` |
| search intent | EDUCATIONAL/TECHNICAL article — hot vs cold chamber aluminum die casting myth |
| business intent | MEDIUM-HIGH (customer education preventing bad process/metal choice) |
| article intent | Myth-debunking / process explainer (NOT a sales landing page) |
| cold-chamber citation relationship | **Already satisfied** — `cold-chamber-die-casting-services.md:53` links to `/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/`, which exactly matches the new page URL → **no cold-chamber repair needed** |
| H1 | `Hot Chamber vs. Cold Chamber Aluminum Die Casting Myth` |
| title | `Hot Chamber vs. Cold Chamber Aluminum Die Casting Myth` |
| description | unique, natural, no overstatement (verified present) |
| canonical | `https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/` |
| schema | Article + Organization + BreadcrumbList (reused `schema_type: Article` engine path; includes `datePublished`/`dateModified` + `image`; **no global schema edit**) |
| internal links | **Inbound:** already present via cold-chamber:53 (exact match). **Outbound:** `/cold-chamber-die-casting-services/`, `/zinc-die-casting-services/`, `/aluminum-die-casting/`, `/contact/` — all resolve. |
| image dependency | LOCAL: `/images/5000t-aluminum-die-casting-machine-large-structural-parts.webp` (cold-chamber machine, topically matched; used as Article `image` + body) |
| validation | GENERATED_OUTPUT_VALID |
| confidence | HIGH |

**Overstatement safeguards honored:** wording is "generally / typically / in conventional production / depends on alloy/process/equipment"; explicitly states flat "hot-chamber aluminum die casting is impossible" would overstate; notes specialized exceptions exist; no invented equipment models, temperatures, supplier cases, or experimental data. Article intent preserved (educational) distinct from the cold-chamber **service** page (links out for CTA only).

---

## STEP 12–14 — FRONT MATTER / SCHEMA / IMAGES

- **Front matter** strictly follows the existing repo pattern: `title, description, canonical, url, robots, page_type, schema_type, translationKey, language, rfq, cta, cta_url, related_services`. Target 1 = `page_type: page` / `schema_type: WebPage`; Target 2 = `page_type: post` / `schema_type: Article` + `date`/`modified`/`image`. No new/invented fields.
- **Schema** reuses the existing engine (`layouts/partials/seo-jsonld.html`): Organization + WebPage always; Article only when `schema_type: Article`. No global schema/SEO template was modified. FAQ JSON-LD was **not** emitted (engine generates FAQPage only via a `faq:` param; existing pages keep body-FAQ only — matched that norm).
- **Images** use only the 18 already-localized `/static/images/` media. No WP media re-download, no hotlink, no new/modified asset files.

---

## STEP 15 — BUILD + VALIDATION (A–L)

`hugo --gc --minify` → **exit 0**, 52 pages / 20 static.

| Check | TARGET 1 | TARGET 2 |
|---|---|---|
| A. Build | PASS (exit 0) | PASS (exit 0) |
| B. HTML generated | `public/.../index.html` present | `public/.../index.html` present |
| C. Front matter | valid; rendered | valid; rendered |
| D. canonical | `https://alumcasting.com/...` ✓ | `https://alumcasting.com/...` ✓ |
| E. og:url | `https://alumcasting.com/...` ✓ | `https://alumcasting.com/...` ✓ |
| F. sitemap | included ✓ | included ✓ |
| G. JSON-LD | WebPage + Org + Breadcrumb (valid) | Article + Org + Breadcrumb (valid; datePublished/dateModified/image) |
| H. internal links | all resolve (0 broken) | all resolve (0 broken) |
| I. no WP media | 0 `wp-content` ✓ | 0 `wp-content` ✓ |
| J. no staging URL | 0 `diecasting.github.io`, 0 `/alumcasting/` ✓ | 0 `diecasting.github.io`, 0 `/alumcasting/` ✓ |
| K. no malformed URL | 0 bad `//` internal links ✓ | 0 bad `//` internal links ✓ |
| L. no unintended source changes | tracked diff unchanged (see Step 17) ✓ | tracked diff unchanged ✓ |

**Only `GENERATED_OUTPUT = VALID` is claimed** (not production HTTP 200 — this is the local build environment).

---

## STEP 16 — REDIRECT SAFETY

- Repo contains **no** Hugo/Cloudflare redirect config (`find` for `*redirect*` / `_redirects` returns only `reports/` documentation, not config). The live 301s live outside this repo (WP/Cloudflare) and were not touched.
- **REDIRECTS_IMPLEMENTED = 0.** No `static/_redirects`, no Hugo alias, no server/WP redirect edit.

---

## STEP 17 — DIFF SAFETY

| Check | Result |
|---|---|
| `git status --short` count | 63 (baseline 61 + 2 new content dirs); +2 report files pending → 65 |
| `git diff --name-only` (tracked) | **41** — unchanged from baseline (all P-phase carry-over) |
| New tracked-content change from P.3 | **1 line** in `content/medical-device-component-machining.md` (the added inbound sentence; explicitly permitted by Step 7/17) |
| Unauthorized changes | **0** (the medical page's other diff line — `wp-content`→`/images/` image URL — is pre-existing P-phase media-migration working-tree state already in the 41-file baseline, not introduced by P.3) |
| `hugo.toml` / `layouts/` / `data/` / `static/` / other content | unchanged by P.3 (41-file set is P-phase; no new file added to it) |

Allowed P.3 files: (1) new medical page, (2) new hot-chamber article, (3) the single permitted medical-page inbound link, (4) the two `reports/` deliverables. **No unrelated modification.**

---

## STEP 18–19 — DELIVERABLES

| File | Purpose |
|---|---|
| `reports/S.6-C.4-P.3-content-gap-resolution.md` | This report |
| `reports/S.6-C.4-P.3-content-gap-resolution.csv` | 2-row resolution manifest (fields per Step 19; `redirect_status = NOT_IMPLEMENTED_IN_P3`) |

---

## STEP 20 — FINAL GATE

**P.3 = PASS**

- TARGET 1 = REBUILT · TARGET 2 = REBUILT
- BUILD = PASS · HTML = PASS · JSONLD = PASS · CANONICAL = PASS · INTERNAL_LINKS = PASS · MEDIA = PASS · NO_WP_MEDIA = PASS · NO_STAGING_URL = PASS
- SOURCE_SCOPE = PASS · UNAUTHORIZED_CHANGES = 0
- REDIRECTS_IMPLEMENTED = 0 · CLOUDFLARE_CHANGES = 0 · DNS_CHANGES = 0 · WORDPRESS_CHANGES = 0
- COMMIT = NO · PUSH = NO · DEPLOY = NO

---

## STEP 21 — REQUIRED FINAL OUTPUT

```
PHASE_6_C.4_P.3 = PASS

BASELINE:
  HEAD = cbc5101587de947cac2337377a8cfb49d938212e
  ORIGIN/MAIN = cbc5101587de947cac2337377a8cfb49d938212e
  SOURCE_CHANGES_BEFORE = 41 tracked (P-phase baseline)
  SOURCE_CHANGES_AFTER  = 41 tracked (P.3 added 1 permitted inbound line inside an already-modified file; net tracked-diff count unchanged)

TARGET 1:
  SOURCE = https://alumcasting.com/precision-die-casting-medical-equipment/ (WP id 3174)
  NEW_HUGO_URL = https://alumcasting.com/precision-die-casting-medical-equipment/
  ACTION = REBUILD
  STATUS = REBUILT
  H1 = Precision Die Casting for Medical Equipment
  TITLE = Precision Die Casting for Medical Equipment
  CANONICAL = https://alumcasting.com/precision-die-casting-medical-equipment/
  SCHEMA = WebPage + Organization + BreadcrumbList
  INTERNAL_LINKS = inbound(from medical-device-component-machining) + outbound(4, all resolve)
  MEDIA = LOCAL (/images/semi-solid-...webp, /images/X-Ray-Detector.webp)
  BUILD = PASS
  CONFIDENCE = HIGH

TARGET 2:
  SOURCE = https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/
  NEW_HUGO_URL = https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/
  ACTION = REBUILD
  STATUS = REBUILT
  H1 = Hot Chamber vs. Cold Chamber Aluminum Die Casting Myth
  TITLE = Hot Chamber vs. Cold Chamber Aluminum Die Casting Myth
  CANONICAL = https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/
  SCHEMA = Article + Organization + BreadcrumbList
  INTERNAL_LINKS = inbound(from cold-chamber-die-casting-services:53, exact match) + outbound(4, all resolve)
  MEDIA = LOCAL (/images/5000t-aluminum-die-casting-machine-large-structural-parts.webp)
  BUILD = PASS
  CONFIDENCE = HIGH

REDIRECTS_IMPLEMENTED = 0
CLOUDFLARE = NO
DNS = NO
WORDPRESS = NO
COMMIT = NO
PUSH = NO
DEPLOY = NO

UNAUTHORIZED_CHANGES = 0

REPORT = reports/S.6-C.4-P.3-content-gap-resolution.md
CSV = reports/S.6-C.4-P.3-content-gap-resolution.csv

HARD STOP = (no further action; Q phase owns the 301 repoint)
```

*End of report — S.6-C.4-P.3. Two REBUILD pages created and validated locally; no redirect/production/WP/Cloudflare/DNS change; no commit/push/deploy.*
