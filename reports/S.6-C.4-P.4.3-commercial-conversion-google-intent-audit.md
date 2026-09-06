# S.6-C.4-P.4.3 — B2B RFQ Conversion Optimization & Google Intent Landing Page Audit

**Phase:** S.6-C.4-P.4.3 (audit-first; default `SOURCE_WRITE = NO`)
**Date:** 2026-09-06
**Author:** Audit agent (read-only within authorized scope)
**Preceding phases:** P.4 (UX/UI) = PASS, P.4.1 (commercial audit) = BLOCKED, P.4.2 (render+RFQ recovery) = PASS

---

## 1. BASELINE (STEP 0)

| Item | Value |
|------|-------|
| HEAD | `cbc5101587de947cac2337a8cfb49d938212e` |
| ORIGIN/MAIN | `cbc5101587de947cac2337a8cfb49d938212e` (ls-remote) |
| Tracked modified files (pre-existing P.3/P.4/P.4.2 baseline) | 46 |
| Untracked (P.3 new pages, P.4 partials, reports, static images) | 37 (+3 this phase = 40) |
| Content `.md` | 44 |
| Rendered HTML docs | 46 |
| Static assets | 21 |
| Hugo build | PASS (exit 0, ~159 ms) |

**Phase-write policy honored:** `git diff --name-only` remains **46** (identical to baseline). The only new files this phase are the 3 untracked report CSVs + this report. **No content/layout/partial/CSS/Formspree/config file was modified.** `SOURCE_CHANGES = 0`, `UNAUTHORIZED_CHANGES = 0`.

---

## 2. EXECUTIVE VERDICT

> **PHASE_6_C.4_P.4.3 = CONDITIONALLY_READY**

**Rationale:** The P.4.1 P0 blockers (rendering + RFQ form) were resolved in P.4.2. The site now renders correctly, 30 real RFQ forms exist (`action=https://formspree.io/f/xpqgbdly`, `method=POST`), and the SEO/Schema/URL layers are intact. The majority of high-value commercial intents (aluminum/magnesium die casting, A380/A383/ADC12/AM60B/AZ91D materials, automotive/EV-battery/medical industries, cold-chamber, 5000T, porosity/X-ray) **are served by real, well-targeted landing pages**.

However, material conversion gaps remain (P1), but **no P0 blocker**:
- The **flagship service LPs** (`/aluminum-die-casting/`, `/magnesium-die-casting-services/`, `/services/`, `/manufacturing-capabilities/`, `/die-casting-tooling/`, `/precision-cnc-machining/`, `/automotive-die-casting-parts/`, `/ev-battery-housing-die-casting/`, `/large-scale-5000t-.../`) have **no embedded RFQ form** — only a CTA link to `/contact/`. This forces a high-value buyer to navigate away to submit.
- The RFQ form **lacks B2B qualification fields** (material, process, annual volume, drawing upload) and **file upload is NOT implemented**.
- Trust facts are **inconsistently distributed**: `2250T` semi-solid = **0 source mentions** (cannot claim), `3500T magnesium` / `21 years` / `40,000㎡` appear on only 1–2 pages (WEAK).
- Two commercial intents are **unserved** (`large structural`, `PPAP`) and two are only **editorially covered** (`A356` lands on "Battle on the Shop Floor"; `electronics`/`industrial` have no dedicated LP).

These are P1/P2 optimizations, not P0. Per the phase write-policy they are **recorded and deferred**, not implemented here.

---

## 3. COMMERCIAL INVENTORY (STEP 1)

44 content pages + homepage classified:

