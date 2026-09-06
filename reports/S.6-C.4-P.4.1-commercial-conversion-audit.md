# S.6-C.4-P.4.1 — Commercial Conversion & Google Intent Audit

**Project:** AlumCasting Hugo migration (diecasting/alumcasting)
**Date:** 2026-09-06
**Mode:** READ-ONLY audit (no source, layout, CSS, config, form, CTA, URL, redirect, Cloudflare, DNS, WordPress, or production changes; no commit/push/deploy)
**Deliverables:** this report + 3 CSVs (see Part 17)

---

## 0. Executive Summary — the blunt answer

> **Does the current 52-page site convert Google Organic / Google Ads / B2B engineering traffic into RFQ?**

**No. Not in its current built state.** The site is technically online and SEO-clean, but two **P0 regressions** make it commercially non-functional:

1. **P0-A — Entire page body is HTML-escaped** (`&lt;p&gt;`, `&lt;h2&gt;`, `&lt;img&gt;` shown as literal text). Introduced by P.4's `{{ $c := .Content }}{{ $c = replaceRE ... }}{{ $c }}` pattern. Every content page currently displays raw HTML source instead of formatted content.
2. **P0-B — The RFQ Formspree form is escaped** (`&lt;form class="rfq-form" action="https://formspree.io/f/xpqgbdly" method="POST"&gt;`). There is **no functional `<form>` element anywhere on the site** (0 of 46 pages). RFQ submission is impossible.

Net effect: the intended funnel
`Google Organic → Commercial LP → Capability Proof → Trust → RFQ`
and
`Google Ads → Intent-Matched LP → RFQ`
**do not exist** until P0-A/P0-B are fixed. The commercial *content and structure* are largely present in source (good intent coverage, correct Formspree endpoint, intact SEO), but they are currently **invisible and non-submitting**.

**What is healthy:** SEO/Schema/URL layer is intact (Title, Meta, H1, Canonical = alumcasting.com, Organization/WebPage/BreadcrumbList JSON-LD on all pages, FAQPage + Article where applicable). No `diecasting.github.io`, `wp-content`, or `/alumcasting/` leaks. No legacy WordPress/PHP/SiteGround form dependency. No-JS hamburger nav is correctly implemented.

---

## 1. Baseline Confirmation

