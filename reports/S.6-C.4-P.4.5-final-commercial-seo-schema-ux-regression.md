# S.6-C.4-P.4.5 — FINAL COMMERCIAL / SEO / SCHEMA / UX REGRESSION GATE

**Phase:** S.6-C.4-P.4.5 (AUDIT ONLY — `SOURCE_WRITE = NO`)
**Date:** 2026-09-06
**Author:** Regression-gate agent
**Preceding:** P.4.4 = PASS
**Driven by:** Final read-only regression gate after P.4.4 implementation.
**Mode:** AUDIT ONLY. No source / content / layout / CSS / config / CNAME / redirect / WordPress / Cloudflare / DNS change. No commit / push / deploy / Actions. No real RFQ / email. No new pages.

---

## 1. EXECUTIVE RESULT

> **PHASE_6_C.4_P.4.5 = PASS_WITH_DEFERRED_ITEMS**

Every FINAL GATE condition passes. No P0/P1 regression exists. All 41 forms render as real HTML with the correct Formspree endpoint and the P.4.4-added B2B qualification fields. 11/11 flagship LPs carry embedded RFQ + trust. A356 routing passes with editorial preserved. Trust facts remain evidence-backed (no fabrication). 2250T is **not published** (0 mentions) — and per the user's explicit clarification this phase, the largest die-casting machine is **5000T** (already evidence-backed / PASS); 2250T is therefore correctly confirmed as NOT a capability and must never be published. SEO / Schema / Media / UX / URL / leakage regressions are all 0. Build PASS. No unauthorized source change.

Deferred items (NOT failures): DRAWING_UPLOAD (DEFERRED), 2250T (resolved as DO-NOT-PUBLISH; 5000T authoritative), Large-structural LP (deferred to a future build phase), A356 dedicated commercial LP (deferred), Electronics/Industrial LPs (DO_NOT_CREATE).

---

## 2. BASELINE (§0)

| Item | Value | Note |
|------|-------|------|
| HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` | matches `BASELINE_HEAD` exactly — no HARD STOP |
| origin/main | not a resolvable local ref | expected; P.4.4 recorded `ORIGIN_MAIN: no local tracking ref; HEAD=origin baseline` |
| Tracked diff (source) | 47 files | identical to P.4.5 entry baseline (cumulative P.4.x uncommitted; **0 introduced by P.4.5**) |
| Working-tree entries | 93 | cumulative P.4.x + generated `public/`; no surprise |

No HEAD/origin difference → no HARD STOP. Working tree was **not** reset/checkedout/cleaned/stashed/restored.

---

## 3. P.4.4 EVIDENCE REVIEWED (§1)

Read actual deliverables (not summary):
- `reports/S.6-C.4-P.4.4-b2b-rfq-conversion-implementation.md` (verdict PASS, gate table, diff safety)
- `reports/S.6-C.4-P.4.4-rfq-form-implementation.csv` (41-form inventory; 11 flagship before/after)
- `reports/S.6-C.4-P.4.4-a356-intent-routing.csv` (A356 routing + editorial retained + NEW_LP_DEFERRED)
- `reports/S.6-C.4-P.4.4-capability-trust-implementation.csv` (trust/capability changes; 2250T DEFERRED)

Confirmed the 11 flagship CTA-link-only LPs upgraded by P.4.4:
`aluminum-die-casting`, `magnesium-die-casting-services`, `services`, `automotive-die-casting-parts`, `ev-battery-housing-die-casting`, `manufacturing-capabilities`, `large-scale-5000t-aluminum-die-casting-factory-china`, `porosity-control-x-ray-inspection-castings`, `precision-cnc-machining`, `die-casting-tooling`, `gravity-die-casting-manufacturer`.

---

## 4. RENDERING REGRESSION (§2, §3)

- BUILD = **PASS** (`/d/hugo/hugo --gc --minify`, exit 0; 46 HTML docs / 21 static; 52 Pages).
- ESCAPED_HTML pages = **0** (`&lt;p&gt;`, `&lt;h1`, `&lt;h2`, `&lt;strong&gt;`, `&lt;form`, `&amp;amp;` all absent).
- REAL_HTML_FORMS = **41**; ESCAPED_FORMS = **0**.
- **P0_RENDERING_REGRESSION = 0.**

---

## 5. RFQ / FORM REGRESSION (§4)

Audit of all 41 real `<form>` tags in generated HTML:
- `action=https://formspree.io/f/xpqgbdly` on **41/41** → WRONG_FORM_ENDPOINTS = **0**.
- `method=POST` on **41/41** → NON_POST_FORMS = **0**.
- Legacy WP/PHP endpoint refs = **0** (LEGACY_WP_FORMS = 0, LEGACY_PHP_FORMS = 0).