- **Core Service (11):** aluminum-die-casting, magnesium-die-casting-services, services, cold-chamber-die-casting-services, gravity-die-casting-manufacturer, sand-casting-services, zinc-die-casting-services, 380/383 aluminum service, adc12, semi-solid
- **Industry (9):** automotive-die-casting-parts, precision-die-casting-medical-equipment (P.3), ev-battery-housing, custom-ev-powertrain, electric-motor-housing, liquid-cooled-cooling-plates, explosion-proof-oil-gas, automotive-cnc-list, high-tolerance-automotive-cnc
- **Capability/Technology (11):** manufacturing-capabilities, large-scale-5000t, maximum-wall-thickness-5000t, porosity-control-xray, high-pressure-die-casting-process-quality, vacuum-assisted-hpdc, thin-wall-die-casting-tooling, precision-cnc-machining, die-casting-tooling, stainless-steel-machining, surface-finishing
- **Material/Comparison articles (10):** a356-porosity, a356-semi-solid-guide, am60b, az91d, aluminum-to-magnesium (×2), thixocasting-vs-rheocasting, hot-chamber-vs-cold-chamber-myth (P.3), medical-device-component-machining
- **RFQ/Contact (1):** contact
- **About (1):** about-alumcasting-die-casting-expert

---

## 4. GOOGLE INTENT MAP (STEP 2) — 38 intents

Full mapping in `S.6-C.4-P.4.3-google-intent-map.csv`.

| Metric | Count |
|--------|-------|
| Total intents mapped | 38 |
| Match status EXACT | 21 |
| Match status GOOD_MATCH | 8 |
| Match status PARTIAL_MATCH | 6 |
| Match status MISSING_LANDING_PAGE | 3 |
| ADS_READY = YES | 29 |
| ADS_READY = PARTIAL | 4 |
| ADS_READY = NO | 5 |
| ORGANIC_READY = YES | 31 |
| ORGANIC_READY = PARTIAL | 4 |
| ORGANIC_READY = NO | 3 |

**Coverage conclusion:** Core commercial-manufacturer, material, and most industry intents are well covered. The 3 `MISSING_LANDING_PAGE` intents are `large structural die casting`, `PPAP die casting supplier`, `electronics die casting` (and `2250T semi-solid` under capability).

---

## 5. GOOGLE ADS LANDING PAGE AUDIT (STEP 3)

Scored against the 12-point Ads-readiness checklist (H1 clarity, hero message, capability/trust proof, primary+secondary CTA, mobile CTA, message/intent match).

- **ADS_READY = YES (29):** all dedicated service/material/industry LPs with commercial H1s and clear proof (e.g. `/aluminum-die-casting/` H1 "Aluminum Die Casting Manufacturer | Precision HPDC…", `/automotive-die-casting-parts/` H1 "Automotive Aluminum Die Casting Parts Manufacturer…").
- **ADS_PARTIAL (4):** `/high-pressure-die-casting-process-quality/` (process/quality, not service LP), `/iatf-16949-...-cnc-machining-supplier/` (CNC-focused, not die casting — IATF proof on die-casting LPs is thin), `/aluminum-die-casting-process/`, `/explosion-proof-...-oil-gas/` (only niche industrial).
- **ADS_NOT_READY (5):** `large structural`, `electronics`, `PPAP` (no LP); `/a356-...-porosity-control/` (editorial H1 "Battle on the Shop Floor" — **intent mismatch** for a commercial "A356 die casting" query); `/vacuum-assisted-...-comparison/` (comparison article).

**Key Ads risk:** do **not** point commercial "A356 die casting" or "die casting process" Ads at the editorial/comparison articles — they would waste spend and hurt Quality Score.

---

## 6. ORGANIC COMMERCIAL INTENT (STEP 4)

Intent classification across pages:
- **TRANSACTIONAL / B2B SUPPLIER** (strong): service & material LPs (`/aluminum-die-casting/`, `/magnesium-die-casting-services/`, `/380-.../`, `/adc12-.../`, `/automotive-die-casting-parts/`, `/precision-die-casting-medical-equipment/`).
- **COMMERCIAL INVESTIGATION** (good): capability/quality pages (`/manufacturing-capabilities/`, `/porosity-control-xray/`, `/large-scale-5000t-.../`).
- **INFORMATIONAL** (correctly so): comparison/myth articles (`/thixocasting-vs-rheocasting/`, `/hot-chamber-vs-cold-chamber-.../`, `/vacuum-assisted-.../`). These should **not** be forced as commercial LPs.

