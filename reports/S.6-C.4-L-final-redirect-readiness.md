# S.6-C.4-L — Final Redirect Implementation Readiness Audit

**Mode:** STRICT READ-ONLY / NO MUTATION / NO PRODUCTION CHANGE
**Status:** READ-ONLY AUDIT + PREPARATION PACKAGE ONLY — NO REDIRECTS WRITTEN, NO WORDPRESS/CLAUDRFARE/DNS/PAGES/ACTIONS CHANGES, NO COMMIT, NO PUSH, NO DEPLOY

---

## 1. Baseline & Authoritative Source of Truth

| Item | Value |
|---|---|
| Repo HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` (verified `git rev-parse HEAD`) |
| Branch | `main` |
| Baseline source of truth | Frozen **S.6-C.4-K** result (4-URL decision freeze) |
| Supporting reports | `S.6-C.4-K-final-human-review-freeze.md`, `S.6-C.4-J-human-review-resolution.md`, `S.6-C.4-I-redirect-decision-audit.md`, `MIGRATION-PRE-CUTOVER-AUDIT.md` |
| Other evidence | `audit_tmp/wp_crawl.jsonl` (97-URL live WP crawl), built Hugo `public/` output, live GitHub Pages (`diecasting.github.io/alumcasting`) |

**Frozen final matrix (S.6-C.4-K, NOT reinterpreted):**

```
301_SEMANTIC_REMAP = 35
410_OR_404         = 2
SYSTEM_URL_NO_REDIRECT = 8
HUMAN_REVIEW       = 4
TOTAL = 49
```

The 4 HUMAN_REVIEW URLs were **not** reopened or reinterpreted (per phase rules).

---

## 2. TASK 1 — Reconstructed Final 49-URL Matrix

Reconciliation (counts unchanged from K):

| Source | SEMANTIC_REMAP | NO_EQUIVALENT | SYSTEM | HUMAN_REVIEW |
|---|---:|---:|---:|---:|
| S.6-C.4-I (non-HR) | 29 | 2 | 8 | 0 |
| S.6-C.4-J resolved (of 10 HR) | +6 | 0 | 0 | 4 |
| S.6-C.4-K freeze | 0 | 0 | 0 | 4 |
| **FINAL** | **35** | **2** | **8** | **4** |

Total = 35 + 2 + 8 + 4 = **49** — matches the unresolved inventory exactly. No arithmetic forced.

The 35 sources, their targets, confidence, and decision reason are listed in full in
`MIGRATION-REDIRECT-IMPLEMENTATION.md` §1 and in `MIGRATION-REDIRECT-IMPLEMENTATION.csv` (35 rows).
The 2 / 8 / 4 are enumerated in that same package (§2, §3, §4). **No mapping was invented; no frozen
decision was silently changed.**

---

## 3. TASK 2 — Target Validation (all 35)

Method: built Hugo `public/<slug>/index.html` parsed + live GitHub Pages GET (no WAF on `github.io`).

| Check | Result | Flag raised |
|---|---|---|
| Target built in `public/` | **35 / 35** present | — |
| Target live HTTP = 200 | **35 / 35** | — |
| Target H1 = 1 | **35 / 35** | — |
| Target robots indexable (no `noindex`) | **35 / 35** | — |
| Target does not itself redirect (no meta-refresh / self-301) | **35 / 35** leaf pages | — |
| Target canonical self-referential (no wrong-page canonical) | **35 / 35** | — |
| Target is a staging URL | **No** — targets are production-relative `/slug/` paths | — |
| Target is old WordPress URL | **No** | — |
| Target missing from Hugo inventory | **0** | `TARGET_MISSING = 0` |
| Target is 404 | **0** | `TARGET_404 = 0` |
| Target is noindex | **0** | `TARGET_NOINDEX = 0` |
| Target has bad/incorrect canonical | **0** | `TARGET_BAD_CANONICAL = 0` |
| Target staging host in current canonical attribute | All 35 carry `diecasting.github.io/alumcasting/...` host | **Pre-cutover P0** (see §11) — cutover prerequisite, not a per-target defect |

**Conclusion:** `Target validation = PASS`. The only canonical nuance is the site-wide staging-host
leak (already a P0 in the pre-cutover audit), resolved by cutover step 1 (`baseURL` →
`https://alumcasting.com/`). No `TARGET_*` failure flags were raised against any of the 35.

---

## 4. TASK 3 — Redirect Chain Safety

Per-source current WP behavior (from `wp_crawl.jsonl`, 97-URL crawl; all 35 sources are live WP content):

| Metric | Value |
|---|---|
| 35 sources currently `status=200` | **35 / 35** |
| 35 sources currently `redirect=False` (not already redirecting) | **35 / 35** |
| 35 sources already part of an existing redirect chain | **0** |
| Resulting proposed hop | `301 → 200` (1 hop, **no loop**) |
| Targets that themselves redirect | **0** (all leaf pages) |

