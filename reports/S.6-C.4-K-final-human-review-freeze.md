# S.6-C.4-K — Final Human Review / 4-URL Decision Freeze

**Mode:** STRICT READ-ONLY / NO MUTATION / NO PRODUCTION CHANGE
**Status:** READ-ONLY DECISION FREEZE — NO REDIRECTS WRITTEN, NO WORDPRESS/CLAUDRFARE/DNS/PAGES/ACTIONS CHANGES, NO COMMIT, NO PUSH

---

## 1. Baseline Git State

| Item | Value |
|---|---|
| Expected baseline HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` |
| Current HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` (verified via `git rev-parse HEAD`) |
| Current branch | `main` |
| origin/main | `cbc5101587de947cac2337377a8cfb49d938212e` (verified via `git ls-remote origin refs/heads/main`) |
| Working tree | clean except **3 prior-phase untracked reports** (`MIGRATION-PRE-CUTOVER-AUDIT.md`, `S.6-C.4-I-redirect-decision-audit.md`, `S.6-C.4-J-human-review-resolution.md`); **no tracked source/config changes** |
| Tracked changes from this phase | **NONE** |

Baseline matches the expected authoritative commit. No HARD STOP triggered.

---

## 2. Four Source URLs (from S.6-C.4-J)

| # | Old WP URL | WP Title (evidence) | WP Status | Canonical |
|---|---|---|---|---|
| #2 | `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | *Chinese Aluminum Casting Grade Equivalents: A380 vs ADC12* | 200 | self (`alumcasting.com/...`) |
| #3 | `/forging-casting-vs-cnc-manufacturing-guide/` | *Forging vs Casting vs CNC: Master the Manufacturing Choice* | 200 | self |
| #4 | `/guide-to-types-of-metal-casting-processes/` | *Types of Metal Casting Processes* | 200 | self |
| #7 | `/magnesium-vs-aluminum-die-casting/` | *Magnesium vs. Aluminum Die Casting* | 200 | self |

> **Slug note (K spec §"FOUR REMAINING"):** The K spec abbreviates #2 as `/chinese-grade-equivalents/` and #3 as `/forging-casting-vs-cnc/`; these are the same WP posts as J #2 (`/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/`) and J #3 (`/forging-casting-vs-cnc-manufacturing-guide/`). No additional URLs were invented.

---

## 3. Source-Content Evidence

Captured from (a) live read-only HTTP this phase and (b) the prior documented crawl `wp_crawl.jsonl` (97 URLs, all 200/self-canonical):

- **#2** — Confirmed **200 / live this phase** (`title: Chinese Aluminum Casting Grade Equivalents: A380 vs ADC12`). 3-way aluminum-alloy **grade-equivalence mapping** (A380 / A356 / ADC12 + Chinese YL/ZL standards). Informational/commercial-mixed intent, dominant material = aluminum, dominant *format* = comparison table.
- **#3** — Confirmed **200 / live this phase** (`title: Forging vs Casting vs CNC: Master the Manufacturing Choice`). 3-process **selection guide**. Site offers casting + CNC but **NOT forging**; intent = "which process do I choose".
- **#4** — Confirmed **200** from `wp_crawl.jsonl` (self-canonical). Broad **multi-process survey** (sand, gravity, HPDC, investment, etc.). Informational/taxonomy intent.
- **#7** — Confirmed **200** from `wp_crawl.jsonl` (self-canonical). Pure **2-material comparison** (magnesium vs aluminum). No dominant-material signal in title/H1.

All four are **genuine comparison / overview / pillar** articles, not single-topic pages.

---

## 4. Candidate Owner Matrix

Candidate Hugo pages were confirmed present in `content/` (titles read directly from front matter):

| Source | Candidate A | Candidate B | Candidate C | Exists? |
|---|---|---|---|---|
| #2 | `/380-aluminum-die-casting-service/` ("A380 Aluminum Die Casting Service") | `/a356-aluminum-die-casting-porosity-control/` ("…A356…Porosity Control") | `/adc12-die-casting-cnc-machining/` ("ADC12 Die Casting & CNC") | all YES |
| #3 | `/aluminum-die-casting/` ("Aluminum Die Casting Manufacturer") | `/precision-cnc-machining/` ("High Tolerance Precision CNC Machining") | — | both YES |
| #4 | `/aluminum-die-casting/` | `/sand-casting-services/` | `/high-pressure-die-casting-process-quality/` | all YES |
| #7 | `/magnesium-die-casting-services/` ("Magnesium Die Casting Supplier") | `/aluminum-die-casting/` | — | both YES |