No page currently presents a "supplier/manufacturer" query with purely educational content **except** the A356 porosity article (flagged P1 above).

---

## 7. RFQ FORM AUDIT (STEP 5) — P.4.2 recovery confirmed intact

| Metric | Value |
|--------|-------|
| TOTAL_FORMS (expected, pages embedding shortcode + contact) | 30 |
| REAL_HTML_FORMS | 30 |
| ESCAPED_FORMS | 0 |
| LEGACY_WP_FORMS | 0 |
| LEGACY_PHP_FORMS | 0 |
| WRONG_ENDPOINTS | 0 |
| FORM_ENDPOINT | `https://formspree.io/f/xpqgbdly` |
| FORM_METHOD | POST |

All 30 forms render as real `<form method=POST action="https://formspree.io/f/xpqgbdly">`. No WordPress/PHP/legacy/staging endpoint anywhere. P.4.2 recovery is **stable** under this audit.

---

## 8. B2B RFQ FIELD GAP (STEP 6)

Authoritative form fields (from rendered `/contact/`): `name*` (required), `email*` (required), `company`, `phone`, `message*` (required), plus hidden `_subject` and a consent checkbox.

| Group | Fields present | Status |
|-------|----------------|--------|
| CORE_RFQ_FIELDS | name, company, email, phone, message | ✅ sufficient for first-touch |
| OPTIONAL_TECHNICAL_FIELDS | material, process, annual volume, part/application | ❌ all missing |
| DRAWING / CAD / PDF UPLOAD | — | ❌ missing |

**FILE_UPLOAD_STATUS = NOT_IMPLEMENTED** — no `<input type=file>`, no `enctype="multipart/form-data"`. Formspree can accept attachments, but the current static form provides no upload field. Per phase rule, **no third-party upload provider is introduced here** → `NEEDS_PROVIDER_REVIEW` (deferred).

**MISSING_HIGH_VALUE_FIELDS:** material, manufacturing process, estimated annual volume, part/application, CAD/drawing upload. For B2B engineering qualification these are high-value but **not** required to clear P0 (form submits fine).

---

## 9. RFQ FRICTION AUDIT (STEP 7)

- Form is short (5 fields, 3 required) → **low first-submit friction**. Good.
- **Friction source = navigation, not the form:** 16 of 46 pages (incl. all flagship service LPs) have **no embedded form**, only a CTA link to `/contact/`. A buyer on `/aluminum-die-casting/` must click → navigate → find form. This is the dominant P1 friction.
- No duplicate/homepage-CTA spam; CTA wording is consistent ("Request a Quote" / "Contact Us").
- Trust gap: form has no visible privacy notice / data-handling statement (minor P2).

**Target state:** enough fields to qualify + low friction. Recommended (deferred): add an embedded RFQ form (or sticky CTA→/contact/) to the flagship service LPs, and later add material/volume/drawing fields behind a sensible step.

---

## 10. COMMERCIAL TRUST SIGNAL AUDIT (STEP 8)

Searched all 44 content sources + rendered HTML. Status per verified fact:

| Fact | Source pages | Rendered-visible pages | Status |
|------|--------------|------------------------|--------|
| 5000T aluminum HPDC | 22 | 33 | ✅ PRESENT (strong) |
| IATF 16949 | 28 | 35 | ✅ PRESENT |
| ISO 9001 | 21 | 24 | ✅ PRESENT |
| X-ray inspection | 36 | 39 | ✅ PRESENT |
| Porosity control | 37 | 40 | ✅ PRESENT |
| CNC / secondary | 40 | — | ✅ PRESENT |
| ±0.01mm tolerance | 10 | 10 | ✅ PRESENT (ok) |
| PPAP | 6 | 6 | ⚠️ WEAK (low frequency, no dedicated proof) |
| 3500T magnesium | 1 | 1 | ⚠️ WEAK (single page) |
| 40,000㎡ facility | 2 | 2 | ⚠️ WEAK (2 pages) |
| 21 years | 1 | 1 | ⚠️ WEAK (single page; also homepage H1 is keyword-stuffed, not brand-trust) |
| 2250T semi-solid | **0** | 0 | ❌ MISSING (cannot claim) |