| Check | Result |
|---|---|
| HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` ✓ matches required |
| ORIGIN/MAIN (remote head) | same hash ✓ |
| Working tree | P.3 + P.4 modifications present (expected, from prior phases) |
| P.4 files present | `static/css/main.css`, `site-header.html`, `site-footer.html`, `related-cards.html`, `cta-band.html`, `home-sections.html` ✓ |
| Hugo build | PASS — exit 0, 128 ms |
| Pages (Hugo metric) | **52** (44 content + 2 taxonomy indexes + sitemap/RSS/404 objects) |
| Rendered HTML docs audited | **46** (`public/**/index.html`) |
| Static files | **21** |

No HARD STOP triggered (baseline matches).

---

## 2. P0 Blockers (with evidence)

### P0-A — Content body HTML-escaped site-wide (P.4 regression)

**Evidence (raw bytes, `public/contact/index.html` and every content page):**
```
<div class="container content">&lt;p>&lt;strong>Technical Author:&lt;/strong> Hank, Senior Tooling Engineer &amp;amp; Manufacturing Professional
&lt;strong>Production Hub:&lt;/strong> Dongguan &amp;amp; Shenzhen Region ...
```
- 44 of 46 rendered pages contain `&lt;p` / `&lt;h2` / `&lt;h3` / `&lt;form` (escaped body).
- `&amp;amp;` double-escaping is the smoking gun: `.Content` → `replaceRE` (returns plain `string`) → `{{ $c }}` prints the string, which Hugo re-escapes.

**Root cause (git-verified):**
- `git show HEAD:layouts/_default/single.html` → pre-P.4 was simply `{{ .Content }}` (renders correctly).
- P.4 changed it to:
  ```go
  {{ $c := .Content }}
  {{ $c = replaceRE "<h1[^>]*>.*?</h1>" "" $c }}
  {{ $c }}            ← string printed → escaped
  ```
  Same pattern in `layouts/_default/list.html` (lines 13-14) and `layouts/index.html` (lines 23-24).
- `replaceRE` returns a `string`; printing a string in a Hugo template HTML-escapes it. The intended H1-strip must be done on a `template.HTML` value, e.g. `{{ .Content | replaceRE ... | safeHTML }}`.

**Impact:** Visitors see raw HTML tags as text → 100% content readability failure; no capability/trust proof is visible; no CTA/link is clickable (anchors are escaped too).

### P0-B — RFQ Formspree form escaped → zero functional forms

- `&lt;form class="rfq-form" action="https://formspree.io/f/xpqgbdly" method="POST"&gt;` present on **30 pages** (the `{{< formspree >}}` shortcode is embedded in 29 content files + contact).
- **0 pages** contain a real `<form>` element.
- The Formspree endpoint itself is correct (`xpqgbdly` = authorized target), but the form is rendered as literal text, so **no RFQ can be submitted from any page**.
- `LEGACY_FORM_DEPENDENCY = NO` — there is no WordPress/PHP/SiteGround form handler; the site is 100% static Formspree (just broken by the escape bug).

**Impact:** B2B conversion path = 0. Every "Request a Quote" CTA links to `/contact/`, which also has the escaped (dead) form.

---

## 3. Part-by-Part Findings

### PART 1 — Site-wide commercial funnel audit
46 pages classified (Homepage, Service, Material, Industry, Capability, Quality, About, Contact, Article, Other). Deliverable: `S.6-C.4-P.4.1-commercial-page-matrix.csv`. Every row currently carries `conversion_risk = P0 – content escaped + RFQ form broken`. Underlying page-type/intent/capability data is populated for post-fix CRO.

### PART 2 — Google Organic commercial intent audit
Intent coverage is **structurally present but not deliverable** while P0-A stands. Mapping of the user's required themes:
- Manufacturing services: `aluminum die casting` (✓ `/aluminum-die-casting/`), `magnesium` (✓), `semi-solid` (✓), `CNC` (✓), `X-ray` (✓), `heat treatment` (△ partial), `high-pressure/HPDC` (✓).
- Material intent: ADC12 (✓), A380 (△ partial—on a383 page), A356 (✓), AM60B (✓), AZ91D (✓).
- Industry intent: automotive (✓), medical (✓ P.3 rebuild), electronics (**MISSING dedicated LP**), industrial (**MISSING dedicated LP**).
- Capability intent: 5000T (✓), large structural (✓), tolerance (△), high-volume (△), X-ray (✓), PPAP (△), quality (✓).
Verdict: **ORGANIC_INTENT_AUDIT = FAIL** (pages render as text); intent *architecture* is mostly COVERED, pending P0 fix.

### PART 3 — Keyword → Landing Page architecture
30 intents mapped in `S.6-C.4-P.4.1-organic-intent-map.csv`. All `match_status` values are `COVERED_RENDER_BROKEN` / `PARTIAL_RENDER_BROKEN` / `MISSING_LP`; `rfq_status = BROKEN` for all. Two `MISSING_LP`: *electronics die casting*, *industrial die casting* → recommend new dedicated landing pages post-P0.

### PART 4 — Google Ads landing page audit
Above-the-fold checks (What/Who/Capability/Trust/Next-step) **cannot be evaluated visually because the body is escaped**. H1/hero exist in layout chrome but proof/CTA bodies are literal text. Verdict: **ADS_LANDING_AUDIT = FAIL**. Once P0 fixed, strongest Ads candidates: `/aluminum-die-casting/`, `/magnesium-die-casting-services/`, `/precision-die-casting-medical-equipment/`, `/large-scale-5000t-aluminum-die-casting-factory-china/`.

### PART 5 — B2B buyer journey audit
The required journey (Can you make / volume / material / tolerance / industry / quality / inspect / trust / RFQ) **answers exist in source** (machine tonnage, materials, tolerances, PPAP, X-ray, 21yr, 40,000㎡) but are **invisible** due to P0-A and **un-actionable** due to P0-B.
- **P0:** content invisible, RFQ impossible.
- **P1:** 2250T semi-solid proof text missing from all content; 21-year proof on only 1 page; no file-upload for drawings (blocks engineering RFQ).
- **P2:** volume/material qualification fields absent from form.

### PART 6 — CTA system audit
- CTA wording **inconsistent**: "Request a Quote" (cta-band, 35 pages), "Send RFQ" (form button, escaped), "Ready to start your project?" (cta-band heading), "Contact" (gravity page).
- CTA hierarchy: header RFQ button + in-page cta-band + (dead) form button — reasonable structure, but all destinations currently broken.
- CTA placement: header (above fold ✓), cta-band (after proof ✓), but proof itself is escaped.
Verdict: **CTA_AUDIT = FAIL** (destinations non-functional).

### PART 7 — RFQ form audit
See `S.6-C.4-P.4.1-rfq-form-audit.csv`. Field-level: name*, email*, company, phone, message* + hidden `_subject` — all present **in source** but inert (escaped). **No file upload (CAD/PDF)** → high B2B friction. No material/service dropdown, no annual-volume field → weak qualification. `LEGACY_FORM_DEPENDENCY = NO`.
Verdict: **RFQ_FORM_AUDIT = FAIL**.

### PART 8 — Formspree target configuration audit
- `formspree.io` present on 30 pages; endpoint **always** `https://formspree.io/f/xpqgbdly`.
- **Single endpoint**, matches `AUTHORIZED_TARGET_ENDPOINT`. No second/legacy endpoint. No `action=` to WP/PHP. Homepage/contact/RFQ all share the same correct endpoint.
- Form is rendered escaped → non-functional, but the *configuration* is correct.
Verdict: **FORMSPREE_AUDIT = PASS** (config correct; functional break recorded under RFQ/P0-B).

