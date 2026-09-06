# AlumCasting Migration — Redirect Implementation Package (PREPARATION ONLY)

**Phase:** S.6-C.4-L — Final Redirect Implementation Readiness Audit
**Mode:** STRICT READ-ONLY / NO MUTATION
**Status:** PREPARATION ARTIFACT ONLY. No WordPress, Cloudflare, DNS, Hugo source, GitHub Pages, or redirect changes were made. Nothing was imported.

> This document is the **import-ready package**. It is NOT imported during this phase. The CSV
> (`MIGRATION-REDIRECT-IMPLEMENTATION.csv`) contains exactly 35 rows. The execution order and
> prerequisites below are documentation only.

---

## 0. Authoritative Baseline

| Item | Value |
|---|---|
| Repo HEAD (frozen K baseline) | `cbc5101587de947cac2337377a8cfb49d938212e` |
| Final decision matrix (frozen S.6-C.4-K) | 35 `301_SEMANTIC_REMAP` + 2 `410_OR_404` + 8 `SYSTEM` + 4 `HUMAN_REVIEW` = **49** |
| This phase mutated | only 3 report files (this + CSV + L report), all uncommitted |
| WordPress / Cloudflare / DNS / Pages / Actions / Hugo source | **NOT touched** |

**Do NOT reopen or reinterpret the 4 HUMAN_REVIEW decisions.** They remain `HUMAN_REVIEW`.

---

## 1. 35 Approved 301 Redirects (the import set)

Format mirrors `MIGRATION-REDIRECT-IMPLEMENTATION.csv`: `source,target,status,regex`.
Source is the legacy WordPress path; target is the **production-absolute** Hugo URL
(`https://alumcasting.com/<slug>/`). `status=301`, `regex=0` for every row.

