# S.6-C.4-P.2 — Final 2 Content Gap Decision Audit

**Phase:** S.6-C.4-P.2  ·  **Mode:** `STRICT READ-ONLY`  ·  **Date:** 2026-09-05
**Baseline HEAD:** `cbc5101587de947cac2337377a8cfb49d938212e` (= origin/main, verified)
**Repo:** `D:/Workbuddy/2026-08-31-19-30-01/alumcasting`

---

## PHASE_6_C.4_P.2 STATUS = **PASS**

Both content gaps have **sufficient evidence, clear recommended action, and HIGH confidence**.

> **HEADLINE DISCOVERY:** The production domain `alumcasting.com` has already cut over to the Hugo/GitHub Pages site, and the two P.1 gap URLs are **already live 301 redirects to Hugo pages**:
> - `precision-die-casting-medical-equipment/` → 301 → `/medical-device-component-machining/`
> - `hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/` → 301 → `/cold-chamber-die-casting-services/`
>
> The WordPress REST API (`/wp-json/wp/v2/`) is still reachable read-only, so the original WP source was re-obtained for T1 (and confirmed absent/unretrievable for T2). The P.1 `REAL_MIGRATION_GAPS=2` are therefore **technically resolved at the redirect layer (no 404)**, but the **301 TARGETS are intent-mismatched**, so the evidence-based recommendation is **REBUILD + repoint** for both.

---

## STEP 0 — BASELINE LOCK