---

## 6. B2B RFQ FIELD COMPLETENESS (§5)

Core fields present on all 41 forms: Name, Company, Business Email (`type=email`), Phone, Project Details (message★). `name★/email★/message★` required; company/phone optional.

B2B qualification (optional selects, introduced by P.4.4):
- `material` present on **41/41** → MATERIAL_FIELD = PASS
- `process` present on **41/41** → PROCESS_FIELD = PASS
- `annual_volume` present on **41/41** → ANNUAL_VOLUME_FIELD = PASS

CAD upload: **no `<input type=file>` anywhere** → CAD_UPLOAD_STATUS = **DEFERRED / NOT_IMPLEMENTED** (honest; no fake upload field). Do not invent upload capability.
CORE_FIELD_COMPLETENESS = PASS; B2B_FIELDS = PASS.

---

## 7. FLAGSHIP COMMERCIAL LP RFQ COVERAGE (§6)

| LP | real `<form>` | trust block | IATF 16949 | B2B fields |
|----|----|----|----|----|
| aluminum-die-casting | ✓ | ✓ | ✓ | ✓ |
| magnesium-die-casting-services | ✓ | ✓ | ✓ | ✓ |
| services | ✓ | ✓ | ✓ | ✓ |
| automotive-die-casting-parts | ✓ | ✓ | ✓ | ✓ |
| ev-battery-housing-die-casting | ✓ | ✓ | ✓ | ✓ |
| manufacturing-capabilities | ✓ | ✓ | ✓ | ✓ |
| large-scale-5000t-aluminum-die-casting-factory-china | ✓ | ✓ | ✓ | ✓ |
| porosity-control-x-ray-inspection-castings | ✓ | ✓ | ✓ | ✓ |
| precision-cnc-machining | ✓ | ✓ | ✓ | ✓ |
| die-casting-tooling | ✓ | ✓ | ✓ | ✓ |
| gravity-die-casting-manufacturer | ✓ | ✓ | ✓ | ✓ |

FLAGSHIP_LP_EXPECTED = 11; FLAGSHIP_LP_WITH_REAL_EMBEDDED_RFQ = 11; FLAGSHIP_RFQ_COVERAGE = **PASS**. Forms are real rendered HTML (not CTA/mailto links).

---

## 8. A356 COMMERCIAL INTENT REGRESSION (§7)

`/a356-aluminum-die-casting-porosity-control/`:
- editorial article exists, NOT deleted → A356_EDITORIAL_PRESERVED = PASS
- commercial routing block "Sourcing Production-Grade A356 Die Casting" present → A356_COMMERCIAL_ROUTING = PASS
- routing targets (`/semi-solid-die-casting-heat-treatable-aluminum/`, `/aluminum-die-casting/`, `/automotive-die-casting-parts/`) all resolve to real routes → A356_TARGETS_VALID = PASS
- no broken target, no redirect loop, no canonical change, no duplicate LP
- 2250T absent on this page (confirmation of §9)

---

## 9. TRUST / CAPABILITY REGRESSION (§8, §9)

Trust block (`quality-trust.html`, rendered on 11 flagship LPs) contains all 6 required, evidence-backed topics:
`IATF 16949`, `ISO 9001`, `PPAP`, `X-Ray`, `Porosity Control`, `CMM` — all present; no fabricated cert numbers, agencies, expiry, customers, or OEM-supplier status.

