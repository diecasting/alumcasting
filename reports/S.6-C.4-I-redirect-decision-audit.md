# AlumCasting Migration — Pre-Cutover URL Redirect Decision Audit (S.6-C.4-I)

**MODE: STRICT READ-ONLY / NO MUTATION / NO PRODUCTION CHANGE**

> READ-ONLY AUDIT · NO PRODUCTION CHANGES · NO REDIRECTS MODIFIED · NO SOURCE CHANGES · NO COMMIT · NO PUSH

## 1. Executive Summary

This phase resolves the 49 unresolved WordPress URLs identified by `reports/MIGRATION-PRE-CUTOVER-AUDIT.md`
(97 WP sitemap URLs minus 48 verified Hugo equivalents = 49 gap). Every URL received exactly one defensible disposition.

**Decision totals (49 URLs):**

- `301_SEMANTIC_REMAP` = **29**
- `HUMAN_REVIEW` = **10**
- `410_OR_404_NO_EQUIVALENT` = **2**
- `SYSTEM_URL_NO_REDIRECT` = **8**
- **Total = 49**

**Confidence:** HIGH = 16, MEDIUM = 23, LOW = 10.

Of the 41 content URLs (posts/pages), **29** have a single clear topical Hugo owner and should 301 to it;
**10** are genuinely ambiguous (multiple plausible owners) and require human review; **2** are true orphans (404).
The **8** taxonomy/archive URLs are system URLs with no redirect.

**URL_REDIRECT_DECISION_STATUS = CONDITIONALLY_READY** — most URLs are defensible, but 10 HUMAN_REVIEW items remain.

## 2. Baseline Verification

- Expected baseline HEAD: `cbc5101587de947cac2337377a8cfb49d938212e`
- Verified current HEAD: `cbc5101587de947cac2337377a8cfb49d938212e` (MATCH)
- Branch: `main`
- `origin/main`: `cbc5101587de947cac2337377a8cfb49d938212e` (MATCH)
- Working tree: only the pre-existing untracked `reports/MIGRATION-PRE-CUTOVER-AUDIT.md` from the prior phase;
  no tracked source/config files modified by this phase.

## 3. Source Inventory

- WordPress sitemap URLs (Yoast, 4 child maps): **97**
  - posts = 45, pages = 44, categories = 4, tags = 4
- Hugo content files: **42** markdown + homepage = **44 published URLs**
- Hugo-equivalent URLs (from prior audit): **48** (A=42 exact + B=6 remap/alias)
- Unresolved URLs (this audit): **49** (C=41 content + F=8 taxonomy)

## 4. WP → Hugo Coverage Reconciliation

| Bucket | Count |
| --- | ---: |
| Total WP sitemap URLs | 97 |
| Exact Hugo equivalents (A) | 42 |
| Existing semantic remaps/aliases (B) | 6 |
| Unresolved content, no equiv (C) | 41 |
| Taxonomy/system (F) | 8 |
| **Unresolved subtotal** | **49** |

Independently reconstructed count = **49** (matches the prior audit; no forced adjustment).

## 5. Complete 49-URL Decision Matrix

Columns: # | Old WP URL | Type | WP Status | WP Title | WP H1 | WP Canonical | Hugo Candidate (Target) | Decision | Confidence | Evidence | Notes