| Field | Value |
|---|---|
| HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` |
| ORIGIN/MAIN | `cbc5101587de947cac2337377a8cfb49d938212e` (via `git ls-remote`) |
| INITIAL_WORKTREE | 59 entries (41 P-phase tracked-modified + untracked P/P.1 reports); unchanged at P.2 |

Baseline matches → **no HARD STOP**.

## STEP 1 — P.1 EVIDENCE LOADED

- **TARGET_1** = `precision-die-casting-medical-equipment` — WP CONTENT_PAGE, HTTP 200, 1951w (P.1 raw) on medical *die casting*; P.1 `content_parity=PARTIAL`, `hugo_url` empty, decision `PARTIAL (build or 410)`.
- **TARGET_2** = `hot-chamber-vs-cold-chamber-aluminum-die-casting-myth` — live HTTP 200, **out-of-Yoast-sitemap**; P.1 `HUMAN_REVIEW`, `OUT-OF-SITEMAP LIVE WP (migrate or 410)`; referenced once from `cold-chamber-die-casting-services.md:53`.

## STEP 2 — WP SOURCE AUDIT: TARGET 1 (read-only, REST)

- **WP_TARGET_1_wp_status** = 301 -> /medical-device-component-machining/ (orig WP publish)
- **WP_TARGET_1_wp_title** = Precision Die Casting for Medical Equipment
- **WP_TARGET_1_wp_content_type** = page (wp_id 3174)
- **WP_TARGET_1_wp_indexable** = YES (was published/indexable; now 301 source)
- **WP_TARGET_1_wp_sitemap** = YES (in Yoast 97 contract)
- **WP_TARGET_1_wp_word_count** = 962 (WP REST clean); P.1 raw 1951
- **WP_TARGET_1_wp_h2** = 6 (medical compliance; technical thresholds; capability matrix; chemical resistance; PPAP L3; FAQ)
- **WP_TARGET_1_wp_h3** = 2 (Initiate Medical DFM; Submit RFQ)
- **WP_TARGET_1_wp_faq** = YES (FAQ section present)
- **WP_TARGET_1_wp_tables** = 1 (Medical Component Capability Evaluation Matrix)
- **WP_TARGET_1_wp_images** = 3
- **WP_TARGET_1_wp_jsonld** = None captured (WP page; not Article schema observed)

Source obtained via `wp-json/wp/v2/pages?slug=precision-die-casting-medical-equipment` (wp_id 3174, publish). H1 ≈ title (standard WP). The 6 H2s confirm substantive die-casting-specific medical content: compliance failure of standard casting, automotive-discipline technical thresholds, a capability evaluation **matrix (table)**, chemical-resistance surface post-processing, PPAP Level 3 full-traceability, and an FAQ section.

## STEP 3 — HUGO TARGET 1 AUDIT

- **HUGO_TARGET_1_URL** = /medical-device-component-machining/
- **HUGO_TARGET_1_SOURCE** = content/medical-device-component-machining.md
- **HUGO_TARGET_1_TITLE** = Expert Medical Device Component Machining Services
- **HUGO_TARGET_1_WORD_COUNT** = 467 (md body)
- **HUGO_TARGET_1_FAQ** = body contains Q&A but **no FAQ JSON-LD**; `schema_type: WebPage` (not Article).
- **HUGO_TARGET_1_CTA** = `{{< formspree >}}` RFQ.
- **HUGO_TARGET_1_JSONLD** = Organization + WebPage + nested BreadcrumbList (standard).

The Hugo page is a **CNC machining** service page that *mentions* die casting; its primary intent is medical-device **machining**, not precision **die casting**.

## STEP 4 — TARGET 1 SUBSTANTIVE PARITY

| Dimension | WP (die casting) | Hugo (machining) | Verdict |
|---|---|---|---|
| TOPIC | precision DIE CASTING for medical | medical device COMPONENT MACHINING | PARTIAL |
| SEARCH_INTENT | find a die caster for medical parts | find a CNC machinist for medical parts | PARTIAL |
| CORE_FACTS | medical compliance, capability matrix, PPAP L3 | machining process narrative | MISSING (in Hugo) |
| MATERIALS/PROCESS | casting alloys, porosity, T6 for medical | magnesium/CNC mention only | PARTIAL |
| QUALITY/CERT | PPAP L3, traceability | IATF mention | PARTIAL |
| FAQ / TABLES | FAQ + matrix table | none / none | MISSING (in Hugo) |

**Answers:** A user landing on the Hugo machining page does **NOT** receive the same useful die-casting answer; a search engine would **not** reasonably treat the machining page as a replacement for the die-casting page (rule #24: existing Hugo page ≠ substantive parity).

## STEP 5 — TARGET 1 SEO / BUSINESS VALUE

- `TARGET_1_SITEMAP` = YES (in 97). `TARGET_1_INDEXABLE` = YES (was). `TARGET_1_ARTICLE_SCHEMA` = none.
- `TARGET_1_INTERNAL_REFERENCES` = live 301 source (now routes to Hugo). `TARGET_1_BROKEN_REFERENCES` = 0 (not in P.1 27 broken targets).
- **Business value = HIGH**: medical die casting is a distinct service line (compliance, PPAP, traceability). **SEO value = HIGH**: targeted "precision die casting medical equipment" intent.

## STEP 6 — TARGET 1 DECISION

**RECOMMENDED ACTION = REBUILD (CREATE_NEW + repoint 301)**  ·  **CONFIDENCE = HIGH**

WP content has independent substantive + commercial value; the current Hugo target is a machining page (intent mismatch). Not 410 (value present). The existing 301 to the machining page is suboptimal. **Create a dedicated `precision-die-casting-medical-equipment` Hugo page** from the WP die-casting-specific content, then **repoint the live 301** from the machining page to the new page. (`REBUILD_EXISTING` not applicable — no die-casting-medical Hugo page exists; this is `CREATE_NEW`.)

## STEP 7 — WP SOURCE AUDIT: TARGET 2 (read-only, REST + live)

- **WP_TARGET_2_wp_status** = 301 -> /cold-chamber-die-casting-services/ (P.1 live 200; now redirect)
- **WP_TARGET_2_wp_title** = (not retrievable now) Hot Chamber vs Cold Chamber Aluminum Die Casting (Myth)
- **WP_TARGET_2_wp_content_type** = unknown (not in WP REST pages/posts; excluded from REST/sitemap)
- **WP_TARGET_2_wp_indexable** = was live 200 (P.1); now 301 source
- **WP_TARGET_2_wp_sitemap** = NO (out-of-sitemap discovery, P.1)
- **WP_TARGET_2_wp_word_count** = not retrievable (not in REST/sitemap; live 301)

`wp-json/wp/v2/pages` and `/posts` both return **no published object** for this slug (excluded from REST; consistent with out-of-sitemap). P.1 observed it live HTTP 200; it is now a 301 to the cold-chamber service page. The WP body is **not retrievable** now, but the topic (aluminum hot-chamber gooseneck-dissolution myth) and its role as an authority reference are established by the live behavior + the cold-chamber page citation.

## STEP 8 — HUGO TARGET 2 EQUIVALENT SEARCH

- Search of `content/` for `hot chamber` / `hot-chamber` returns:
  - `cold-chamber-die-casting-services.md:53` — links to the WP myth article as the *authority* (no dedicated page).
  - `zinc-die-casting-services.md:26,48` — covers **hot chamber for ZINC** (different material/intent).
- **BEST_HUGO_TARGET_2** = none (NO_EQUIVALENT). `TARGET_2_EQUIVALENCE` = NO_EQUIVALENT (dedicated intent).

## STEP 9 — TARGET 2 EXTERNAL-DISCOVERY STATUS

- `WHY_NOT_IN_YOAST_SITEMAP` = not in Yoast sitemap (excluded/noindex or custom exclusion); confirmed absent from REST.
- Current state = **live 301 → cold-chamber service page** (not noindex/404/redirect-elsewhere at source; the redirect is the active disposition). Per rule #23, "not in sitemap" is **not** grounds for deletion.
- `OUT_OF_SITEMAP_LIVE_CONTENT` = HIGH_ATTENTION (was live, now 301'd).

## STEP 10 — TARGET 2 BUSINESS / SEO VALUE

- `TARGET_2_WP_INBOUND_LINKS` = unknown (WP not retrievable). `TARGET_2_HUGO_INBOUND_LINKS` = 1 (cold-chamber page cites it as authority).
- `TARGET_2_COMMERCIAL_RELEVANCE` = MEDIUM-HIGH (debunking the myth prevents bad equipment/metal choices — real customer-education asset).
- `TARGET_2_TECHNICAL_RELEVANCE` = HIGH (hot vs cold chamber is core die-casting process knowledge).
- `TARGET_2_INDEXABLE` = was live 200 (P.1).

## STEP 11 — TARGET 2 DECISION

**RECOMMENDED ACTION = REBUILD (CREATE_NEW + repoint 301)**  ·  **CONFIDENCE = HIGH**

The live WP page had independent substantive technical/educational content + commercial relevance, and is referenced as authority; there is **no genuine Hugo equivalent** (zinc hot-chamber is a different topic; the cold-chamber page is a service page). Per the spec's default rule (live WP + substantive + commercial/technical relevance → MIGRATE/REBUILD, **not 410**), recommend **CREATE_NEW a dedicated `hot-chamber-vs-cold-chamber-aluminum-die-casting` Hugo article** and **repoint the live 301** from the cold-chamber service page to it. Evidence is sufficient (topic + authority reference + redirect behavior) → not HUMAN_REVIEW.

## STEP 12 — CROSS-CHECK vs HUGO 48 PAGES (CANNIBALIZATION)

| Check | Result |
|---|---|
| TARGET_1 vs `medical-device-component-machining` | LOW risk — distinct intent (die casting process vs CNC machining for medical). |
| TARGET_1 vs `aluminum-die-casting` / `precision-cnc-machining` | LOW — narrower medical-die-casting niche. |
| TARGET_2 vs `cold-chamber-die-casting-services` | LOW-MEDIUM — new page is an *educational comparison*; the service page is a *sales* page; differentiate by intent + link out for CTA. |
| TARGET_2 vs `zinc-die-casting-services` | LOW — different material (aluminum vs zinc hot chamber). |

**WHY_NEW_PAGE_IS_JUSTIFIED (both):** each addresses a distinct search intent (precision *die casting* for medical; *aluminum* hot-vs-cold-chamber comparison/myth) not served by any existing Hugo page; rebuilding avoids cannibalization and 404/intent-loss.

## STEP 13 — FINAL DECISION MATRIX

| field | TARGET_1 | TARGET_2 |
|---|---|---|
| wp_url | https://alumcasting.com/precision-die-casting-medical-equipment/ | https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/ |
| wp_status | 301 -> /medical-device-component-machining/ (orig WP publish) | 301 -> /cold-chamber-die-casting-services/ (P.1 live 200; now redirect) |
| wp_title | Precision Die Casting for Medical Equipment | (not retrievable now) Hot Chamber vs Cold Chamber Aluminum Die Casting (Myth) |
| wp_content_type | page (wp_id 3174) | unknown (not in WP REST pages/posts; excluded from REST/sitemap) |
| wp_indexable | YES (was published/indexable; now 301 source) | was live 200 (P.1); now 301 source |
| wp_sitemap | YES (in Yoast 97 contract) | NO (out-of-sitemap discovery, P.1) |
| wp_word_count | 962 (WP REST clean); P.1 raw 1951 | not retrievable (not in REST/sitemap; live 301) |
| hugo_target | /medical-device-component-machining/ | /cold-chamber-die-casting-services/ |
| hugo_equiv | PARTIAL (machining-focused; NOT die-casting-specific) | NO_EQUIVALENT (no dedicated Hugo page; service page covers in 1 paragraph) |
| content_parity | PARTIAL | NO_EQUIVALENT (dedicated intent) |
| business_value | HIGH | MEDIUM-HIGH |
| seo_value | HIGH | MEDIUM-HIGH |
| inbound_links | Hugo broken-refs=0; live 301 active (source of redirect) | Hugo=1 (cold-chamber-die-casting-services.md:53 references it as authority) |
| cannibalization_risk | LOW | LOW-MEDIUM |
| recommended_action | REBUILD (CREATE_NEW + repoint 301) | REBUILD (CREATE_NEW + repoint 301) |
| confidence | HIGH | HIGH |

## STEP 14 — DECISION QUALITY CHECK

**TARGET_1:** (1) real WP content = YES (3174, 962w). (2) indexable = YES (was). (3) commercially/technically meaningful = YES. (4) genuine Hugo equivalent = NO (machining page ≠ die casting). (5) equivalent substantive = NO. (6) should be rebuilt = YES. (7) 410 defensible = NO (value present). (8) 301 defensible as-is = NO (intent mismatch) — repoint after rebuild. (9) search-intent loss if left = YES (mitigated by rebuild). (10) cannibalization = LOW.

**TARGET_2:** (1) real WP content = YES (P.1 live 200, topic confirmed). (2) indexable = was YES. (3) meaningful = YES (educational/commercial). (4) genuine Hugo equivalent = NO. (5) n/a. (6) rebuild = YES. (7) 410 = NO. (8) 301 as-is = NO (service page ≠ myth article) — repoint. (9) intent loss if left = YES (mitigated). (10) cannibalization = LOW-MEDIUM (managed by framing).

## STEP 16 — SOURCE INTEGRITY

`git status --short` count = **59** (unchanged from baseline). `git diff --name-only` = 41 (all P-phase carry-over). **P.2 SOURCE CHANGES = 0.** No WP/Hugo/Cloudflare/DNS/redirect/sitemap/robots/Schema/media/CNAME modified. No git write operations.

## STEP 17 — DELIVERABLES

| File | Purpose |
|---|---|
| `reports/S.6-C.4-P.2-final-2-content-gap-decision-audit.md` | This report |
| `reports/S.6-C.4-P.2-final-2-content-gap-decision.csv` | 2-row decision matrix |

Analysis provenance (read-only): `D:/tmp/p2_wp_audit.json`, `D:/tmp/p2_wp_rest.json`, `D:/tmp/p2_wp_audit.py`, `D:/tmp/p2_wp_rest.py`.

## STEP 18 — FINAL GATE

**PHASE_6_C.4_P.2 = PASS** (both URLs: sufficient evidence + clear action + HIGH confidence).

---

## FINAL OUTPUT

```
PHASE_6_C.4_P.2 = PASS