Fabrication negative-scan across all generated HTML: the only `OEM`-related match is pre-existing homepage copy "global automotive Tier-1 and **OEM suppliers**" (a generic customer-segment description, NOT an invented "approved OEM supplier of [Brand]" claim). No cert numbers, no customer/OEM certifications invented.
- TRUST_FACTS_UNSUPPORTED = 0
- CERTIFICATE_NUMBERS_INVENTED = 0
- CUSTOMERS_INVENTED = 0
- OEM_CLAIMS_INVENTED = 0

Machine capability:
- 5000T claim: **PASS** (authoritative largest machine; user-confirmed this phase; 33 files reference 5000T)
- 3500T magnesium: **PASS** (re-confirmed single-source fact; not expanded)
- 2250T semi-solid: **DEFERRED_FACT_VERIFICATION** — 0 mentions in source AND 0 in generated output. Per user clarification, 2250T is NOT a capability; 5000T is the factual largest. 2250T_UNVERIFIED_PUBLISHED_MENTIONS = **0** → no HARD STOP, no P0/P1 content regression.

---

## 10. 21 YEARS / 40,000㎡ CLAIMS (§10)

- 21 years: present on About + Homepage (repo-verified fact from `automotive-die-casting-parts.md`); not altered to "22 years" → **PASS**
- 40,000㎡: present on Homepage "Manufacturing at a Glance" + About → **PASS**
- Numbers unchanged; not inferred from current calendar year.

---

## 11. SEO REGRESSION (§11)

- H1 count = exactly **1** per content page across all 46 HTML → H1_VIOLATIONS = 0
- missing title = 0; bad canonical = 0
- canonical uses `https://alumcasting.com/` on every page; staging canonical = 0; old-project-path canonical = 0
- production baseURL confirmed: `https://alumcasting.com/`
- sitemap.xml: 46 `<loc>` entries, **all** `https://alumcasting.com/...`; 0 staging/project-path/wp-content URLs
- robots.txt: `User-agent: * / Allow: /` — no staging/project-path leakage
- SEO_REGRESSION = 0 (read-only; no SEO modified)

---

## 12. COMMERCIAL SEARCH INTENT (§12)

Using P.4.3 intent map + P.4.4 evidence, commercial-intent pages classified:
- service / high-intent manufacturing / automotive / aluminum / magnesium / semi-solid / CNC-precision / medical-device commercial / A356 routing → all carry DIRECT_RFQ or COMMERCIAL_CTA (embedded form or Request-a-Quote). No EDITORIAL_ONLY dead-end on a commercial-intent service LP.
- Deferred (not blockers): large-structural (no LP yet), electronics/industrial (DO_NOT_CREATE), dedicated A356 commercial LP (routed instead).
- No P0 commercial conversion blocker.

---

## 13. GOOGLE ADS LANDING-PAGE REGRESSION (§13)

For P.4.3 Ads-ready / partial pages (service + high-intent LPs): verified visible H1, relevant commercial intent, visible conversion CTA, RFQ path, no escaped HTML, no broken links, no empty form, no editorial-only dead end. ADS_LANDING_P0_BLOCKERS = **0**. (On-site readiness only; no Google Ads approval claimed.)

---

## 14. SCHEMA REGRESSION (§14)

- JSONLD_PARSE_ERRORS = **0**
- Types intact: Organization 46, WebPage 46, BreadcrumbList 46, Article 6, FAQPage 3 (Question 15 / Answer 15)
- Organization `@id` = `https://alumcasting.com/#organization` (present in 46 pages)
- Organization `url` = `https://alumcasting.com/`
- Organization `logo` = `https://alumcasting.com/images/KingShip-Logo.webp` (46 pages)
- No staging domain, no `diecasting.github.io/alumcasting/`, no old project-path URLs in any JSON-LD
- SCHEMA_REGRESSION = 0 (regression verification only; no schema redesign)

---

## 15. MEDIA REGRESSION (§15)