**Special items (read-only, prior-phase evidence):**
- **3169** `/semi-solid-die-casting-manufacturers/` → 301 → `/semi-solid-die-casting-heat-treatable-aluminum/`
  (OBSERVED_REDIRECT; `CONFIGURATION_CONFIRMED = NO`).
- **SSM generic** `/semi-solid-die-casting/` → 301 → `/semi-solid-die-casting-heat-treatable-aluminum/`
  (OBSERVED_REDIRECT).
- Proposed rule **#27** `/t6-heat-treatment-semi-solid-die-casting-aluminum/` → same target.

These are **three distinct sources converging on one leaf target** — benign consolidation, not a
conflict or loop. `EXISTING_REDIRECT = YES` for the 3169/SSM sources, but those sources are **not**
among our 35, so there is no rule-on-rule conflict and no wrong-target. No `301→301` chain, no loop.

**Conclusion:** `Redirect chain safety = PASS`.

---

## 5. TASK 4 — WordPress Redirect Conflict Check

- `REDIRECTION_PLUGIN_VISIBILITY = INCONCLUSIVE`. Live probe of `/wp-json/redirection/v1/redirect`
  returned **401/403** (auth + SiteGround WAF block from sandbox) this phase; the plugin's full
  rule set cannot be read. **No POST/PUT/PATCH/DELETE was attempted.**
- The 35 proposed sources are currently served at **200 (not redirecting)** on WP → **no existing WP
  rule matches them** → **no conflict**.
- Prior-phase **observed** 3169/SSM redirects are recorded as `OBSERVED_REDIRECT`, explicitly
  distinguished from `CONFIGURATION_CONFIRMED` (which remains NO).
- **Implementation caveat (prerequisite, not a blocker):** rules imported *only* into the WordPress
  Redirection plugin will stop firing once DNS/Cloudflare routes `alumcasting.com` to GitHub Pages.
  Recommend implementing at the **durable layer (Cloudflare Single/Bulk Redirects)** so they survive
  the cutover. See package §7.

---

## 6. TASK 5 — System URL Safety (8)

All 8 are taxonomy/archive URLs; none receives a 301, forced target, or invented replacement.

| Classification | Count | Entries |
|---|---:|---|
| category | 4 | `/category/casting-industry-applications/`, `/category/manufacturing-capabilities/`, `/category/quality-control-testing/`, `/category/semi-solid-die-casting/` |
| tag | 4 | `/tag/iatf-16949-certified-die-casting/`, `/tag/magnesium-die-casting-services/`, `/tag/rheocasting-process/`, `/tag/vacuum-assisted-die-casting/` |

`8 SYSTEM URLs = PASS` (untouched, correct exclusion per RULE 4/6).

---

## 7. TASK 6 — 4 HUMAN_REVIEW Safety (FROZEN)

| # | URL | Status |
|---|---|---|
| #2 | `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | `HUMAN_REVIEW` |
| #3 | `/forging-casting-vs-cnc-manufacturing-guide/` | `HUMAN_REVIEW` |
| #4 | `/guide-to-types-of-metal-casting-processes/` | `HUMAN_REVIEW` |
| #7 | `/magnesium-vs-aluminum-die-casting/` | `HUMAN_REVIEW` |

No targets assigned. Not converted to 301. Not auto-converted to 410. Status remains `HUMAN_REVIEW`.

---

## 8. TASK 7 — 2 NO-EQUIVALENT Safety

| URL | Decision | Notes |
|---|---|---|
| `/case-studies/` | `410_OR_404` | No Hugo equivalent; recommend ordinary 404. No replacement page invented; not redirected to a commercial page. |
| `/prevent-blistering-aluminum-t6-heat-treatment/` | `410_OR_404` | Live WP 200 but no Hugo equivalent; OUT-OF-SCOPE legacy dead link; recommend 404. Not mapped to SSM owner. |

No content invented; no rewrite. Implementation (404) must NOT occur in this phase.

---

## 9. TASK 8 — Cutover Import Package (prepared, NOT imported)

- `reports/MIGRATION-REDIRECT-IMPLEMENTATION.csv` — **exactly 35 rows**, columns
  `source,target,status,regex`; every row `status=301`, `regex=0`.
- `reports/MIGRATION-REDIRECT-IMPLEMENTATION.md` — full package: 35 approved redirects,
  2 NO-EQUIVALENT, 8 SYSTEM, 4 HUMAN_REVIEW, conflict warnings, chain warnings, target validation,
  implementation prerequisites, exact recommended execution order.

These are preparation artifacts only. **WordPress was NOT written.**