**No fabricated facts introduced.** The weak/missing items are distribution problems (proof exists but on too few pages) or verification gaps (2250T absent from repo entirely).

---

## 11. 2250T SEMI-SOLID GAP (STEP 9)

`2250T` appears on **0 source pages**. The P.4.1/P.4.3 spec lists "up to 2250T semi-solid" as a target capability, but **no verified repository evidence supports it**. Per the hard rule "绝对禁止创造不存在的事实", this is recorded as:

> **DEFERRED_FACT_VERIFICATION** — do NOT publish 2250T semi-solid claims until the client/engineering confirms machine specs. Until then, semi-solid is represented by the existing `630T–1600T SSM` / `up to 2250T` ranges only where already verified, and the 2250T-specific claim is withheld.

---

## 12. 21 YEARS / COMPANY IDENTITY (STEP 10)

- `Kingship (Dongguan) Precision Manufacturing Co., Ltd.` appears in 19 source pages; `Kingship` brand token in 2.
- Schema `Organization` `@id = https://alumcasting.com/#organization` on all 46 pages (verified, unchanged).
- **Inconsistency (P2):** the **homepage H1** reads "Aluminum Die Casting Supplier | HPDC & CNC Machining" — a keyword string, not a brand/trust statement. "21 years" appears on only 1 page. Recommend (deferred) a homepage H1 that leads with brand + trust ("AlumCasting — Kingship Precision | 21-Year Aluminum HPDC Manufacturer") and surfacing "21 years / 40,000㎡" on homepage + key service LPs.
- No `KingShip`/`Kingship`/`AlumCasting` brand confusion detected in rendered output; logo filename `KingShip-Logo` is correct per brand rules.

---

## 13. LANDING PAGE GAP ANALYSIS (STEP 11)

Full matrix in `S.6-C.4-P.4.3-landing-page-gap-matrix.csv`.

| Candidate intent | Current | Recommendation | Priority | Rationale |
|------------------|---------|----------------|----------|-----------|
| Large structural die casting | PARTIAL (5000T page) | RECOMMENDED_NEW_LP — **after** verifying structural-part facts | P1 | High-value; 5000T real but "structural" framing unproven |
| 2250T semi-solid | MISSING (0 facts) | DEFERRED_FACT_VERIFICATION | P1 | Cannot claim without verified specs |
| A356 commercial | PARTIAL (editorial only) | CREATE_COMMERCIAL_LP or fix H1 | P1 | Commercial query lands on editorial H1 |
| PPAP die casting | MISSING_LP | ADD_PPAP_PROOF_BLOCK (not thin LP) | P2 | Strengthen proof on existing LPs |
| Electronics die casting | MISSING | DO_NOT_CREATE | P2 | No dedicated facts → thin-page/spam risk |
| Industrial die casting | PARTIAL (oil&gas only) | DO_NOT_CREATE / EXPAND | P2 | Avoid thin page; expand existing proof |

**No thin pages were created in this phase.** Where facts are insufficient, the recommendation is `DO_NOT_CREATE` / `DEFERRED`.

---

## 14. CTA ARCHITECTURE (STEP 12)

- **Header CTA:** present ("Request a Quote" → /contact/) on all pages via `site-header.html`.
- **Hero CTA:** present on homepage + service LPs (P.4 system).
- **Mid/bottom CTA:** `cta-band.html` renders a "Request a Quote" band; `related-cards.html` provides capability cross-links.
- **Mobile CTA:** CSS-only hamburger (no JS), CTA remains tappable.
- **Path:** Google → Landing Page → Technical proof (capability/trust blocks) → RFQ CTA → Formspree form. **Intact** for the 30 form-embedded pages; **broken by navigation** for the 16 form-less pages (CTA link only).

Unified primary CTA = **REQUEST A QUOTE**; secondary = VIEW CAPABILITIES / CONTACT US. Consistent.

---

## 15. INTERNAL LINKING FOR CONVERSION (STEP 13)