No comparison hub / overview hub / knowledge-architecture page exists in the Hugo repository for any of these four intents.

---

## 5. Semantic Comparison (OWNER TEST)

A candidate becomes the 301 owner **only if** it passes all 10 OWNER-TEST conditions — including: *directly satisfies the dominant intent*, *does not materially misrepresent the original content*, *is not merely adjacent by keyword*, *does not create topic cannibalization*.

| # | Best candidate | Passes OWNER TEST? | Why it fails |
|---|---|---|---|
| #2 | `/380-aluminum-die-casting-service/` (A380, title-leading) | **NO** | Covers 1/3 of the post (A380 only); the "grade-equivalence mapping" intent is absent. Forcing misrepresents 2/3 of the content. `aluminum-die-casting` is a **commercial** page → violates **BUSINESS-INTENT SAFETY** (no redirect of informational article to commercial page by keyword). |
| #3 | `/aluminum-die-casting/` | **NO** | Post is a 3-process selection guide; landing on casting drops forging + CNC halves and changes intent. Site does **not** forge, so no full-satisfaction target exists. |
| #4 | `/aluminum-die-casting/` | **NO** | Post surveys *all* casting types; die casting is one of many. Landing on die casting misrepresents the broad survey intent (fails "satisfies majority of source intent"). |
| #7 | `/magnesium-die-casting-services/` OR `/aluminum-die-casting/` | **NO** | Pure 2-material comparison; neither single-material page is "the comparison". Spec **§6A** explicitly warns: do NOT auto-select either material absent a clear dominant intent. Forcing to either drops half the content. |

**Conclusion:** No candidate passes the OWNER TEST for any of the four URLs. Per spec **§D**, when no suitable owner exists, `410_OR_404` is allowed *only if source evidence supports intentional retirement*; absent that signal, the correct disposition is **HUMAN_REVIEW**.

---

## 6. Decision Rationale (per URL)

### #2 `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` → **HUMAN_REVIEW**
Genuine 3-way aluminum-alloy grade-equivalence comparison with no surviving comparison hub in Hugo. The three candidate pages each own 1/3 of the post. The umbrella `/aluminum-die-casting/` is a commercial service page — redirecting an informational grade-mapping article to it would violate the BUSINESS-INTENT SAFETY rule. **Business decision required:** either (a) 301 to `/380-aluminum-die-casting-service/` (A380, the title-leading alloy) accepting partial misrepresentation, or (b) accept 404/410, or (c) build a future informational "alloy grade equivalents" hub (out of scope this phase).

### #3 `/forging-casting-vs-cnc-manufacturing-guide/` → **HUMAN_REVIEW**
3-process selection guide where the site offers only 2 of the 3 processes (no forging). No process-selection hub exists. Either single page (casting or CNC) changes the user's expected comparison intent. **Business decision required:** pick `/aluminum-die-casting/` (flagship process) or `/precision-cnc-machining/` or 404/410.

### #4 `/guide-to-types-of-metal-casting-processes/` → **HUMAN_REVIEW**
Broad multi-process overview with no surviving "all casting processes" hub. A defensible `410_OR_404` exists (no overview page = genuinely no equivalent), and an alternative `/aluminum-die-casting/` map is possible but intent-changing. Because both are reasonable and alter intent, **business must choose**. **Note:** this is the strongest `410` candidate of the four (no overview page exists at all).

### #7 `/magnesium-vs-aluminum-die-casting/` → **HUMAN_REVIEW**
Textbook 2-material comparison (spec §6A). Neither single-material page satisfies "the comparison." No dominant-material signal in source. Forcing to either misrepresents. **Business decision required:** pick magnesium, aluminum, or 404/410.

---

## 7. Confidence

The **retention** decision (HUMAN_REVIEW) for each is **HIGH** confidence — the reasons (no single owner passes OWNER TEST; BUSINESS-INTENT SAFETY; spec §6A) are explicit and rule-based, not a guess. A LOW-confidence 301 would, per spec, remain HUMAN_REVIEW rather than be forced; here even the "least-bad" 301 candidates fail the owner test, so retention is the only defensible outcome.

---

## 8. Final 49-URL Reconciliation