| # | Source (legacy WP) | Target (Hugo, production) | Conf | Reason (RULE) |
|--:|---|---|---|---|
| 1 | `/5-methods-eliminate-porosity-aluminum-pressure-die-casting/` | `https://alumcasting.com/porosity-control-x-ray-inspection-castings/` | HIGH | RULE 2 porosity owner |
| 2 | `/a356-aluminum-die-casting-alloy-properties/` | `https://alumcasting.com/a356-aluminum-die-casting-porosity-control/` | MED | RULE 2 A356 hub |
| 3 | `/a380-aluminum-die-casting-alloy-properties/` | `https://alumcasting.com/380-aluminum-die-casting-service/` | MED | RULE 2 A380 owner |
| 4 | `/aluminum-alloy-adc12-properties-engineering-guide/` | `https://alumcasting.com/adc12-die-casting-cnc-machining/` | MED | RULE 2 ADC12 owner |
| 5 | `/benefits-of-permanent-mold-casting-for-structural-integrity/` | `https://alumcasting.com/gravity-die-casting-manufacturer/` | MED | RULE 2 permanent-mold = gravity |
| 6 | `/bridge-tooling-low-volume-aluminum-die-casting-guide/` | `https://alumcasting.com/die-casting-tooling/` | MED | RULE 2 tooling |
| 7 | `/cost-down-dfm-design-aluminum-die-casting-molds/` | `https://alumcasting.com/die-casting-tooling/` | MED | RULE 2 DFM/tooling |
| 8 | `/custom-casting-ev-battery-housing-prototypes/` | `https://alumcasting.com/ev-battery-housing-die-casting/` | HIGH | RULE 2 EV-battery owner |
| 9 | `/die-casting-defects-solutions-pro-guide/` | `https://alumcasting.com/high-pressure-die-casting-process-quality/` | MED | RULE 2 defects/quality |
| 10 | `/die-casting-machine-tonnage-calculation-formula/` | `https://alumcasting.com/maximum-wall-thickness-projected-area-limits-5000t-die-casting/` | MED | RULE 2 tonnage/5000T |
| 11 | `/ev-battery-housing-die-casting-design-prototyping-guide/` | `https://alumcasting.com/ev-battery-housing-die-casting/` | HIGH | RULE 2 EV-battery owner |
| 12 | `/gravity-die-casting-step-by-step-guide-pro/` | `https://alumcasting.com/gravity-die-casting-manufacturer/` | HIGH | RULE 2 gravity owner |
| 13 | `/how-dfm-analysis-reduces-die-casting-costs/` | `https://alumcasting.com/die-casting-tooling/` | MED | RULE 2 DFM/tooling |
| 14 | `/investment-casting-vs-die-casting-selection-guide/` | `https://alumcasting.com/aluminum-die-casting/` | MED | RULE 2 die-casting side |
| 15 | `/iso-14001-high-pressure-aluminium-die-casting-manufacturer/` | `https://alumcasting.com/high-pressure-die-casting-process-quality/` | MED | RULE 2 HPDC |
| 16 | `/low-pressure-vs-high-pressure-die-casting-comparison/` | `https://alumcasting.com/high-pressure-die-casting-process-quality/` | MED | RULE 2 HPDC |
| 17 | `/magnesium-die-casting-corrosion-protection-mao-coating/` | `https://alumcasting.com/magnesium-die-casting-services/` | MED | RULE 2 magnesium topic |
| 18 | `/minimum-draft-angle-for-aluminium-die-casting-dfm-guide/` | `https://alumcasting.com/die-casting-tooling/` | MED | RULE 2 DFM/tooling |
| 19 | `/permanent-mold-vs-die-casting-expert-comparison/` | `https://alumcasting.com/gravity-die-casting-manufacturer/` | MED | RULE 2 permanent-mold = gravity |
| 20 | `/post-casting-treatments-finishing-options-aluminum/` | `https://alumcasting.com/surface-finishing-aluminum-magnesium-casting/` | HIGH | RULE 2 finishing |
| 21 | `/ppap-level-3-documentation-for-die-casting-iatf-guide/` | `https://alumcasting.com/iatf-16949-high-tolerance-automotive-cnc-machining-supplier/` | MED | RULE 2 IATF/PPAP |
| 22 | `/rheocasting-vs-conventional-hpdc-cost-analysis/` | `https://alumcasting.com/thixocasting-vs-rheocasting-comparison/` | MED | RULE 2 rheocasting |
| 23 | `/sand-casting-process-guide/` | `https://alumcasting.com/sand-casting-services/` | MED | RULE 2 sand hub |
| 24 | `/scaling-automotive-cnc-machining-production/` | `https://alumcasting.com/high-tolerance-automotive-cnc-machining/` | MED | RULE 2 automotive CNC |
| 25 | `/scaling-die-casting-production-t0-to-10000-units/` | `https://alumcasting.com/large-scale-5000t-aluminum-die-casting-factory-china/` | MED | RULE 2 large-scale cap |
| 26 | `/small-batch-die-casting-cnc-finishing-guide/` | `https://alumcasting.com/precision-cnc-machining/` | MED | RULE 2 CNC finishing |
| 27 | `/t6-heat-treatment-semi-solid-die-casting-aluminum/` | `https://alumcasting.com/semi-solid-die-casting-heat-treatable-aluminum/` | HIGH | RULE 1/2 SSM T6 owner (user-mandated) |
| 28 | `/ultimate-aluminum-die-casting-design-guide-expert-tips/` | `https://alumcasting.com/aluminum-die-casting/` | MED | RULE 2 aluminum hub |
| 29 | `/zamak-3-vs-zamak-5-die-casting-selection-guide/` | `https://alumcasting.com/zinc-die-casting-services/` | MED | RULE 2 zinc/Zamak |
| 30 | `/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/` | `https://alumcasting.com/am60b-magnesium-alloy-die-casting-suppliers/` | MED | J#1 AM60B crashworthiness owner |
| 31 | `/machining-allowance-optimization-aluminum-casting-porosity/` | `https://alumcasting.com/porosity-control-x-ray-inspection-castings/` | HIGH | J#5 porosity owner |
| 32 | `/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/` | `https://alumcasting.com/magnesium-die-casting-services/` | MED | J#6 magnesium lightweight |
| 33 | `/metal-stamping-vs-die-casting-cost-design-guide/` | `https://alumcasting.com/aluminum-die-casting/` | MED | J#8 die-casting owner |
| 34 | `/pore-free-die-casting-weldable-automotive-structural-parts/` | `https://alumcasting.com/automotive-die-casting-parts/` | HIGH | J#9 automotive low-porosity |
| 35 | `/why-we-recommended-a356-over-adc12-high-stress-structural-parts/` | `https://alumcasting.com/a356-aluminum-die-casting-porosity-control/` | HIGH | J#10 A356 recommendation |

**Multi-source → single-target consolidation (expected, not a defect):**
`/porosity-control-x-ray-inspection-castings/` ← sources 1 & 31;
`/gravity-die-casting-manufacturer/` ← 5, 12, 19;
`/die-casting-tooling/` ← 6, 7, 13, 18;
`/ev-battery-housing-die-casting/` ← 8, 11;
`/high-pressure-die-casting-process-quality/` ← 9, 15, 16;
`/aluminum-die-casting/` ← 14, 28, 33;
`/magnesium-die-casting-services/` ← 17, 32.
Each consolidated Hub page has a **single, self-referential canonical**; consolidation is correct redirect behavior.