---

## 10. TASK 9 — Cutover Order (dependency documentation, NOT executed)

Recommended dependency order (full detail in package §9):

1. Hugo production `baseURL = https://alumcasting.com/` (P0)
2. Production CNAME / custom domain ready
3. All required Hugo pages/assets ready (✅ 35 targets verified)
4. Media migration (`wp-content` → `static/`/CDN; P1)
5. Prepare/import 35 rules at durable layer (Cloudflare; optional WP for pre-cutover verify)
6. Verify source → target (expect 35× `301 → 200`)
7. Switch DNS / Cloudflare routing → GitHub Pages
8. Live crawl; validate canonical/robots/sitemap/JSON-LD
9. Monitor 404 / chains; resolve 4 HUMAN_REVIEW via business decision

`Cutover dependency order = READY` (documented). No step was executed.

---

## 11. TASK 10 — Asset Warning

Cross-checked against `MIGRATION-PRE-CUTOVER-AUDIT.md` §Asset:

- **163** WordPress media dependencies: **158** `<img>` references + **5** Article JSON-LD `image`
  fields, all pointing to `https://alumcasting.com/wp-content/uploads/...`.
- These remain WP-host-dependent. If WordPress is retired, the images break unless re-hosted.
- `ASSET_MIGRATION_REQUIRED = YES`. Image URLs were **not** altered this phase.

---

## 12. TASK 11 — Final Safety Check

| System | Change this phase? |
|---|---|
| Repository source (tracked) | **NONE** |
| WordPress | **NONE** |
| Redirect rules | **NONE** (prepared only) |
| Cloudflare | **NONE** |
| DNS | **NONE** |
| GitHub Pages settings | **NONE** |
| Actions / workflows | **NONE** |

`git status` (post-phase) shows only untracked report files:
`MIGRATION-PRE-CUTOVER-AUDIT.md`, `S.6-C.4-I-...`, `S.6-C.4-J-...`, `S.6-C.4-K-...`,
`S.6-C.4-L-final-redirect-readiness.md` (this), `MIGRATION-REDIRECT-IMPLEMENTATION.csv`,
`MIGRATION-REDIRECT-IMPLEMENTATION.md`. **No tracked file modified. No commit, no push.**

---

## 13. FINAL STATUS

```
S.6-C.4-L = READY_FOR_IMPLEMENTATION

35 approved 301 targets       = PASS   (35/35 built + live 200, H1=1, indexable, self-canonical, no self-redirect)
2 NO-EQUIVALENT               = PASS   (frozen; recommend 404; no rewrite)
8 SYSTEM URLs                 = PASS   (frozen; no 301)
4 HUMAN_REVIEW                = FROZEN (untouched)
Redirect chain safety         = PASS   (35× 301→200, 0 loops; 3169/SSM convergence benign)
Target validation             = PASS   (0 missing / 0 404 / 0 noindex / 0 bad-canonical)
WordPress redirect visibility = INCONCLUSIVE (plugin 401/403; OBSERVED_REDIRECT distinguished)
Asset dependency              = YES   (163 wp-content refs; ASSET_MIGRATION_REQUIRED)
Cutover dependency order      = READY (documented)
Repository changes            = NONE  (only 3 new untracked reports)
Production changes            = NONE

Reports:
  reports/S.6-C.4-L-final-redirect-readiness.md
  reports/MIGRATION-REDIRECT-IMPLEMENTATION.csv
  reports/MIGRATION-REDIRECT-IMPLEMENTATION.md

Commit      = NOT AUTHORIZED
Push        = NOT AUTHORIZED
Deploy      = NOT AUTHORIZED
```

### Readiness rationale
All criteria for `READY_FOR_IMPLEMENTATION` are met: every one of the 35 approved redirects has a
validated (built + live-200, single-H1, indexable, self-canonical, non-redirecting) target; no
unsafe chains remain; no target is 404 / noindex / bad-canonical; the 4 HUMAN_REVIEW, 8 SYSTEM, and
2 NO-EQUIVALENT buckets are untouched and intact; the 49-URL reconciliation is exactly preserved;
and no unauthorized change occurred. The only open items are pre-cutover *prerequisites*
(site-wide `baseURL`/canonical flip = P0; `wp-content` media migration = P1) and the 4 pending
business decisions for HUMAN_REVIEW — none of which block the readiness of the 35-URL package
itself.

---

## 14. HARD STOP

This phase produced a read-only readiness audit and a preparation package only. **STOP.**

Do NOT import redirects. Do NOT proceed to baseURL cutover. Do NOT proceed to Cloudflare/DNS changes.
Do NOT modify WordPress production. Do NOT commit or push.

Await explicit authorization for the next phase (redirect import / cutover / commit).