Combined with the 39 non-HUMAN_REVIEW decisions from S.6-C.4-I (`29 SEMANTIC_REMAP + 2 NO_EQUIVALENT + 8 SYSTEM`) and the 6 HR→REMAP resolutions from S.6-C.4-J:

| Source | SEMANTIC_REMAP | NO_EQUIVALENT | SYSTEM | HUMAN_REVIEW |
|---|---:|---:|---:|---:|
| S.6-C.4-I (non-HR) | 29 | 2 | 8 | 0 |
| S.6-C.4-J resolved (of 10 HR) | +6 | 0 | 0 | 4 |
| S.6-C.4-K freeze (of 4 HR) | 0 | 0 | 0 | 4 |
| **FINAL** | **35** | **2** | **8** | **4** |

Total = 35 + 2 + 8 + 4 = **49** — matches the unresolved inventory exactly. No forced arithmetic.

Counts are unchanged from S.6-C.4-J because this freeze correctly **retained** all 4 as HUMAN_REVIEW (no reclassification was defensible under the OWNER TEST).

---

## 9. Redirect Safety Checks

- **No new 301 was created this phase.** The freeze leaves the 4 URLs as `HUMAN_REVIEW` (pending business decision). Until then they must **not** be wired to a redirect.
- The existing **35 SEMANTIC_REMAP** targets were already verified in prior phases: each target exists as a built Hugo page, is indexable, has a valid canonical, and is not a staging URL. (Verification provenance: `hugo_pages.json`, S.6-C.4-I §5, S.6-C.4-H live check.)
- The 4 retained URLs: if left un-redirected at cutover they will naturally 404 on the new Hugo origin (acceptable gap pending business decision); if the business later chooses 410, that is a deliberate retirement, not a misdirect.
- **No redirect target is staging** (`diecasting.github.io/alumcasting/` is never a migration target; all targets are production-domain-relative Hugo paths).

---

## 10. Live Verification

Read-only HTTP (this phase), browser UA, unverified-TLS opener:

| URL | HTTP | Final | Canonical | Note |
|---|---:|---|---|---|
| `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | 200 | self | self | live-verified this phase |
| `/forging-casting-vs-cnc-manufacturing-guide/` | 200 | self | self | live-verified this phase |
| `/guide-to-types-of-metal-casting-processes/` | 200* | self | self | *from `wp_crawl.jsonl` (prior crawl; live re-fetch WAF-timed-out this phase but status unchanged) |
| `/magnesium-vs-aluminum-die-casting/` | 200* | self | self | *from `wp_crawl.jsonl` (prior crawl; WAF-timed-out this phase) |

Candidate Hugo targets confirmed present in `content/` (titles in §4). No "proposed target" live check is performed because **no 301 is recommended** for these four.

---

## 11. Explicit Statement — No Writes Occurred

This phase performed **read-only** inspection only. The following were **NOT** modified, created, or deleted:

- No WordPress post/page/redirect changed.
- No Hugo source (`content/`, `layouts/`, `data/`, `static/`, `hugo.yaml`/`config`) changed.
- No `baseURL` changed; no CNAME created.
- No Cloudflare, DNS, GitHub Pages setting, or GitHub Actions workflow changed.
- No redirect was written or removed (WP Redirection rule 3169 and the semi-solid chain remain untouched, read-only evidence only).
- No `git add`, `commit`, `push`, `reset`, `checkout`, `clean`, or `stash` was performed.

The **only** filesystem mutation is the creation of this report (`reports/S.6-C.4-K-final-human-review-freeze.md`), which remains **uncommitted** by design.

---

## 12. Explicit Statement — No Production Systems Changed

- **WordPress production:** unchanged.
- **Cloudflare:** unchanged.
- **DNS:** unchanged.
- **GitHub Pages / Actions:** unchanged (no redeploy, no workflow edit).
- **Hugo site:** unchanged (no rebuild published).
- **Redirects (WP + future Hugo):** none added/removed.

---

## 13. HARD STOP

This phase produced a decision-freeze audit only. **STOP.**

Do NOT proceed to redirect implementation.
Do NOT proceed to baseURL cutover.
Do NOT proceed to Cloudflare/DNS changes.
Do NOT proceed to WordPress production changes.
Do NOT commit or push.

The 4 remaining HUMAN_REVIEW URLs require an explicit **business/product decision** (build future informational/knowledge-architecture comparison hubs, or accept 404/410) before the final 301 map can be frozen. Await explicit authorization for the next phase.