---

## 2. 2 NO-EQUIVALENT URLs (recommend 404, no rewrite)

| Source (legacy WP) | Decision | Notes |
|---|---|---|
| `/case-studies/` | `410_OR_404` | No Hugo equivalent; recommend ordinary **404**. If valuable, author a Hugo page post-cutover (P2). Do **not** redirect to a vaguely related commercial page. |
| `/prevent-blistering-aluminum-t6-heat-treatment/` | `410_OR_404` | Live WP 200 but no Hugo equivalent; OUT-OF-SCOPE legacy dead link. Recommend ordinary **404**. Do **not** map to the SSM heat-treatable owner (different subject: blistering prevention vs SSM T6). |

---

## 3. 8 SYSTEM URLs (NO redirect — taxonomy/archive)

| Source (legacy WP) | Type | Exclusion reason |
|---|---|---|
| `/category/casting-industry-applications/` | category | taxonomy/system archive — RULE 4/6 |
| `/category/manufacturing-capabilities/` | category | taxonomy/system archive — RULE 4/6 |
| `/category/quality-control-testing/` | category | taxonomy/system archive — RULE 4/6 |
| `/category/semi-solid-die-casting/` | category | taxonomy/system archive — RULE 4/6 |
| `/tag/iatf-16949-certified-die-casting/` | tag | system archive — RULE 4/6 |
| `/tag/magnesium-die-casting-services/` | tag | system archive — RULE 4/6 |
| `/tag/rheocasting-process/` | tag | system archive — RULE 4/6 |
| `/tag/vacuum-assisted-die-casting/` | tag | system archive — RULE 4/6 |

No 301, no forced target, no invented Hugo replacement.

---

## 4. 4 HUMAN_REVIEW URLs (FROZEN — no target, no 410 conversion)

| Source (legacy WP) | Status | Why frozen |
|---|---|---|
| `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | `HUMAN_REVIEW` | 3-way alloy comparison; no surviving comparison hub; business must choose (recommend `/380-aluminum-die-casting-service/` or 404). |
| `/forging-casting-vs-cnc-manufacturing-guide/` | `HUMAN_REVIEW` | 3-process selection; site offers casting + CNC but not forging; no hub; business must choose. |
| `/guide-to-types-of-metal-casting-processes/` | `HUMAN_REVIEW` | process overview; no overview hub; business must choose 410 vs map to `/aluminum-die-casting/`. |
| `/magnesium-vs-aluminum-die-casting/` | `HUMAN_REVIEW` | 2-material comparison; no comparison hub; business must choose. |

These remain **un-redirected** at cutover (they will naturally 404 on the new origin until a business decision is made). Acceptable gap; no forced 301.

---

## 5. Target Validation Summary (all 35)

Validated against the built Hugo `public/` artifact AND live GitHub Pages (`diecasting.github.io/alumcasting`):

- **Built (public/):** 35 / 35 present.
- **Live HTTP:** 35 / 35 return **200**.
- **H1:** 35 / 35 have exactly **1** `<h1>`.
- **Robots:** 35 / 35 are **indexable** (no `noindex` meta).
- **Self-redirect:** 35 / 35 are **leaf pages** (no meta-refresh, no internal 301).
- **Canonical:** 35 / 35 are **self-canonical** (canonical points to their own slug — no target points to a wrong/different page).
- **Staging host in canonical:** All 35 currently carry `diecasting.github.io/alumcasting/...` as the canonical **host**. This is the **site-wide P0 canonical/baseURL leak** from the pre-cutover audit, resolved at cutover **step 1** (flip `baseURL` → `https://alumcasting.com/` + production CNAME). It is a **cutover prerequisite**, NOT a per-target defect. No `TARGET_BAD_CANONICAL` / `TARGET_STAGING` flag is raised against the target *paths* (they are production-relative Hugo paths).
- **Conclusion:** `Target validation = PASS` (0 404, 0 noindex, 0 bad canonical, 0 self-redirect).

---

## 6. Redirect Chain Safety