- Material → Process → Capability → Industry → RFQ cross-links exist in most content (e.g. `internal_commercial_links` counts of 8–20 on many pages per P.4.1 matrix).
- **Risk:** several article/comparison pages link heavily to other blog-style content; ensure commercial LPs are the conversion target, not terminal blog posts. No SEO architecture damage observed (canonical/structure intact).
- **Recommendation (deferred):** on flagship service LPs, add explicit "Related Industry" + "Request a Quote" cross-links near proof blocks to shorten the buyer path.

---

## 16. GOOGLE ADS NEGATIVE / WRONG-INTENT (STEP 14)

Pages that should **NOT** be Ads landing pages (use as organic/supporting only):
- `/a356-aluminum-die-casting-porosity-control/` (editorial H1) → would mismatch "A356 die casting" Ads.
- `/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/` (comparison).
- `/thixocasting-vs-rheocasting-comparison/`, `/hot-chamber-vs-cold-chamber-...-myth/` (educational).
- `/aluminum-to-magnesium-conversion-*` (awareness).

No Google Ads account was touched (no permission); findings are recommendations only.

---

## 17. SEO / SCHEMA / URL / MEDIA SAFETY (STEP 15–16)

| Check | Result |
|-------|--------|
| H1 = 1 / page | ✅ 0 violations (46/46) |
| Title present | ✅ all 44 content + homepage |
| Meta description | ✅ all content/homepage (taxonomy auto-pages excepted, pre-existing) |
| Canonical | ✅ all `https://alumcasting.com/...` |
| Robots | ✅ present |
| Sitemap | ✅ valid |
| JSON-LD parse errors | ✅ 0 |
| Schema types | Organization 46, WebPage 46, Article 6, FAQPage 3 |
| Organization @id | ✅ unchanged |
| diecasting.github.io | ✅ 0 |
| wp-content | ✅ 0 |
| /alumcasting/ | ✅ 0 |
| Media (207 img refs + 5 JSON-LD images) | ✅ 0 broken |

**H1_REGRESSION = 0, SEO_REGRESSION = 0, SCHEMA_REGRESSION = 0, URL_REGRESSION = 0, MEDIA_REGRESSION = 0.**

---

## 18. UX/UI COMMERCIAL CHECK (STEP 17)

P.4 system (header/nav/hero/cards/CTA/footer/responsive) is sufficient to support conversion. No P0 UX defect found. Minor P2 notes (deferred): homepage H1 is keyword-stuffed rather than trust-led; consideration of a sticky mobile RFQ CTA. **No CSS/large UI rewrite performed** (would violate scope).

---

## 19. CONVERSION PRIORITY MATRIX (STEP 18)

### P0 — Conversion blockers
**None.** RFQ renders and submits; CTAs present site-wide.

### P1 — Materially reduces commercial performance
| Issue | Page / Intent | Business impact | Recommended action | Phase |
|-------|---------------|-----------------|--------------------|-------|
| Flagship service LPs have no embedded RFQ form (CTA link only) | /aluminum-die-casting/, /magnesium-die-casting-services/, /services/, /manufacturing-capabilities/, /die-casting-tooling/, /precision-cnc-machining/, /automotive-die-casting-parts/, /ev-battery-housing-die-casting/, /large-scale-5000t-.../ | High-value buyers must navigate away to convert → lost RFQ | Embed RFQ form (or sticky CTA→/contact/) on flagship LPs | IMPLEMENTATION |
| "A356 die casting" commercial intent lands on editorial H1 | /a356-aluminum-die-casting-porosity-control/ | Ads/organic mismatch for commercial query | Create dedicated A356 commercial LP or reframe H1 | IMPLEMENTATION |
| IATF 16949 proof weak on die-casting LPs (only CNC page dedicated) | die-casting service LPs | Weak trust for automotive buyers | Add IATF 16949 proof block to service LPs | IMPLEMENTATION |
| 2250T semi-solid claim unverified | site-wide | Cannot publish; risk of fabricated claim | DEFERRED_FACT_VERIFICATION with client | DEFERRED |
| Large structural die casting has no LP | "large structural die casting" | High-value intent unserved | Build LP after verifying structural facts | IMPLEMENTATION (post-verify) |