- WP media references = 0; staging media references = 0; mixed (http) media = 0; broken local media = 0
- 207 `<img>` refs, **all** use local production prefix `/images/` (or `https://alumcasting.com/images/`)
- 18/18 migrated media assets present in `public/images/`
- No `/alumcasting/images/`, no `wp-content/uploads`, no external WordPress image dependency
- MEDIA_REGRESSION = 0

---

## 16. INTERNAL LINK REGRESSION (§16)

- P.4.4-introduced links (A356 routing ×3 + quality-trust → `/porosity-control-x-ray-inspection-castings/`) all resolve → P.4.4_INTRODUCED_BROKEN_LINKS = **0**
- Full-site internal-link scan: 0 broken internal links in generated output.
- No redirects implemented; no links auto-repaired in P.4.5. Migration-era deferred targets (if any) remain legitimately pending Q-phase redirect implementation — none were introduced by P.4.4.

---

## 17. UX / RESPONSIVE REGRESSION (§17)

Static presence across all 46 HTML: header 46, footer 46, nav 46, CTA 46, `main.css` linked 46, viewport meta 46. Trust block, A356 routing block, RFQ form all present and well-formed. Responsive CSS (P.4 token system, breakpoints 980/860/560) unchanged by P.4.5 (audit-only). Mobile nav (checkbox-hamburger, JS-free), form fields fit viewport, no P.4.4-caused horizontal overflow, CTA visible, trust block layout intact, footer intact. UX_REGRESSION = 0. (True pixel rendering not performed; CSS rules verified present and unchanged.)

---

## 18. ACCESSIBILITY REGRESSION (§18)

- Form labels associated with inputs/selects via `for`/`id` (verified in rendered forms).
- The only inputs without an associated `for` are the 11 hidden `_subject` Formspree fields (1 per flagship form) — hidden inputs correctly require no label.
- Button semantics (`<button type=submit>`) intact; heading hierarchy h1→h2 preserved; `aria-label` present on mobile nav toggle; focus-visible CSS already implemented in P.4.
- No unrelated accessibility issues rewritten. ACCESSIBILITY_REGRESSION = 0.

---

## 19. URL / ROUTING REGRESSION (§19)

- No new URL introduced by P.4.4 beyond existing content references (11 embedded forms + trust block are inline content, not new routes).
- No redirect changes, no canonical changes, no sitemap URL changes, no CNAME changes.
- No duplicate URLs created by P.4.4. URL_REGRESSION = 0.

---

## 20. MIGRATION SAFETY (§20)

Generated-output leakage scan (excludes historical audit-report text):
- `diecasting.github.io/alumcasting` = 0
- `/alumcasting/images/` = 0
- `wp-content` = 0
- staging domain = 0
- old project-path = 0
- PRODUCTION_URL_LEAKAGE = 0; STAGING_LEAKAGE = 0; WP_MEDIA_LEAKAGE = 0; OLD_PROJECT_PATH_LEAKAGE = 0

---

## 21. GIT SAFETY (§21)

End-of-audit re-check:
- `git rev-parse HEAD` = `cbc5101587de947cac2337377a8cfb49d938212e` (unchanged)
- origin/main unchanged (not a local ref; consistent with baseline)
- Tracked source diff = 47 files (identical to P.4.5 entry; **0 new source changes by this audit**)
- Generated `public/` (untracked) was rebuilt for verification and preserved; the required report (this file) added as untracked artifact and preserved.
- HEAD_UNCHANGED = YES; ORIGIN_UNCHANGED = YES; NO_NEW_SOURCE_CHANGES = YES.

---

## 22. FINAL GATE MATRIX (§22)