- **35 sources — current WP state (from `wp_crawl.jsonl`, 97-URL crawl):** all return **`status=200, redirect=False`**. None is already part of an existing redirect chain. → Proposed rule adds a clean **`301 → 200`** (1 hop, no loop) for every source.
- **Proposed targets — all leaf pages** (verified in §5), so no `301 → 301` loop is created.
- **Semi-solid consolidation interaction:**
  - Pre-existing WP rule **3169** `/semi-solid-die-casting-manufacturers/` → 301 → `/semi-solid-die-casting-heat-treatable-aluminum/` (OBSERVED_REDIRECT from prior phases; `CONFIGURATION_CONFIRMED = NO`).
  - Pre-existing WP `/semi-solid-die-casting/` → 301 → `/semi-solid-die-casting-heat-treatable-aluminum/` (OBSERVED_REDIRECT).
  - Proposed rule **#27** `/t6-heat-treatment-semi-solid-die-casting-aluminum/` → `/semi-solid-die-casting-heat-treatable-aluminum/`.
  - **Assessment:** three *distinct* sources converge on the **same leaf target**. This is benign consolidation, **not** a conflict or loop (target is a real, non-redirecting Hugo page). `EXISTING_REDIRECT = YES` for the 3169/SSM sources, but those sources are **not** among our 35, so there is **no conflict** and **no wrong-target**.
- **Conclusion:** `Redirect chain safety = PASS`.

---

## 7. WordPress Redirect Conflict Check

- `REDIRECTION_PLUGIN_VISIBILITY = INCONCLUSIVE`. The Redirection plugin REST API
  (`/wp-json/redirection/v1/redirect`) returned **401/403** (auth/WAF) on live probe this phase;
  its full rule set cannot be read. No POST/PUT/PATCH/DELETE was attempted.
- **Observed behavior** (prior-phase live evidence, status unchanged, no writes): 3169 and the SSM
  generic URL are 301 redirects to the heat-treatable owner. This is recorded as **OBSERVED_REDIRECT**,
  explicitly **distinguished from CONFIGURATION_CONFIRMED**.
- **No conflict:** the 35 proposed sources are currently served at **200 (not redirecting)** on WP,
  so no existing WP rule matches them. The only shared element (target) is a leaf page, so there is
  no rule-on-rule conflict.
- **Important implementation caveat (prerequisite):** if the 35 rules are imported only into the
  **WordPress Redirection plugin**, they will stop firing once DNS/Cloudflare routes `alumcasting.com`
  to GitHub Pages (WordPress would no longer be the request handler). **Recommendation:** implement
  the 35 rules at the **durable layer** — Cloudflare Single/Bulk Redirects (or Transform Rules) —
  which persist across the DNS cutover. The WordPress plugin may also hold them pre-cutover for
  verification, but Cloudflare is the authoritative post-cutover enforcement point.

---

## 8. Implementation Prerequisites (must be satisfied BEFORE import)

1. **P0 — baseURL flip.** Hugo `baseURL` → `https://alumcasting.com/` and production CNAME ready
   (resolves the staging-host canonical leak for all 44 pages, including the 35 targets).
2. **Asset migration (P1).** 163 `wp-content` media references (158 `<img>` + 5 Article JSON-LD
   images) remain WP-host-dependent → `ASSET_MIGRATION_REQUIRED = YES`. Host in `static/`/CDN or
   keep WP media live post-cutover. Do **not** alter image URLs in this phase.
3. **Hugo pages/assets ready** — all 35 targets already built and live (verified).
4. **Durable redirect layer** — Cloudflare redirects configured (see §7 caveat) so rules survive
   the DNS cutover.
5. **4 HUMAN_REVIEW + 2 NO_EQUIVALENT** — left as-is; revisit only after explicit business decision.

---

## 9. Exact Recommended Execution Order (documentation only — NOT executed)

1. Ensure Hugo production `baseURL = https://alumcasting.com/` (P0).
2. Ensure production CNAME / custom domain is ready and DNS target prepared.
3. Ensure all required Hugo pages/assets are ready (✅ 35 targets verified this phase).
4. Migrate `wp-content` media into `static/` or CDN (P1) — `ASSET_MIGRATION_REQUIRED`.
5. Prepare/import the 35 rules at the **durable layer** (Cloudflare Single/Bulk Redirects; optionally
   also WordPress Redirection for pre-cutover verification). Use `MIGRATION-REDIRECT-IMPLEMENTATION.csv`.
6. Verify source → target behavior (live crawl; expect 35× `301 → 200`).
7. Switch DNS / Cloudflare routing `alumcasting.com` → GitHub Pages.
8. Live crawl + validate canonical / robots / sitemap / JSON-LD.
9. Monitor 404 / redirect chains; resolve the 4 HUMAN_REVIEW via business decision; confirm 2
   NO-EQUIVALENT return 404.

---

## 10. Final Safety Statement (this phase)

No WordPress, Cloudflare, DNS, GitHub Pages, Actions, or Hugo source change was made. No redirect
was created, modified, or removed. The only filesystem changes are the three uncommitted report
files. **Commit, push, and deploy are NOT authorized for this phase.**

**NEXT ACTION = STOP.** Await explicit authorization before any import, cutover, or commit.
