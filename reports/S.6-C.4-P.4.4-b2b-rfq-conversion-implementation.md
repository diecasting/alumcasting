# S.6-C.4-P.4.4 — B2B RFQ Conversion Implementation

**Phase:** S.6-C.4-P.4.4 (implementation — `SOURCE_WRITE = YES` for confirmed P1 items only)
**Date:** 2026-09-06
**Author:** Implementation agent
**Preceding:** P.4 (UX/UI)=PASS, P.4.1=BLOCKED, P.4.2=PASS, P.4.3=CONDITIONALLY_READY
**Driven by:** `PHASE_6_C.4_P.4.3 = CONDITIONALLY_READY` → implement the 5 confirmed P1 B2B conversion fixes.

---

## 1. BASELINE (STEP 0)

| Item | Value | Note |
|------|-------|------|
| HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` | matches expected baseline |
| origin/main | not a local tracking ref; remote `diecasting/alumcasting` | HEAD is the baseline — no HARD STOP |
| Tracked modified (PREVIOUS_BASELINE, C class) | 46 → 47 | +1 = `layouts/shortcodes/formspree.html` edited this phase |
| Untracked (C class + P.4.4) | baseline reports/partials/css/images + 1 new shortcode | `layouts/shortcodes/quality-trust.html` is P.4.4 |
| Content `.md` | 44 | 14 edited by P.4.4 (see §6) |
| Build (this phase) | `hugo --gc --minify` → exit 0, 46 docs | PASS |

**Change classification:** A = P.4.4 RFQ/conversion (shortcode + 11 flagship pages); B = P.4.4 intent/trust (A356 routing, homepage/about trust); C = previous baseline; **D = 0 (no unauthorized change).** No edit touched `hugo.toml`, `layouts/_default/*`, `layouts/partials/*`, `layouts/index.html`, `static/`, SEO/schema/config.

---

## 2. EXECUTIVE VERDICT

> **PHASE_6_C.4_P.4.4 = PASS**

All FINAL GATE conditions are satisfied (see §7). The five P.4.3 P1 conversion gaps were implemented with minimal, reversible, fact-only changes:

- **P1-1** — 11 flagship/service LPs that were CTA-link-only now have an embedded RFQ form + trust block (bottom-of-page CTA).
- **P1-2** — The reusable Formspree shortcode gained optional B2B qualification fields (material / process / annual volume) on **all 41 forms**, with no increase in required fields (friction preserved).
- **P1-3** — A356 commercial intent now routes via a dedicated CTA block on the editorial article to real commercial pages; the editorial article is retained; `NEW_LP_DEFERRED`.
- **P1-4** — IATF 16949 / ISO 9001 / PPAP / X-Ray / Porosity Control trust proof added to the 11 flagship LPs (repo-verified facts only — no fabricated cert numbers/agencies/expiry/customers).
- **P1-5 (2250T)** — Evidence search found **0 mentions**; per the hard rule, **DO NOT PUBLISH** → `DEFERRED_FACT_VERIFICATION`.

Regression scans are fully green: escaped HTML = 0, escaped forms = 0, all 41 forms `action=https://formspree.io/f/xpqgbdly method=POST`, 1 H1/page, 0 leaks (`diecasting.github.io` / `wp-content` / `/alumcasting/`), JSON-LD 0 errors, 0 broken media.

---

## 3. STEP 3–6 — RFQ FORM COMPONENT & DESIGN

**Component strategy (STEP 3):** Extended the **existing** `layouts/shortcodes/formspree.html` rather than building a duplicate system. All forms site-wide (30 previously embedded + 11 new) now inherit the B2B fields. Created `layouts/shortcodes/quality-trust.html` as the new reusable trust block.

**B2B fields (STEP 4):** added an *optional* fieldset — `material` (select: Aluminum/A356/A380/A383/ADC12/Magnesium/AM60B/AZ91D/Zinc/Other — **only repo-verified alloys**), `process` (select: HPDC/Semi-Solid/Gravity/Sand/CNC/Tooling/Other — **only repo-verified processes**), `annual_volume` (select: Prototype/Low Volume/Production/High Volume — safe categories, no invented numeric ranges). All optional → low first-submit friction preserved.

**Drawing / CAD upload (STEP 5):** P.4.3 = `NOT_IMPLEMENTED`; the Formspree plan cannot be verified from build context. Per the explicit rule ("do not deploy a field that looks like it works but doesn't"), **no `<input type=file>` was added** → `DRAWING_UPLOAD = DEFERRED` (`FILE_UPLOAD_STATUS = NOT_IMPLEMENTED`). The homepage misleading "Upload CAD Drawing" CTA was corrected to an honest `mailto:hank@alumcasting.com` path.

**Form UX (STEP 6):** mobile-friendly, every `label` associated with its `input`/`select`, `email` is `type=email`, only `name`/`email`/`message` required, clear "Request a Quote" button, `action=xpqgbdly`, `method=POST`, **no JS**, no WordPress/PHP/SiteGround dependency.

---

## 4. STEP 7–8 — FLAGSHIP + HOMEPAGE PLACEMENT

Each of the 11 CTA-link-only flagship/service pages received, at the bottom of its content:
1. `{{< quality-trust >}}` — trust/capability proof block
2. `{{< formspree >}}` — embedded RFQ form

Hero CTA (P.4 system) and mid-page CTA links already existed; the bottom form closes the "navigate-away-to-convert" friction gap. Homepage kept its hero "Request a Quote" and was **not** given an oversized form (STEP 8); it gained a `40,000㎡` trust row and a corrected CAD CTA.

---

## 5. STEP 9 — A356 COMMERCIAL INTENT ROUTING (P1-3)

`/a356-aluminum-die-casting-porosity-control/` keeps its editorial H1 ("Battle on the Shop Floor") and embedded form. Added a **"Sourcing Production-Grade A356 Die Casting"** CTA block that routes commercial A356 intent to real commercial pages:
- `/semi-solid-die-casting-heat-treatable-aluminum/`
- `/aluminum-die-casting/`
- `/automotive-die-casting-parts/`

Editorial article **not deleted**. No dedicated thin A356 commercial LP was created → `NEW_LP_DEFERRED`.

---

## 6. STEP 10–14 — TRUST, CAPABILITY FACTS, DO-NOT-CREATE

**Trust block (STEP 10):** `quality-trust.html` renders verified IATF 16949, ISO 9001, PPAP Support, X-Ray Inspection (→ links to real `/porosity-control-x-ray-inspection-castings/`), Porosity Control (→ same), CMM Dimensional. **No fabricated cert numbers, agencies, expiry, customers, or OEM-supplier status.**

**Capability facts (STEP 11):** 5000T retained (verified, unchanged); 3500T magnesium re-confirmed from its single existing source (not expanded); **2250T = 0 mentions → DO NOT PUBLISH → `DEFERRED_FACT_VERIFICATION`.**

**21 years / 40,000㎡ (STEP 12):** both are repo-verified facts (in `automotive-die-casting-parts.md`). Surfaced on Homepage ("Manufacturing at a Glance" table) and About (facility paragraph). "21 years" not changed to "22 years".

**Large structural (STEP 13):** existing 5000T content already partially covers it; **no new LP created** → `NEW_LP_DEFERRED_TO_P.4.5` (build only after verified structural facts).

**Electronics / Industrial (STEP 14):** `DO_NOT_CREATE` per P.4.3 — no thin pages added.

---

## 7. FINAL GATE (STEP 25)

| Gate | Result |
|------|--------|
| P0_REGRESSION | **0** |
| REAL_HTML_FORMS > 0 | **41** |
| ESCAPED_FORMS | **0** |
| ALL_RFQ_FORMS_ENDPOINT_CORRECT | **YES** (xpqgbdly) |
| ALL_RFQ_FORMS_METHOD_POST | **YES** |
| RFQ_CORE_FIELDS | **PASS** (name★/email★/company/phone/message★) |
| FLAGSHIP_RFQ_PATH | **PASS** (11/11 flagship LPs now embedded) |
| A356_INTENT_ROUTING | **PASS** (routing block + editorial retained) |
| IATF_TRUST | **PASS** |
| PPAP_TRUST | **PASS** |
| 2250T_SSM | **DEFERRED_FACT_VERIFICATION** (not published) |
| 5000T | **PASS** |
| 3500T_MAGNESIUM | **PASS** (re-confirmed) |
| 21_YEARS | **PASS** (surfaced) |
| 40000_SQM | **PASS** (surfaced) |
| SEO_REGRESSION | **0** |
| SCHEMA_REGRESSION | **0** |
| URL_REGRESSION | **0** |
| MEDIA_REGRESSION | **0** |
| BUILD | **PASS** (exit 0, 46 docs) |
| UNAUTHORIZED_CHANGES | **0** |

All gates satisfied → **PHASE_6_C.4_P.4.4 = PASS**.

---

## 8. REGRESSION SCAN EVIDENCE (STEP 20–23)

Rendered-HTML scan (46 pages):
- REAL_HTML_FORMS (xpqgbdly) = **41** (was 30; +11 new embedded)
- ESCAPED_FORMS = **0**; ESCAPED_HTML pages = **0**
- BAD_ENDPOINT / BAD_METHOD = **0**
- H1 violations (≠1) = **0**
- Leaks: `diecasting.github.io` = 0, `wp-content` = 0, `/alumcasting/` = 0
- JSON-LD parse errors = **0**; schema types intact (Org/WebPage/Article/FAQPage/BreadcrumbList + FAQ Answer/Question)
- Media broken = **0**
- Forms with material/process/annual_volume = **41/41**; forms with file upload = **0**
- 2250T present anywhere in `public/` = **False**

---

## 9. DIFF SAFETY (STEP 24)

- **A (P.4.4 RFQ/conversion):** `layouts/shortcodes/formspree.html` (extended); `layouts/shortcodes/quality-trust.html` (new); 11 flagship `content/*.md` (embedded form + trust).
- **B (P.4.4 intent/trust):** `content/a356-aluminum-die-casting-porosity-control.md` (routing); `content/_index.md` (40,000㎡ + CAD CTA fix); `content/about-alumcasting-die-casting-expert.md` (21yr/40k surfacing).
- **C (previous baseline, untouched this phase):** the 46 pre-existing tracked modifications (incl. `hugo.toml`, `layouts/_default/*`, `layouts/partials/*`, `layouts/index.html`) + prior untracked reports/partials/css/images.
- **D (unauthorized):** **0** — no file outside `content/` and `layouts/shortcodes/` was modified.

---

## 10. DELIVERABLES (STEP 26)

- `reports/S.6-C.4-P.4.4-b2b-rfq-conversion-implementation.md` (this file)
- `reports/S.6-C.4-P.4.4-rfq-form-implementation.csv` (form state before/after for 11 flagship pages + component)
- `reports/S.6-C.4-P.4.4-a356-intent-routing.csv` (A356 routing change + NEW_LP_DEFERRED)
- `reports/S.6-C.4-P.4.4-capability-trust-implementation.csv` (trust/capability changes + 2250T DEFERRED)

---

## 11. DEFERRED / NOT IMPLEMENTED (per spec)

- **Drawing / CAD upload** — `DEFERRED` (Formspree plan unverifiable; no fake upload field).
- **2250T semi-solid** — `DEFERRED_FACT_VERIFICATION` (0 repo facts; do not publish).
- **Large structural LP** — `NEW_LP_DEFERRED_TO_P.4.5` (build only after verified structural facts).
- **Dedicated A356 commercial LP** — `NEW_LP_DEFERRED` (routing to existing pages instead of thin page).
- **Electronics / Industrial LPs** — `DO_NOT_CREATE` (thin-page risk).
- No new industry pages, no large SEO content, no new URL, no Google Ads campaign, no Analytics, no conversion tracking, no Cloudflare/DNS/redirect/WordPress/production deploy.

---

*HARD STOP: WordPress = NO, Cloudflare = NO, DNS = NO, GitHub Pages settings = NO, redirects = NO, production deploy = NO, commit = NO, push = NO, actions rerun = NO. Source changes are limited to `content/` and `layouts/shortcodes/` (P.4.4 A/B only).*