### PART 9 — RFQ recommended structure
Current form lacks: service/material `<select>`, annual-volume field, and **file upload (CAD/PDF)**. Recommended B2B structure (Name, Company, Business Email, Service select, Material, Application, Annual Volume, Project Details, Upload Drawing, Submit) is **not met**. Add file input + qualification fields post-P0.

### PART 10 — Capability / Trust architecture audit
Verified facts vs. visibility (all currently **invisible** due to P0-A; values per source presence):
- 160T–5000T aluminum HPDC → proof text present (✓ source, e.g. `/manufacturing-capabilities/`, `/large-scale-5000t/`), invisible.
- 630T / 1600T SSM → present on `/manufacturing-capabilities/` + `/cold-chamber/` (✓ source), invisible.
- **up to 2250T semi-solid → MISSING from all content** (✗ source) — real gap.
- up to 3500T magnesium → only `/cold-chamber/` (sparse).
- ±0.01mm machining → 10 pages (✓ source), invisible.
- X-Ray → 36 content files (✓), invisible.
- IATF 16949 → 28 files (✓), invisible. ISO 9001 → 21 files (✓), invisible. PPAP → 6 files (✓), invisible.
- 40,000㎡ → 3 files (✓ source, not missing), invisible + not on high-value LPs.
- 21 years → **only 1 file** (`/automotive-die-casting-parts/`) — too sparse; should appear on About/Contact/Capabilities.
Verdict: **CAPABILITY_PROOF_AUDIT = FAIL**, **TRUST_PROOF_AUDIT = FAIL** (invisible); plus 2 content gaps (2250T, 21yr sparsity).

### PART 11 — Commercial internal linking
Internal links exist in source (`internal_commercial_links` counts in matrix) but are **escaped → not clickable** while P0-A stands. Structural observations for post-fix:
- Service→Capability/Industry/Article links generally present.
- Some Articles lack a clear RFQ path; related-cards partially mitigate.
- Mild cannibalization risk: alloy pages (380/383/adc12/a356) overlap; recommend clear differentiation (alloy + application angle).
Verdict: **INTERNAL_COMMERCIAL_LINKING = FAIL** (links inert).