| # | Old WP URL | Type | WP St | WP Title | H1 | WP Canonical | Target | Decision | Conf | Evidence | Notes |
| --: | --- | --- | --: | --- | --: | --- | --- | --- | --- | --- | --- |
| 1 | `/5-methods-eliminate-porosity-aluminum-pressure-die-casting/` | post | 200 | How to Eliminate Porosity in Aluminum Pres | 2 | `/5-methods-eliminate-porosity-aluminum-pressure-die-casting/` | `/porosity-control-x-ray-inspection-castings/` | 301_SEMANTIC_REMAP | HIGH | WP post on eliminating porosity. Hugo /porosity-control-x-ray-inspection-castings/ is the direct porosity owner (RULE 2). | - |
| 2 | `/a356-aluminum-die-casting-alloy-properties/` | post | 200 | A356 Aluminum Die Casting Alloy Properties | 1 | `/a356-aluminum-die-casting-alloy-properties/` | `/a356-aluminum-die-casting-porosity-control/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on A356 alloy properties. Hugo /a356-aluminum-die-casting-porosity-control/ owns A356 topic (RULE 2). | Porosity page is the A356 hub; acceptable remap. |
| 3 | `/a380-aluminum-die-casting-alloy-properties/` | post | 200 | A380 Aluminum Alloy: Properties & Manufact | 2 | `/a380-aluminum-die-casting-alloy-properties/` | `/380-aluminum-die-casting-service/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on A380 alloy properties. Hugo /380-aluminum-die-casting-service/ owns A380 topic (RULE 2). | - |
| 4 | `/aluminum-alloy-adc12-properties-engineering-guide/` | post | 200 | Aluminum Alloy ADC12 Properties / KingShip | 1 | `/aluminum-alloy-adc12-properties-engineering-guide/` | `/adc12-die-casting-cnc-machining/` | 301_SEMANTIC_REMAP | MEDIUM | WP post 'ADC12 properties engineering guide'. Hugo owns ADC12 topic via /adc12-die-casting-cnc-machining/ (ADC12 Die Casting & CNC Machining). Single clear topical owner (RULE 2). | Verify body depth vs service page before wiring 301. |
| 5 | `/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/` | post | 200 | AM60B vs AZ91D Magnesium Elongation & Cras | 2 | `/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/` | `-` | HUMAN_REVIEW | LOW | WP comparison AM60B vs AZ91D (magnesium alloys). Plausible owners: /am60b-magnesium-alloy-die-casting-suppliers/ OR /az91d-magnesium-die-casting-automotive-parts/ (RULE 7). | Human picks correct successor. |
| 6 | `/benefits-of-permanent-mold-casting-for-structural-integrity/` | post | 200 | Structural Integrity & Benefits of Permane | 2 | `/benefits-of-permanent-mold-casting-for-structural-integrity/` | `/gravity-die-casting-manufacturer/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on permanent-mold benefits. Permanent mold = gravity die casting; Hugo /gravity-die-casting-manufacturer/ owns it (RULE 2). | - |
| 7 | `/bridge-tooling-low-volume-aluminum-die-casting-guide/` | post | 200 | Bridge Tooling for Die Casting: Low Volume | 2 | `/bridge-tooling-low-volume-aluminum-die-casting-guide/` | `/die-casting-tooling/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on bridge tooling / low-volume. Hugo /die-casting-tooling/ owns tooling subject (RULE 2). | - |
| 8 | `/case-studies/` | page | 200 | Aluminum Die Casting Case Studies / Automo | 1 | `/case-studies/` | `-` | 410_OR_404_NO_EQUIVALENT | HIGH | WP 'case studies' page has NO Hugo equivalent (no case-studies hub in repo). No legitimate target (RULE 1/3/9). | Recommend ordinary 404; if case studies are valuable, author a Hugo page post-cutover (P2). |
| 9 | `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | post | 200 | Chinese Aluminum Casting Grade Equivalents | 2 | `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | `-` | HUMAN_REVIEW | LOW | WP post on Chinese grade equivalents A380/A356/ADC12. Three plausible alloy-page owners (RULE 7). | Human picks one or accepts 404. |
| 10 | `/cost-down-dfm-design-aluminum-die-casting-molds/` | post | 200 | How DFM Analysis Can Reduce Your Die Casti | 2 | `/cost-down-dfm-design-aluminum-die-casting-molds/` | `/die-casting-tooling/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on DFM / mold design for cost-down. Hugo /die-casting-tooling/ owns tooling+DFM subject (RULE 2). | - |
| 11 | `/custom-casting-ev-battery-housing-prototypes/` | post | 200 | EV Battery Housing Prototypes / Custom Die | 2 | `/custom-casting-ev-battery-housing-prototypes/` | `/ev-battery-housing-die-casting/` | 301_SEMANTIC_REMAP | HIGH | WP post 'EV Battery Housing Prototypes / Custom Die Casting Expert'. Hugo /ev-battery-housing-die-casting/ is the direct EV-battery-housing service owner (title match 'High-Precision EV Battery Housing Die Casting'). Single clear successor (RULE 2). | Strong subject match; wire 301 at cutover. |
| 12 | `/die-casting-defects-solutions-pro-guide/` | post | 200 | Die Casting Defects and Solutions / KingSh | 2 | `/die-casting-defects-solutions-pro-guide/` | `/high-pressure-die-casting-process-quality/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on die-casting defects/solutions. Hugo /high-pressure-die-casting-process-quality/ owns defects/quality subject (RULE 2). | /porosity-control-x-ray-inspection-castings/ also related. |
| 13 | `/die-casting-machine-tonnage-calculation-formula/` | post | 200 | Die Casting Machine Tonnage Calculation Fo | 2 | `/die-casting-machine-tonnage-calculation-formula/` | `/maximum-wall-thickness-projected-area-limits-5000t-die-casting/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on machine tonnage calc. Hugo /maximum-wall-thickness-projected-area-limits-5000t-die-casting/ owns tonnage/5000T limits (RULE 2). | - |
| 14 | `/ev-battery-housing-die-casting-design-prototyping-guide/` | post | 200 | EV Battery Housing Die Casting Design / Ki | 2 | `/ev-battery-housing-die-casting-design-prototyping-guide/` | `/ev-battery-housing-die-casting/` | 301_SEMANTIC_REMAP | HIGH | WP post on EV-battery-housing design/prototyping. Hugo /ev-battery-housing-die-casting/ is the direct owner (RULE 2). | - |
| 15 | `/forging-casting-vs-cnc-manufacturing-guide/` | post | 200 | Forging vs Casting vs CNC: Master the Manu | 2 | `/forging-casting-vs-cnc-manufacturing-guide/` | `-` | HUMAN_REVIEW | LOW | WP comparison of forging/casting vs CNC. Plausible owners: /precision-cnc-machining/ OR /aluminum-die-casting/ (RULE 7). | Human picks correct successor. |
| 16 | `/gravity-die-casting-step-by-step-guide-pro/` | post | 200 | Gravity Die Casting Process Guide / KingSh | 2 | `/gravity-die-casting-step-by-step-guide-pro/` | `/gravity-die-casting-manufacturer/` | 301_SEMANTIC_REMAP | HIGH | WP post 'gravity die casting step-by-step guide'. Hugo /gravity-die-casting-manufacturer/ is the direct gravity-die-casting owner (RULE 2). | - |
| 17 | `/guide-to-types-of-metal-casting-processes/` | post | 200 | Types of Metal Casting Processes | 2 | `/guide-to-types-of-metal-casting-processes/` | `-` | HUMAN_REVIEW | LOW | WP post surveying many casting processes (sand, gravity, HPDC, investment, etc.). Multiple Hugo process pages are plausible owners; no single obvious successor (RULE 7). | Human must pick one hub or accept 404. Do NOT auto-redirect to an arbitrary process page. |
| 18 | `/how-dfm-analysis-reduces-die-casting-costs/` | post | 200 | How DFM Analysis Cuts Die Casting Costs by | 2 | `/how-dfm-analysis-reduces-die-casting-costs/` | `/die-casting-tooling/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on DFM cost analysis. Hugo /die-casting-tooling/ owns DFM/tooling (RULE 2). | - |
| 19 | `/investment-casting-vs-die-casting-selection-guide/` | post | 200 | Investment Casting vs Die Casting / KingSh | 2 | `/investment-casting-vs-die-casting-selection-guide/` | `/aluminum-die-casting/` | 301_SEMANTIC_REMAP | MEDIUM | WP comparison investment casting vs die casting. Hugo /aluminum-die-casting/ owns the die-casting side (investment casting absent from Hugo) (RULE 2). | - |
| 20 | `/iso-14001-high-pressure-aluminium-die-casting-manufacturer/` | page | 200 | ISO 14001 High Pressure Aluminium Die Cast | 0 | `/iso-14001-high-pressure-aluminium-die-casting-manufacturer/` | `/high-pressure-die-casting-process-quality/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on ISO-14001 HPDC manufacturer. Hugo /high-pressure-die-casting-process-quality/ owns HPDC subject (RULE 2). | - |
| 21 | `/low-pressure-vs-high-pressure-die-casting-comparison/` | post | 200 | LPDC vs HPDC: Choose the Right Die Casting | 2 | `/low-pressure-vs-high-pressure-die-casting-comparison/` | `/high-pressure-die-casting-process-quality/` | 301_SEMANTIC_REMAP | MEDIUM | WP post comparing low vs high pressure die casting. Hugo /high-pressure-die-casting-process-quality/ owns HPDC subject (RULE 2). | - |
| 22 | `/machining-allowance-optimization-aluminum-casting-porosity/` | post | 200 | Prevent Subsurface Porosity: Machining All | 2 | `/machining-allowance-optimization-aluminum-casting-porosity/` | `-` | HUMAN_REVIEW | LOW | WP post on machining allowance + porosity. Plausible owners: /porosity-control-x-ray-inspection-castings/ OR /precision-cnc-machining/ (RULE 7). | Human picks correct successor. |
| 23 | `/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/` | post | 200 | Magnesium AZ91D vs Aluminum ADC12 for Ligh | 2 | `/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/` | `-` | HUMAN_REVIEW | LOW | WP comparison magnesium AZ91D vs aluminum ADC12. Plausible owners: /magnesium-die-casting-services/ OR /adc12-die-casting-cnc-machining/ (RULE 7). | Human picks correct successor. |
| 24 | `/magnesium-die-casting-corrosion-protection-mao-coating/` | post | 200 | Magnesium Die Casting MAO Coating & Corros | 2 | `/magnesium-die-casting-corrosion-protection-mao-coating/` | `/magnesium-die-casting-services/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on magnesium MAO coating / corrosion protection. Hugo /magnesium-die-casting-services/ owns magnesium topic (RULE 2). | Sub-topic of magnesium hub; acceptable remap. |
| 25 | `/magnesium-vs-aluminum-die-casting/` | post | 200 | Magnesium vs. Aluminum Die Casting | 2 | `/magnesium-vs-aluminum-die-casting/` | `-` | HUMAN_REVIEW | LOW | WP comparison 'Magnesium vs Aluminum Die Casting'. Plausible owners: /magnesium-die-casting-services/ OR /aluminum-die-casting/ (RULE 7). | Human picks correct successor. |
| 26 | `/metal-stamping-vs-die-casting-cost-design-guide/` | post | 200 | Metal Stamping vs Die Casting / KingShip | 2 | `/metal-stamping-vs-die-casting-cost-design-guide/` | `-` | HUMAN_REVIEW | LOW | WP comparison stamping vs die casting. Plausible owners: /aluminum-die-casting/ OR /precision-cnc-machining/ (RULE 7). | Human picks correct successor. |
| 27 | `/minimum-draft-angle-for-aluminium-die-casting-dfm-guide/` | post | 200 | Minimum Draft Angle for Aluminium Die Cast | 2 | `/minimum-draft-angle-for-aluminium-die-casting-dfm-guide/` | `/die-casting-tooling/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on draft-angle DFM. Hugo /die-casting-tooling/ owns tooling+DFM (RULE 2). | - |
| 28 | `/permanent-mold-vs-die-casting-expert-comparison/` | post | 200 | Permanent Mold vs Die Casting / KingShip | 2 | `/permanent-mold-vs-die-casting-expert-comparison/` | `/gravity-die-casting-manufacturer/` | 301_SEMANTIC_REMAP | MEDIUM | WP comparison permanent-mold vs die casting. Permanent mold = gravity die casting; Hugo /gravity-die-casting-manufacturer/ owns it (RULE 2). | - |
| 29 | `/pore-free-die-casting-weldable-automotive-structural-parts/` | post | 200 | Pore-Free Die Casting for Weldable Parts / | 2 | `/pore-free-die-casting-weldable-automotive-structural-parts/` | `-` | HUMAN_REVIEW | LOW | WP post on pore-free weldable automotive structural parts. Plausible owners: /porosity-control-x-ray-inspection-castings/ OR /automotive-die-casting-parts/ (RULE 7). | Human picks correct successor. |
| 30 | `/post-casting-treatments-finishing-options-aluminum/` | post | 200 | Guide to Post-Casting Treatments & Aluminu | 2 | `/post-casting-treatments-finishing-options-aluminum/` | `/surface-finishing-aluminum-magnesium-casting/` | 301_SEMANTIC_REMAP | HIGH | WP post on post-casting finishing. Hugo /surface-finishing-aluminum-magnesium-casting/ owns finishing subject (RULE 2). | - |
| 31 | `/ppap-level-3-documentation-for-die-casting-iatf-guide/` | post | 200 | PPAP Level 3 Documentation for Die Casting | 2 | `/ppap-level-3-documentation-for-die-casting-iatf-guide/` | `/iatf-16949-high-tolerance-automotive-cnc-machining-supplier/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on PPAP/IATF documentation. Hugo /iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ owns IATF/PPAP subject (RULE 2). | - |
| 32 | `/prevent-blistering-aluminum-t6-heat-treatment/` | post | 200 | How to Prevent Blistering in T6 Heat-Treat | 2 | `/prevent-blistering-aluminum-t6-heat-treatment/` | `-` | 410_OR_404_NO_EQUIVALENT | HIGH | WP post 'How to Prevent Blistering in T6 Heat-Treated Aluminum' is live (200) but has NO Hugo equivalent. The SSM heat-treatable owner is the consolidated SSM page, not blistering-specific. No legitimate target exists (RULE 1/3/9). | OUT-OF-SCOPE legacy dead link carried from prior phases. Recommend ordinary 404 (no rewrite planned). Do NOT map to heat-treatable owner (different subject). |
| 33 | `/rheocasting-vs-conventional-hpdc-cost-analysis/` | post | 200 | Rheocasting vs HPDC Cost: The Real Manufac | 2 | `/rheocasting-vs-conventional-hpdc-cost-analysis/` | `/thixocasting-vs-rheocasting-comparison/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on rheocasting vs HPDC cost. Hugo /thixocasting-vs-rheocasting-comparison/ owns the rheocasting topic (RULE 2). | - |
| 34 | `/sand-casting-process-guide/` | post | 200 | Complete Sand Casting Process Guide | 2 | `/sand-casting-process-guide/` | `/sand-casting-services/` | 301_SEMANTIC_REMAP | MEDIUM | WP post 'sand casting process guide'. Hugo /sand-casting-services/ is the authoritative sand-casting hub (RULE 2). | - |
| 35 | `/scaling-automotive-cnc-machining-production/` | post | 200 | Scaling Automotive Production: 400+ CNC Un | 2 | `/scaling-automotive-cnc-machining-production/` | `/high-tolerance-automotive-cnc-machining/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on scaling automotive CNC production. Hugo /high-tolerance-automotive-cnc-machining/ owns automotive CNC scaling (RULE 2). | /automotive-cnc-machining-equipment-list/ also related. |
| 36 | `/scaling-die-casting-production-t0-to-10000-units/` | post | 200 | Scaling Die Casting Production: From T0 Pr | 2 | `/scaling-die-casting-production-t0-to-10000-units/` | `/large-scale-5000t-aluminum-die-casting-factory-china/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on scaling die-casting output to 10,000 units. Hugo /large-scale-5000t-aluminum-die-casting-factory-china/ owns large-scale production capacity (RULE 2). | /manufacturing-capabilities/ also related; 5000T page is the specific owner. |
| 37 | `/small-batch-die-casting-cnc-finishing-guide/` | post | 200 | Small Batch Die Casting with Precision CNC | 2 | `/small-batch-die-casting-cnc-finishing-guide/` | `/precision-cnc-machining/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on small-batch die casting + CNC finishing. Hugo /precision-cnc-machining/ owns CNC finishing (RULE 2). | /die-casting-tooling/ also related; CNC finishing is the differentiator. |
| 38 | `/t6-heat-treatment-semi-solid-die-casting-aluminum/` | post | 200 | T6 Heat Treatment for Semi-Solid Die Casti | 2 | `/t6-heat-treatment-semi-solid-die-casting-aluminum/` | `/semi-solid-die-casting-heat-treatable-aluminum/` | 301_SEMANTIC_REMAP | HIGH | Per project memory + prior-phase decision this dead-T6 post consolidates into the SSM T6 heat-treatable owner /semi-solid-die-casting-heat-treatable-aluminum/. WP is live (200, self-canonical 'T6 Heat Treatment for Semi-Solid Die Casting'). Clear content-ownership equivalence (RULE 1/2). | User-mandated owner; must NOT be left unresolved. Existing G-phase content links already point here. |
| 39 | `/ultimate-aluminum-die-casting-design-guide-expert-tips/` | post | 200 | Aluminum Die Casting Design Guide | 2 | `/ultimate-aluminum-die-casting-design-guide-expert-tips/` | `/aluminum-die-casting/` | 301_SEMANTIC_REMAP | MEDIUM | WP general aluminum die-casting design guide. Hugo /aluminum-die-casting/ is the top-level aluminum hub (RULE 2). | Generic hub remap; acceptable. |
| 40 | `/why-we-recommended-a356-over-adc12-high-stress-structural-parts/` | post | 200 | A356 vs ADC12 for High-Stress Structural A | 2 | `/why-we-recommended-a356-over-adc12-high-stress-structural-parts/` | `-` | HUMAN_REVIEW | LOW | WP post comparing A356 vs ADC12 for structural parts. Plausible owners: /a356-aluminum-die-casting-porosity-control/ OR /adc12-die-casting-cnc-machining/ (RULE 7). | Human picks correct successor. |
| 41 | `/zamak-3-vs-zamak-5-die-casting-selection-guide/` | post | 200 | Zamak 3 vs. Zamak 5: Zinc Die Casting Sele | 2 | `/zamak-3-vs-zamak-5-die-casting-selection-guide/` | `/zinc-die-casting-services/` | 301_SEMANTIC_REMAP | MEDIUM | WP post on Zamak-3 vs Zamak-5 (zinc alloys). Hugo /zinc-die-casting-services/ owns zinc/Zamak topic (RULE 2). | - |
| 42 | `/category/casting-industry-applications/` | category | 200 | Die Casting Applications / Automotive, EV, | 1 | `/category/casting-industry-applications/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP category archive. Taxonomy/system URL; per RULE 4/6 do not redirect to a service page. | Return 404 or soft no-index archive. |
| 43 | `/category/manufacturing-capabilities/` | category | 200 | Manufacturing Capabilities / Alumcasting | 1 | `/category/manufacturing-capabilities/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP category archive. Taxonomy/system URL (RULE 4/6). | - |
| 44 | `/category/quality-control-testing/` | category | 200 | Rigorous Die Casting Quality Control / IAT | 1 | `/category/quality-control-testing/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP category archive. Taxonomy/system URL (RULE 4/6). | - |
| 45 | `/category/semi-solid-die-casting/` | category | 200 | Semi-Solid Die Casting (SSM) Solutions / H | 1 | `/category/semi-solid-die-casting/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP category archive (semi-solid). Note: also covered by redirect 3169 chain at post level; archive itself = system URL (RULE 4). | - |
| 46 | `/tag/iatf-16949-certified-die-casting/` | post_tag | 200 | IATF 16949 Certified Die Casting Factory / | 1 | `/tag/iatf-16949-certified-die-casting/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP tag archive. System URL (RULE 4/6). | - |
| 47 | `/tag/magnesium-die-casting-services/` | post_tag | 200 | High-Precision Magnesium Die Casting / Lig | 1 | `/tag/magnesium-die-casting-services/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP tag archive. System URL (RULE 4/6). | - |
| 48 | `/tag/rheocasting-process/` | post_tag | 200 | Rheocasting Technology for High-Strength A | 1 | `/tag/rheocasting-process/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP tag archive. System URL (RULE 4/6). | - |
| 49 | `/tag/vacuum-assisted-die-casting/` | post_tag | 200 | Vacuum Die Casting for Porosity-Free Compo | 1 | `/tag/vacuum-assisted-die-casting/` | `-` | SYSTEM_URL_NO_REDIRECT | HIGH | WP tag archive. System URL (RULE 4/6). | - |

## 6. Existing Redirect Evidence

- **Redirection rule 3169** (WP-side, NOT modified): `/semi-solid-die-casting-manufacturers/`
  → 301 → `/semi-solid-die-casting-heat-treatable-aluminum/`. Live WP confirms 301 to the SSM owner.
  This alias is already in the 48 resolved set (B); at cutover it must 301 to the same Hugo owner.
- **Semi-solid chain** (intentionally untouched): `/semi-solid-die-casting/` → (WP) the heat-treatable owner.
  The generic SSM page was deleted in Hugo; its owner is `/semi-solid-die-casting-heat-treatable-aluminum/`.
- **Mandated legacy remaps** (project memory, already applied in content):
  `/casting-industry-applications/` → `/automotive-die-casting-parts/`; `/die-casting-mold-design/` → `/die-casting-tooling/`.
- **Hugo aliases**: none of the 49 targets rely on Hugo `aliases` front matter; all targets are real published Hugo paths.

## 7. Special Cases

- **Dead T6 `/t6-heat-treatment-semi-solid-die-casting-aluminum/`** — live WP post (200, self-canonical).
  Per user instruction + project memory its owner is `/semi-solid-die-casting-heat-treatable-aluminum/`.
  Disposition = `301_SEMANTIC_REMAP` (HIGH). It is NOT left unresolved; G-phase content links already point here.
- **`/prevent-blistering-aluminum-t6-heat-treatment/`** — live WP post (200) with NO Hugo equivalent.
  Carried as OUT-OF-SCOPE legacy dead link from prior phases. Disposition = `410_OR_404_NO_EQUIVALENT` (404).
  Do NOT map to the heat-treatable owner (different subject: blistering prevention vs SSM T6).

## 8. Human Review Items

**10 URLs require human decision** (two+ plausible Hugo owners, RULE 7):

- `/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/` — WP comparison AM60B vs AZ91D (magnesium alloys). Plausible owners: /am60b-magnesium-alloy-die-casting-suppliers/ OR /az91d-magnesium-die-casting-automotive-parts/ (RULE 7).
- `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` — WP post on Chinese grade equivalents A380/A356/ADC12. Three plausible alloy-page owners (RULE 7).
- `/forging-casting-vs-cnc-manufacturing-guide/` — WP comparison of forging/casting vs CNC. Plausible owners: /precision-cnc-machining/ OR /aluminum-die-casting/ (RULE 7).
- `/guide-to-types-of-metal-casting-processes/` — WP post surveying many casting processes (sand, gravity, HPDC, investment, etc.). Multiple Hugo process pages are plausible owners; no single obvious successor (RULE 7).
- `/machining-allowance-optimization-aluminum-casting-porosity/` — WP post on machining allowance + porosity. Plausible owners: /porosity-control-x-ray-inspection-castings/ OR /precision-cnc-machining/ (RULE 7).
- `/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/` — WP comparison magnesium AZ91D vs aluminum ADC12. Plausible owners: /magnesium-die-casting-services/ OR /adc12-die-casting-cnc-machining/ (RULE 7).
- `/magnesium-vs-aluminum-die-casting/` — WP comparison 'Magnesium vs Aluminum Die Casting'. Plausible owners: /magnesium-die-casting-services/ OR /aluminum-die-casting/ (RULE 7).
- `/metal-stamping-vs-die-casting-cost-design-guide/` — WP comparison stamping vs die casting. Plausible owners: /aluminum-die-casting/ OR /precision-cnc-machining/ (RULE 7).
- `/pore-free-die-casting-weldable-automotive-structural-parts/` — WP post on pore-free weldable automotive structural parts. Plausible owners: /porosity-control-x-ray-inspection-castings/ OR /automotive-die-casting-parts/ (RULE 7).
- `/why-we-recommended-a356-over-adc12-high-stress-structural-parts/` — WP post comparing A356 vs ADC12 for structural parts. Plausible owners: /a356-aluminum-die-casting-porosity-control/ OR /adc12-die-casting-cnc-machining/ (RULE 7).

## 9. Decision Counts

- 301_SEMANTIC_REMAP = 29
- HUMAN_REVIEW = 10
- 410_OR_404_NO_EQUIVALENT = 2
- SYSTEM_URL_NO_REDIRECT = 8
- **Total = 49**

Confidence: HIGH=16, MEDIUM=23, LOW=10.

## 10. Migration Readiness

**URL_REDIRECT_DECISION_STATUS = CONDITIONALLY_READY**

Rationale: 36 of 49 URLs (26 remap + 8 system + 2 no-equiv) have defensible, evidence-backed dispositions.
13 HUMAN_REVIEW items remain ambiguous and must be resolved by a human before the final 301 map is wired.
No URL was assigned an invented or staging target (RULE 9/10 honored).

**TOP MIGRATION BLOCKERS / OPEN ITEMS**

1. 10 HUMAN_REVIEW URLs need a human-chosen Hugo owner (see §8).
2. The 26 `301_SEMANTIC_REMAP` targets must be validated for body-subject parity before wiring.
3. Cutover still blocked by the P0 canonical/baseURL flip (staging `github.io` → production `alumcasting.com`)
   from the prior pre-cutover audit — out of scope here but prerequisite.

## 11. Explicit Out-of-Scope Items

- Redirection rule 3169 (WP-side) — read-only evidence only; NOT modified.
- `/prevent-blistering-aluminum-t6-heat-treatment/` — out-of-scope legacy dead link; reported only.
- All Hugo source, layouts, config, baseURL, CNAME, GitHub Pages, Actions, WordPress, Cloudflare, DNS — NOT touched.
- No commits, no pushes, no redirects created or modified.

## 12. HARD STOP

This was a READ-ONLY decision audit. No production changes were made. The only file created is this report,
which remains UNCOMMITTED per the phase rules. Await explicit authorization before any redirect implementation,
commit, or deployment.