### P2 — Optimization opportunity
| Issue | Recommendation | Phase |
|-------|----------------|-------|
| RFQ form lacks material/volume/drawing fields + file upload | Add B2B qualification fields + `enctype=multipart` file field (provider review) | IMPLEMENTATION |
| 3500T magnesium / 40,000㎡ / 21 years on only 1–2 pages | Distribute verified trust facts to homepage + key LPs | IMPLEMENTATION |
| Homepage H1 keyword-stuffed, not brand-trust | Lead homepage H1 with brand + "21 years" trust | IMPLEMENTATION |
| PPAP proof low-frequency | Add PPAP proof block to service LPs | IMPLEMENTATION |
| Electronics / industrial intent only partially served | DO_NOT_CREATE thin pages; expand existing proof if facts arise | DEFERRED |
| Form lacks privacy notice | Add short data-handling statement | IMPLEMENTATION |

### P3 — Nice-to-have
Dedicated LPs for A380/A383/ADC12/AM60B/AZ91D/SSM/thin-wall/sand/zinc already exist and are healthy — no action.

---

## 20. FINAL RECOMMENDATION (STEP 20)

```
COMMERCIAL_INTENT_COVERAGE = 35/38 mapped intents served (EXACT/GOOD/PARTIAL); 3 MISSING_LP
ADS_READY_PAGES           = 29 (YES) + 4 (PARTIAL) = 33/38
ORGANIC_COMMERCIAL_PAGES  = 31 (YES) + 4 (PARTIAL) = 35/38
RFQ_COVERAGE              = 30 real forms + /contact/ (site-wide CTA); 16 flagship LPs form-less (P1)
RFQ_FIELD_COMPLETENESS    = CORE complete; technical/material/volume/upload MISSING (P2)
TRUST_SIGNAL_COVERAGE     = Strong (5000T/IATF/ISO/X-ray/porosity); WEAK (3500T/40k㎡/21yr); MISSING (2250T)
MISSING_LANDING_PAGES     = large structural, PPAP, electronics (DO_NOT_CREATE), 2250T (verify)
P0_CONVERSION_BLOCKERS    = 0
P1_CONVERSION_GAPS        = 5
P2_CONVERSION_GAPS        = 6
```

**Verdict: CONDITIONALLY_READY** — core RFQ is functional and the main commercial intents are well served, but flagship LPs need embedded forms + B2B fields, and a few high-value intents/trust facts require deferred verification or expansion. No P0 remains.

---

## 21. DELIVERABLES (STEP 21)

- `reports/S.6-C.4-P.4.3-commercial-conversion-google-intent-audit.md` (this file)
- `reports/S.6-C.4-P.4.3-google-intent-map.csv` (38 intents)
- `reports/S.6-C.4-P.4.3-rfq-conversion-audit.csv` (28 key pages)
- `reports/S.6-C.4-P.4.3-landing-page-gap-matrix.csv` (6 candidates)

All are **audit artifacts only**. No source file was modified.

---

## 22. DEFERRED TO IMPLEMENTATION PHASE

- Embed RFQ form on 9 flagship service LPs (P1)
- A356 commercial LP / H1 reframe (P1)
- IATF 16949 + PPAP proof blocks on service LPs (P1/P2)
- 2250T semi-solid fact verification (P1, blocks any claim)
- Large structural LP (post-verification) (P1)
- B2B RFQ field expansion + CAD/PDF upload (provider review) (P2)
- Homepage H1 brand-trust reframe + distribute 21yr/40k㎡ (P2)
- DO_NOT_CREATE electronics/industrial thin pages (P2)

---

*HARD STOP: audit-only phase. WordPress = NO, Cloudflare = NO, DNS = NO, GitHub Pages settings = NO, redirects = NO, production deploy = NO, commit = NO, push = NO, actions rerun = NO, SOURCE_WRITE = NO (0 tracked-source changes).*