### PART 12 — Homepage commercial funnel audit
Homepage Hero + curated sections (`home-sections.html`) + cta-band exist and follow the intended path (Header → Hero → What We Make → Capabilities → Capacity → Industries → Quality → Why AlumCasting → RFQ CTA → Footer). **But** the `.Content` body and section copy are escaped → the mid-page proof is literal text. Structure is sound; rendering is broken.
Verdict: blocked by P0-A.

### PART 13 — Mobile conversion audit
Structural: no-JS hamburger nav (✓, good for perf/ads), responsive CSS tokens, 390/768/1024/1440 breakpoints defined. **Cannot validate tap targets / form usability / hero height visually** because content+form are escaped. No horizontal-overflow or font/JS risks identified (no third-party JS beyond Formspree POST).
Verdict: **MOBILE_CONVERSION_AUDIT = FAIL** (form/CTA non-functional; visual QA blocked by P0).

### PART 14 — Performance / Ads UX risk
- No unnecessary JS (no-JS hamburger preserved ✓ — do NOT reintroduce JS).
- No external third-party scripts except Formspree form POST.
- Risk: oversized `.webp` images not systematically checked; recommend a lazy-load/`loading=eager` above-fold audit post-P0. No blocking-CSS or layout-shift evidence.
Verdict: low perf risk; Ads UX blocked by P0.

### PART 15 — SEO / Schema safety check
- Title, Meta Description, H1 (1 per page), H2 hierarchy: intact in `<head>`/layout; body H2s exist in source.
- Canonical = `https://alumcasting.com/...` on all pages (no staging leak).
- OG URL, Sitemap, Robots: unchanged by P.4.1 (read-only).
- JSON-LD: Organization + WebPage + BreadcrumbList on all 46; FAQPage (about, automotive, precision-cnc); Article (6 P.3/technical pages). All present and valid.
- No `diecasting.github.io`, `wp-content`, or `/alumcasting/` strings in output.
**SEO_REGRESSION = 0, SCHEMA_REGRESSION = 0, URL_REGRESSION = 0.** (The P.4 regression is in body *rendering*, not the SEO/Schema layer.)

---

## 4. Part 16 — Priority Matrix

### P0 (blocks conversion / Ads / SEO delivery)
| ID | Issue | URL | Evidence | Business impact | SEO impact | Ads impact | Conversion impact | Fix |
|---|---|---|---|---|---|---|---|---|
| P0-1 | Page body HTML-escaped site-wide | all 44 content pages + homepage | `&lt;p&gt;...` literal; `&amp;amp;` double-escape; 44/46 pages | Content 100% unreadable | None (head/SEO intact) | Landing pages unusable | 0 (no readable CTA/proof) | In `single.html`/`list.html`/`index.html` use `{{ .Content | replaceRE "<h1[^>]*>.*?</h1>" "" | safeHTML }}` instead of `{{ $c := .Content }}{{ $c = replaceRE ... }}{{ $c }}` |
| P0-2 | RFQ Formspree form escaped → no functional form | 30 pages w/ `{{< formspree >}}` | `&lt;form ...&gt;`; 0 real `<form>` | No RFQ possible | None | No conversion from Ads | 0 submissions | Same fix as P0-1 (form is inside escaped `.Content`); alternatively render form via partial with `| safeHTML` |