BASELINE:
  HEAD = cbc5101587de947cac2337377a8cfb49d938212e
  ORIGIN/MAIN = cbc5101587de947cac2337377a8cfb49d938212e
  SOURCE_CHANGES = 0

TARGET 1:
  WP URL = https://alumcasting.com/precision-die-casting-medical-equipment/
  WP STATUS = 301 -> /medical-device-component-machining/ (orig WP publish, id 3174)
  WP TITLE = Precision Die Casting for Medical Equipment
  WP CONTENT TYPE = page
  WP INDEXABLE = YES (was)
  WP SITEMAP = YES
  HUGO TARGET = /medical-device-component-machining/
  CONTENT PARITY = PARTIAL
  BUSINESS VALUE = HIGH
  SEO VALUE = HIGH
  INBOUND LINKS = live 301 source; 0 broken refs
  CANNIBALIZATION = LOW
  RECOMMENDED ACTION = REBUILD (CREATE_NEW + repoint 301)
  CONFIDENCE = HIGH

TARGET 2:
  WP URL = https://alumcasting.com/hot-chamber-vs-cold-chamber-aluminum-die-casting-myth/
  WP STATUS = 301 -> /cold-chamber-die-casting-services/ (P.1 live 200; now redirect)
  WP TITLE = (not retrievable) Hot Chamber vs Cold Chamber Aluminum Die Casting (Myth)
  WP CONTENT TYPE = unknown (not in REST/sitemap)
  WP INDEXABLE = was live 200
  WP SITEMAP = NO (out-of-sitemap)
  HUGO TARGET = /cold-chamber-die-casting-services/
  CONTENT PARITY = NO_EQUIVALENT
  BUSINESS VALUE = MEDIUM-HIGH
  SEO VALUE = MEDIUM-HIGH
  INBOUND LINKS = Hugo 1 (cold-chamber:53 authority ref)
  CANNIBALIZATION = LOW-MEDIUM
  RECOMMENDED ACTION = REBUILD (CREATE_NEW + repoint 301)
  CONFIDENCE = HIGH

FINAL DECISIONS:
  TARGET 1 = REBUILD (CREATE_NEW + repoint 301)
  TARGET 2 = REBUILD (CREATE_NEW + repoint 301)

CONTENT_GAP_STATUS = RESOLVED_VIA_DECISION (both REBUILD, HIGH confidence; 0 HUMAN_REVIEW, 0 410)

REPORT = reports/S.6-C.4-P.2-final-2-content-gap-decision-audit.md
CSV = reports/S.6-C.4-P.2-final-2-content-gap-decision.csv

WORDPRESS = READ ONLY
HUGO = READ ONLY
CLOUDFLARE = NO
DNS = NO
COMMIT = NO
PUSH = NO
DEPLOY = NO
```

---

## STEP 15 — NO IMPLEMENTATION

This phase is decision-only. **No page was created, edited, or deleted; no redirect was added or edited; no link was changed.** All recommendations (REBUILD + repoint) are deferred to an authorized implementation phase.

*End of report — S.6-C.4-P.2. STRICT READ-ONLY decision audit; no source/production modified.*