| CHECK | EXPECTED | ACTUAL | STATUS | EVIDENCE |
|-------|----------|--------|--------|----------|
| P0_RENDERING_REGRESSION | 0 | 0 | PASS | escaped_html pages=0; real forms=41; escaped=0 |
| ESCAPED_HTML | 0 | 0 | PASS | no `&lt;p&gt;`/`&lt;form` etc. |
| REAL_FORMS | 41 | 41 | PASS | `<form ` count across 46 HTML |
| ESCAPED_FORMS | 0 | 0 | PASS | `&lt;form` count=0 |
| WRONG_FORM_ENDPOINTS | 0 | 0 | PASS | all `formspree.io/f/xpqgbdly` |
| NON_POST_FORMS | 0 | 0 | PASS | all `method=POST` |
| CORE_RFQ_FIELDS | PASS | PASS | PASS | name/company/email/phone/message present |
| B2B_FIELDS | PASS | PASS | PASS | material/process/annual_volume 41/41 |
| FLAGSHIP_RFQ_COVERAGE | 11/11 | 11/11 | PASS | 11 LPs have real form+trust |
| A356_ROUTING | PASS | PASS | PASS | routing block + editorial kept + targets valid |
| TRUST_FACT_REGRESSION | 0 | 0 | PASS | 6 topics present; 0 fabricated |
| 5000T | PASS | PASS | PASS | 33 files; user-confirmed largest |
| 3500T | PASS/DEFERRED | PASS | PASS | re-confirmed single source |
| 2250T | DEFERRED_FACT_VERIFICATION | DEFERRED (0 published) | PASS | 0 src + 0 output; user: largest=5000T |
| 2250T_PUBLISHED_UNVERIFIED | 0 | 0 | PASS | grep 2250 = 0 everywhere |
| 21_YEARS | PASS/DEFERRED | PASS | PASS | About+Home present |
| 40000_SQM | PASS/DEFERRED | PASS | PASS | Home+About present |
| SEO_REGRESSION | 0 | 0 | PASS | H1=1; canonical=alumcasting.com; sitemap 46 prod |
| SCHEMA_REGRESSION | 0 | 0 | PASS | JSONLD errors=0; Org/WebPage/FAQ/Article intact |
| MEDIA_REGRESSION | 0 | 0 | PASS | 207 imgs local /images/; 18/18 assets |
| P.4.4_BROKEN_LINK_REGRESSION | 0 | 0 | PASS | P.4.4-introduced broken=0; site=0 |
| UX_REGRESSION | 0 | 0 | PASS | header/footer/nav/cta/css/vp=46 |
| ACCESSIBILITY_REGRESSION | 0 | 0 | PASS | labels assoc; hidden _subject only |
| URL_REGRESSION | 0 | 0 | PASS | no new/duplicate URL |
| STAGING_LEAKAGE | 0 | 0 | PASS | 0 diecasting.github.io etc. |
| WP_MEDIA_LEAKAGE | 0 | 0 | PASS | 0 wp-content |
| BUILD | PASS | PASS | PASS | hugo --gc --minify exit 0 |
| UNAUTHORIZED_SOURCE_CHANGES | 0 | 0 | PASS | tracked diff unchanged (47) |

---

## 23. DEFERRED ITEMS (§23)

- **DRAWING_UPLOAD** = DEFERRED (Formspree plan upload capability unverifiable; no fake field). Honest `mailto:` CAD path on homepage.
- **2250T semi-solid** = DO-NOT-PUBLISH (confirmed NOT a capability; 5000T is the authoritative largest per user 2026-09-06). Never publish 2250T.
- **Large-structural LP** = deferred to a future build phase (only after verified structural facts).
- **A356 dedicated commercial LP** = deferred (routed to existing pages instead of thin page).
- **Electronics / Industrial LPs** = DO_NOT_CREATE (thin-page risk).
- No new URL, no Google Ads campaign, no Analytics, no conversion tracking, no Cloudflare/DNS/redirect/WordPress/production deploy.

These deferred items are **explicitly approved** and do NOT downgrade the verdict.

---

## 24. EXACT NEXT PHASE (§23, §25)

> **PHASE_6_C.4_P.4.5 = PASS_WITH_DEFERRED_ITEMS**

Next phase (only if proceeding):
**NEXT = Q — 301 REDIRECT IMPLEMENTATION** (migration-era deferred targets / legacy URL cutover; requires production WordPress + redirect write authority — out of scope for this audit-only phase).

Do NOT proceed to Q until/unless the user authorizes redirect implementation.

---

*HARD STOP: no commit, no push, no deploy, no DNS, no Cloudflare, no WordPress, no 301 redirects, no source change. Report only.*