### P1 (materially reduces commercial performance)
| ID | Issue | URL | Evidence | Business impact | Fix |
|---|---|---|---|---|---|
| P1-1 | No file upload (CAD/PDF) in RFQ form | all form pages | form has name/email/company/phone/message only | Engineers cannot send drawings → abandon | Add `<input type=file name=upload>` (Formspree supports) |
| P1-2 | No service/material dropdown + annual-volume field | all form pages | form lacks `<select>`/volume | Weak lead qualification | Add service select + annual-volume field per recommended structure |
| P1-3 | up to 2250T semi-solid proof text missing | all content | grep "2250" → 0 files | Capability claim unverifiable on site | Add SSM tonnage statement to `/manufacturing-capabilities/` + semi-solid page |
| P1-4 | 21-year proof on only 1 page | only `/automotive-die-casting-parts/` | grep "21 years" → 1 file | Trust signal under-used | Add "21 years" to About, Contact, Capabilities |
| P1-5 | No dedicated LP for electronics / industrial die casting | none | MISSING_LP in intent map | Misses two industry intents | Create 2 industry landing pages post-P0 |
| P1-6 | Capability/trust proof not near commercial CTA | most service pages | proof in body, CTA in cta-band; body escaped | Proof not reinforcing conversion | Post-P0, ensure proof blocks sit above cta-band |

### P2 (optimization opportunity)
| ID | Issue | Fix |
|---|---|---|
| P2-1 | CTA wording inconsistent (Request a Quote / Send RFQ / Ready to start your project) | Standardize primary CTA verb |
| P2-2 | No privacy/GDPR notice near form | Add short privacy line |
| P2-3 | No custom success/error UX | Add Formspree thank-you / inline error |
| P2-4 | Spam protection = Formspree default only | Add honeypot/captcha if volume warrants |
| P2-5 | Mild cannibalization: alloy pages (380/383/adc12/a356) overlap | Differentiate by alloy+application; canonical internally |
| P2-6 | Some Articles lack explicit RFQ path | Add cta-band/RFQ link to all articles |
| P2-7 | 40,000㎡ proof only on 3 pages, not on high-value LPs | Surface on homepage + capabilities |

---

## 5. Part 17 — Deliverables
- `reports/S.6-C.4-P.4.1-commercial-conversion-audit.md` (this file)
- `reports/S.6-C.4-P.4.1-commercial-page-matrix.csv` (46 rows)
- `reports/S.6-C.4-P.4.1-organic-intent-map.csv` (30 rows)
- `reports/S.6-C.4-P.4.1-rfq-form-audit.csv` (16 rows)

All are audit artifacts only; no implementation performed.

---

## 6. FINAL GATE

```
PHASE_6_C.4_P.4.1 = BLOCKED

BASELINE:
  HEAD = cbc5101587de947cac2337377a8cfb49d938212e
  ORIGIN/MAIN = cbc5101587de947cac2337377a8cfb49d938212e

PAGES_AUDITED = 52   (46 rendered HTML docs + sitemap/RSS/404 objects)
RENDERED_HTML_AUDITED = 46

ORGANIC_INTENT_AUDIT = FAIL
ADS_LANDING_AUDIT = FAIL
B2B_CONVERSION_AUDIT = FAIL
CTA_AUDIT = FAIL
RFQ_FORM_AUDIT = FAIL
FORMSPREE_AUDIT = PASS
CAPABILITY_PROOF_AUDIT = FAIL
TRUST_PROOF_AUDIT = FAIL
INTERNAL_COMMERCIAL_LINKING = FAIL
MOBILE_CONVERSION_AUDIT = FAIL

AUTHORIZED_FORMSPREE_ENDPOINT =
https://formspree.io/f/xpqgbdly

CURRENT_FORMSPREE_ENDPOINT =
https://formspree.io/f/xpqgbdly   (correct endpoint, but rendered escaped → non-functional)

LEGACY_FORM_DEPENDENCY =
NO

P0_ISSUES = 2
P1_ISSUES = 6
P2_ISSUES = 7

SEO_REGRESSION = 0
SCHEMA_REGRESSION = 0
URL_REGRESSION = 0

SOURCE_CHANGES = 0
UNAUTHORIZED_CHANGES = 0

CLOUDFLARE = NO
DNS = NO
WORDPRESS = NO
REDIRECTS = NO
FORM_WRITE = NO
COMMIT = NO
PUSH = NO
DEPLOY = NO

REPORT =
reports/S.6-C.4-P.4.1-commercial-conversion-audit.md

HARD STOP =
No implementation performed.
Formspree endpoint only audited as authorized target configuration.
```
