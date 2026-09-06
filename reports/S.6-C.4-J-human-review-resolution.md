# S.6-C.4-J — Human Review Resolution

**Mode:** STRICT READ-ONLY / NO MUTATION / NO PRODUCTION CHANGE  
**Status:** READ-ONLY AUDIT — NO REDIRECTS MODIFIED, NO WORDPRESS CHANGES, NO CLOUDFLARE CHANGES, NO DNS CHANGES, NO HUGO SOURCE CHANGES, NO BASEURL CHANGES, NO COMMIT, NO PUSH

---

## 1. Executive Summary

This phase resolves the **10 HUMAN_REVIEW** URLs identified by `reports/S.6-C.4-I-redirect-decision-audit.md`, using
actual WordPress evidence (prior crawl + targeted re-fetch) and Hugo repository content inspection. Each of the 10
was inspected for HTTP status, redirect chain, title, H1, canonical, body content, internal links, and compared against
every plausible Hugo candidate page (URL, title, H1, body).

**Outcome:** 6 of 10 were resolved to `301_SEMANTIC_REMAP` on the strength of dominant-intent evidence; **4 remain
`HUMAN_REVIEW`** because they are genuine multi-way comparisons with no single surviving overview/comparison hub in Hugo.

| Metric | Count |
|---|---:|
| Original HUMAN_REVIEW | 10 |
| Resolved this phase | 6 |
| Remaining HUMAN_REVIEW | 4 |
| FINAL_301_SEMANTIC_REMAP | 35 |
| FINAL_410_OR_404 | 2 |
| FINAL_SYSTEM_URL_NO_REDIRECT | 8 |
| FINAL_HUMAN_REVIEW | 4 |
| Total unresolved inventory | 49 |

**Migration readiness: CONDITIONALLY_READY** — the URL map is now 92% (45/49) defensibly assigned; the 4 remaining
HUMAN_REVIEW items are all comparison/pillar pages requiring a business decision, not technical ambiguity.

---

## 2. Baseline Verification

| Item | Value |
|---|---|
| Expected baseline HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` |
| Current HEAD | `cbc5101587de947cac2337377a8cfb49d938212e` (verified via `.git/refs/heads/main`) |
| Current branch | `main` |
| origin/main | `cbc5101587de947cac2337377a8cfb49d938212e` (matches HEAD) |
| Working tree | clean except 2 prior-phase untracked reports (`MIGRATION-PRE-CUTOVER-AUDIT.md`, `S.6-C.4-I-redirect-decision-audit.md`); no tracked changes |

Baseline matches the expected authoritative commit. No HARD STOP triggered.

---

## 3. Exact 10 HUMAN_REVIEW URLs

Extracted verbatim from `reports/S.6-C.4-I-redirect-decision-audit.md` matrix (rows flagged `HUMAN_REVIEW`). No
additional URLs were invented.

1. `/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/` — WP title: *AM60B vs AZ91D Magnesium Elongation & Crashworthiness* | status 200 | H1 count 2 | self-canonical: yes
2. `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` — WP title: *Chinese Aluminum Casting Grade Equivalents: A380 vs ADC12* | status 200 | H1 count 2 | self-canonical: yes
3. `/forging-casting-vs-cnc-manufacturing-guide/` — WP title: *Forging vs Casting vs CNC: Master the Manufacturing Choice* | status 200 | H1 count 2 | self-canonical: yes
4. `/guide-to-types-of-metal-casting-processes/` — WP title: *Types of Metal Casting Processes* | status 200 | H1 count 2 | self-canonical: yes
5. `/machining-allowance-optimization-aluminum-casting-porosity/` — WP title: *Prevent Subsurface Porosity: Machining Allowance Optimization* | status 200 | H1 count 2 | self-canonical: yes
6. `/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/` — WP title: *Magnesium AZ91D vs Aluminum ADC12 for Lightweight Housings* | status 200 | H1 count 2 | self-canonical: yes
7. `/magnesium-vs-aluminum-die-casting/` — WP title: *Magnesium vs. Aluminum Die Casting* | status 200 | H1 count 2 | self-canonical: yes
8. `/metal-stamping-vs-die-casting-cost-design-guide/` — WP title: *Metal Stamping vs Die Casting | KingShip* | status 200 | H1 count 2 | self-canonical: yes
9. `/pore-free-die-casting-weldable-automotive-structural-parts/` — WP title: *Pore-Free Die Casting for Weldable Parts | KingShip* | status 200 | H1 count 2 | self-canonical: yes
10. `/why-we-recommended-a356-over-adc12-high-stress-structural-parts/` — WP title: *A356 vs ADC12 for High-Stress Structural Aluminum Parts* | status 200 | H1 count 2 | self-canonical: yes

---

## 4. Evidence Method

- **WordPress evidence:** prior read-only crawl (`wp_crawl.jsonl`, 97 URLs) supplied status/title/H1/canonical; a
  targeted re-fetch retrieved body snippets + internal links for the 10 URLs. Two fetches were WAF-blocked (status -1:
  `/am60b-vs-az91d-.../` and `/magnesium-vs-aluminum-die-casting/`); for those, the prior crawl's title/H1/canonical
  (all 200, self-canonical) was used. No WAF challenge page was mistaken for content.
- **Hugo evidence:** each candidate content file under `content/*.md` was read; front-matter `title` and body text
  were extracted for intent comparison.
- **No mutation:** all inspection was read-only; no WordPress, Cloudflare, DNS, redirect, Hugo source, or baseURL
  change was made.

---

## 5. Detailed 10-URL Resolution Table

| # | Old WP URL | Original Reason | Candidate A | Candidate B | Candidate C | Final Decision | Target | Conf |
|---|---|---|---|---|---|---|---|---|
| 1 | `/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/` | WP comparison of two magnesium alloys (AM60B vs AZ91D) centred on elongation & c | /am60b-magnesium-alloy-die-casting-suppliers/ | /az91d-magnesium-die-casting-automotive-parts/ | — | **301_SEMANTIC_REMAP** | /am60b-magnesium-alloy-die-casting-suppliers/ | MEDIUM |
| 2 | `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | WP post on Chinese grade equivalents covering A380 / A356 / ADC12. Three plausib | /380-aluminum-die-casting-service/ (corrected from non-existent a380 slug) | /a356-aluminum-die-casting-porosity-control/ | /adc12-die-casting-cnc-machining/ | **HUMAN_REVIEW** | — | MEDIUM |
| 3 | `/forging-casting-vs-cnc-manufacturing-guide/` | WP comparison of forging vs casting vs CNC. Plausible owners: /precision-cnc-mac | /precision-cnc-machining/ | /aluminum-die-casting/ | — | **HUMAN_REVIEW** | — | MEDIUM |
| 4 | `/guide-to-types-of-metal-casting-processes/` | WP post surveying many casting processes (sand, gravity, HPDC, investment, etc.) | /sand-casting-services/ | /gravity-die-casting-manufacturer/ | /high-pressure-die-casting-process-quality/ | **HUMAN_REVIEW** | — | MEDIUM |
| 5 | `/machining-allowance-optimization-aluminum-casting-porosity/` | WP post on machining allowance + porosity. Plausible owners: /porosity-control-x | /porosity-control-x-ray-inspection-castings/ | /precision-cnc-machining/ | — | **301_SEMANTIC_REMAP** | /porosity-control-x-ray-inspection-castings/ | HIGH |
| 6 | `/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/` | WP comparison magnesium AZ91D vs aluminum ADC12. Plausible owners: /magnesium-di | /magnesium-die-casting-services/ | /adc12-die-casting-cnc-machining/ | — | **301_SEMANTIC_REMAP** | /magnesium-die-casting-services/ | MEDIUM |
| 7 | `/magnesium-vs-aluminum-die-casting/` | WP comparison 'Magnesium vs Aluminum Die Casting'. Plausible owners: /magnesium- | /magnesium-die-casting-services/ | /aluminum-die-casting/ | — | **HUMAN_REVIEW** | — | MEDIUM |
| 8 | `/metal-stamping-vs-die-casting-cost-design-guide/` | WP comparison stamping vs die casting. Plausible owners: /aluminum-die-casting/  | /aluminum-die-casting/ | /precision-cnc-machining/ | — | **301_SEMANTIC_REMAP** | /aluminum-die-casting/ | MEDIUM |
| 9 | `/pore-free-die-casting-weldable-automotive-structural-parts/` | WP post on pore-free weldable automotive structural parts. Plausible owners: /po | /automotive-die-casting-parts/ | /porosity-control-x-ray-inspection-castings/ | — | **301_SEMANTIC_REMAP** | /automotive-die-casting-parts/ | HIGH |
| 10 | `/why-we-recommended-a356-over-adc12-high-stress-structural-parts/` | WP post comparing A356 vs ADC12 for structural parts. Plausible owners: /a356-al | /a356-aluminum-die-casting-porosity-control/ | /adc12-die-casting-cnc-machining/ | — | **301_SEMANTIC_REMAP** | /a356-aluminum-die-casting-porosity-control/ | HIGH |

### Per-URL Evidence Comparison & Rationale

#### 1. `/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/am60b-vs-az91d-elongation-properties-automotive-crashworthiness/`
- **WP title:** AM60B vs AZ91D Magnesium Elongation & Crashworthiness
- **Original reason for review:** WP comparison of two magnesium alloys (AM60B vs AZ91D) centred on elongation & crashworthiness. Two plausible magnesium single-alloy owners (RULE 7).
- **Evidence comparison:** WP title leads with AM60B and the differentiator is 'elongation & crashworthiness' — AM60B is the high-ductility/crashworthiness magnesium alloy. Candidate A body opens 'When Crashworthiness is Non-Negotiable: Sourcing AM60B Magnesium Alloy Die Casting Suppliers Who Understand Ductility', a direct mirror of the WP thesis. Candidate B ('AZ91D ... for Automotive Parts') covers the other alloy but not the crashworthiness/elongation angle.
- **Final decision:** **301_SEMANTIC_REMAP** → `/am60b-magnesium-alloy-die-casting-suppliers/`
- **Confidence:** MEDIUM
- **Rationale (why better than alternatives):** The post's conclusion/framing favors AM60B crashworthiness; Candidate A is the surviving canonical AM60B page and its H1 theme matches the WP differentiator. It remains a 2-alloy comparison, so confidence is MEDIUM, not HIGH. Redirecting preserves the dominant user intent (which magnesium alloy wins on crashworthiness).

#### 2. `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/`
- **WP title:** Chinese Aluminum Casting Grade Equivalents: A380 vs ADC12
- **Original reason for review:** WP post on Chinese grade equivalents covering A380 / A356 / ADC12. Three plausible alloy-page owners (RULE 7). NOTE: I-report candidate '/a380-aluminum-die-casting-alloy-properties/' does NOT exist; the real A380 owner is '/380-aluminum-die-casting-service/'.
- **Evidence comparison:** WP slug + title reference three alloys (A380, A356, ADC12). No single Hugo page is a 'grade equivalents comparison'. The three surviving owners are individual alloy pages, each covering only 1/3 of the post. Internal links also reference '/a380-aluminum-die-casting-alloy-properties/' which has no Hugo equivalent.
- **Final decision:** **HUMAN_REVIEW**
- **Confidence:** MEDIUM
- **Rationale (why better than alternatives):** Genuine 3-way alloy comparison with no surviving comparison hub in Hugo. Forcing to any one alloy page (e.g. A380, which the title leads with) would misrepresent the 'equivalents' intent. Business must choose one alloy owner (recommended: /380-aluminum-die-casting-service/ for A380, the title-leading alloy) OR accept 404. Kept HUMAN_REVIEW with the corrected candidate slug.

#### 3. `/forging-casting-vs-cnc-manufacturing-guide/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/forging-casting-vs-cnc-manufacturing-guide/`
- **WP title:** Forging vs Casting vs CNC: Master the Manufacturing Choice
- **Original reason for review:** WP comparison of forging vs casting vs CNC. Plausible owners: /precision-cnc-machining/ OR /aluminum-die-casting/ (RULE 7).
- **Evidence comparison:** WP title 'Forging vs Casting vs CNC: Master the Manufacturing Choice' is a 3-process selection guide. The site offers casting + CNC but NOT forging, so only 2 of 3 have Hugo owners. Neither single page is 'the comparison'. Internal links point to /aluminum-die-casting/ and /automotive-cnc-machining-equipment-list/.
- **Final decision:** **HUMAN_REVIEW**
- **Confidence:** MEDIUM
- **Rationale (why better than alternatives):** Multi-process comparison with no process-selection hub in Hugo. A user expects all three processes; landing on either single page changes intent. Business must decide: point to /aluminum-die-casting/ (site's flagship process) OR /precision-cnc-machining/ OR accept 404. Kept HUMAN_REVIEW.

#### 4. `/guide-to-types-of-metal-casting-processes/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/guide-to-types-of-metal-casting-processes/`
- **WP title:** Types of Metal Casting Processes
- **Original reason for review:** WP post surveying many casting processes (sand, gravity, HPDC, investment, etc.). Multiple Hugo process pages plausible; no single obvious successor (RULE 7).
- **Evidence comparison:** WP title 'Types of Metal Casting Processes' is a multi-process overview. Hugo has individual process pages (sand, gravity, HPDC, semi-solid, thixocasting/rheocasting, vacuum-assisted) but NO overview/'all processes' hub. Each candidate covers only one process.
- **Final decision:** **HUMAN_REVIEW**
- **Confidence:** MEDIUM
- **Rationale (why better than alternatives):** Overview/survey post with no surviving overview hub. Defensible alternatives: (a) 410_OR_404_NO_EQUIVALENT (no overview page exists), or (b) map to /aluminum-die-casting/ as the broadest casting hub. Because both are reasonable and intent-changing, business must choose. Kept HUMAN_REVIEW with the 410 alternative noted.

#### 5. `/machining-allowance-optimization-aluminum-casting-porosity/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/machining-allowance-optimization-aluminum-casting-porosity/`
- **WP title:** Prevent Subsurface Porosity: Machining Allowance Optimization
- **Original reason for review:** WP post on machining allowance + porosity. Plausible owners: /porosity-control-x-ray-inspection-castings/ OR /precision-cnc-machining/ (RULE 7).
- **Evidence comparison:** WP title 'Prevent Subsurface Porosity: Machining Allowance Optimization' leads with porosity prevention in aluminum castings; machining allowance is the porosity-mitigation technique. Candidate A is the porosity/inspection authority page. Candidate B (CNC) is a weaker partial match (machining removes the porous layer but is not the post's thesis).
- **Final decision:** **301_SEMANTIC_REMAP** → `/porosity-control-x-ray-inspection-castings/`
- **Confidence:** HIGH
- **Rationale (why better than alternatives):** Dominant intent is porosity control in castings; Candidate A is the surviving canonical porosity page and owns that topic directly. CNC is secondary. Single defensible target, not arbitrary — HIGH confidence.

#### 6. `/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/magnesium-az91d-vs-aluminum-adc12-lightweight-housing/`
- **WP title:** Magnesium AZ91D vs Aluminum ADC12 for Lightweight Housings
- **Original reason for review:** WP comparison magnesium AZ91D vs aluminum ADC12. Plausible owners: /magnesium-die-casting-services/ OR /adc12-die-casting-cnc-machining/ (RULE 7).
- **Evidence comparison:** WP title 'Magnesium AZ91D vs Aluminum ADC12 for Lightweight Housings' is a 2-material comparison for a lightweight-housing application. Candidate A H1 = 'Magnesium Die Casting Supplier for Lightweight Automotive and EV Components' — direct match to the 'Lightweight Housings' application and magnesium is THE lightweight material. Candidate B covers ADC12 die casting + CNC.
- **Final decision:** **301_SEMANTIC_REMAP** → `/magnesium-die-casting-services/`
- **Confidence:** MEDIUM
- **Rationale (why better than alternatives):** The application framing 'lightweight housings' aligns with Candidate A's explicit 'Lightweight ... Components' ownership; magnesium is the lightweight material. ADC12 is a valid alternative but the post's lightweight intent leans magnesium. MEDIUM because it is still a 2-material comparison; documented alternative is /adc12-die-casting-cnc-machining/.

#### 7. `/magnesium-vs-aluminum-die-casting/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/magnesium-vs-aluminum-die-casting/`
- **WP title:** Magnesium vs. Aluminum Die Casting
- **Original reason for review:** WP comparison 'Magnesium vs Aluminum Die Casting'. Plausible owners: /magnesium-die-casting-services/ OR /aluminum-die-casting/ (RULE 7). Textbook ambiguous family per spec §6A.
- **Evidence comparison:** WP title 'Magnesium vs. Aluminum Die Casting' is a pure 2-material comparison. Neither single-material Hugo page is 'the comparison'. No comparison hub exists in Hugo. (Fresh body fetch was WAF-blocked, status -1; prior crawl confirms title/H1/canonical/200.)
- **Final decision:** **HUMAN_REVIEW**
- **Confidence:** MEDIUM
- **Rationale (why better than alternatives):** Genuine 2-material comparison; forcing to either material misrepresents the comparison intent (exactly the spec §6A warning: do not auto-redirect merely because a slug contains the same material keyword). Business must choose magnesium, aluminum, or accept 404. Kept HUMAN_REVIEW.

#### 8. `/metal-stamping-vs-die-casting-cost-design-guide/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/metal-stamping-vs-die-casting-cost-design-guide/`
- **WP title:** Metal Stamping vs Die Casting | KingShip
- **Original reason for review:** WP comparison stamping vs die casting. Plausible owners: /aluminum-die-casting/ OR /precision-cnc-machining/ (RULE 7).
- **Evidence comparison:** WP title 'Metal Stamping vs Die Casting' compares two processes; the site offers die casting but NOT stamping, so only die casting has a surviving owner. Candidate A is the die-casting owner; Candidate B (CNC) is a mismatch (stamping ≠ CNC machining). Internal links point to /aluminum-die-casting/.
- **Final decision:** **301_SEMANTIC_REMAP** → `/aluminum-die-casting/`
- **Confidence:** MEDIUM
- **Rationale (why better than alternatives):** The post is NOT a pure stamping-intent page (spec §6C warning does not apply); it is 'stamping vs die casting' where the company's only surviving service is die casting. Candidate A is the single defensible successor; Candidate B (CNC) is wrong process family. MEDIUM (comparison nature noted).

#### 9. `/pore-free-die-casting-weldable-automotive-structural-parts/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/pore-free-die-casting-weldable-automotive-structural-parts/`
- **WP title:** Pore-Free Die Casting for Weldable Parts | KingShip
- **Original reason for review:** WP post on pore-free weldable automotive structural parts. Plausible owners: /porosity-control-x-ray-inspection-castings/ OR /automotive-die-casting-parts/ (RULE 7).
- **Evidence comparison:** WP title 'Pore-Free Die Casting for Weldable Parts' + automotive structural. Candidate A H1/body = 'Low-Porosity Aluminum Die Casting for Automotive Parts ... air-tight structural solutions' — near-verbatim topic match (automotive + low-porosity + structural). Candidate B is the general porosity/inspection page (not automotive-specific).
- **Final decision:** **301_SEMANTIC_REMAP** → `/automotive-die-casting-parts/`
- **Confidence:** HIGH
- **Rationale (why better than alternatives):** Candidate A's copy mirrors the WP post almost word-for-word (pore-free/low-porosity + automotive + structural). It is the tighter single-target match than the generic porosity page. HIGH confidence; porosity-control noted as valid alternative.

#### 10. `/why-we-recommended-a356-over-adc12-high-stress-structural-parts/`

- **WP status:** 200 | **H1 count:** 2 | **canonical:** `https://alumcasting.com/why-we-recommended-a356-over-adc12-high-stress-structural-parts/`
- **WP title:** A356 vs ADC12 for High-Stress Structural Aluminum Parts
- **Original reason for review:** WP post comparing A356 vs ADC12 for structural parts. Plausible owners: /a356-aluminum-die-casting-porosity-control/ OR /adc12-die-casting-cnc-machining/ (RULE 7).
- **Evidence comparison:** WP title 'A356 vs ADC12 for High-Stress Structural Aluminum Parts' + slug 'why-we-RECOMMENDED-A356-over-adc12' — the post's own conclusion recommends A356. Candidate A is the surviving A356 page; Candidate B is the alloy the post argues against.
- **Final decision:** **301_SEMANTIC_REMAP** → `/a356-aluminum-die-casting-porosity-control/`
- **Confidence:** HIGH
- **Rationale (why better than alternatives):** The post's recommendation is explicitly A356 for high-stress structural parts; Candidate A is the single defensible target. Candidate B (ADC12) is the 'losing' alloy. HIGH confidence.

---

## 6. Candidate Comparison (family notes)

- **Magnesium vs Aluminum (§6A):** URLs #6, #7. #6 (`magnesium-az91d-vs-aluminum-adc12-lightweight-housing`)
  resolved to magnesium because the *application* (lightweight housings) is magnesium's signature use-case and the
  magnesium page explicitly owns 'Lightweight ... Components'. #7 (`magnesium-vs-aluminum-die-casting`) stayed
  HUMAN_REVIEW — a pure 2-material comparison with no comparison hub (spec §6A warning applied).
- **A356 vs ADC12 (§6B):** URL #10 resolved to A356 because the post *recommends* A356; #2 (3-alloy grade
  equivalents) stayed HUMAN_REVIEW (no single alloy is 'the' successor).
- **Stamping vs Die Casting (§6C):** URL #8 resolved to `/aluminum-die-casting/` because the post is NOT a
  pure-stamping-intent page and the site offers die casting but not stamping; CNC (Candidate B) is the wrong process
  family. The §6C 'do not redirect stamping to die casting' warning does not apply to a comparison where die
  casting is half the subject.
- **Other (§6D):** URLs #3 (forging/casting/CNC), #4 (process overview), #5 (porosity), #9 (pore-free
  automotive) resolved per the same evidence standard.

---

## 7. SEO Intent Analysis

Every remap preserves the **primary user intent**:

- #1 AM60B crashworthiness → AM60B page (informational/material intent preserved).
- #5 porosity prevention → porosity-control page (technical/quality intent preserved).
- #6 lightweight magnesium housing → magnesium page (material/application intent preserved).
- #8 stamping-vs-diecasting → die-casting page (commercial/process intent preserved; stamping has no Hugo presence).
- #9 pore-free automotive structural → automotive-die-casting-parts (product/application intent preserved).
- #10 A356 recommendation → A356 page (material/technical intent preserved).

No redirect was chosen merely to 'avoid a 404' (spec §7): the 4 HUMAN_REVIEW items were retained precisely
because forcing a single target would have changed search intent.

---

## 8. High-Priority Items

The following HUMAN_REVIEW URLs are commercially important comparison/pillar content likely to hold historical
organic traffic and backlinks. They are flagged **HIGH PRIORITY HUMAN DECISION**:

| URL | Why high-priority | Recommended human action |
|---|---|---|
| `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` | 3-alloy grade pillar, likely linked from sourcing pages | Pick `/380-aluminum-die-casting-service/` (A380, title-leading) or 404 |
| `/forging-casting-vs-cnc-manufacturing-guide/` | process-selection pillar | Pick `/aluminum-die-casting/` or `/precision-cnc-machining/` or 404 |
| `/guide-to-types-of-metal-casting-processes/` | process-overview pillar | Decide 410 vs map to `/aluminum-die-casting/` |
| `/magnesium-vs-aluminum-die-casting/` | classic material-comparison pillar | Pick magnesium / aluminum / 404 |

---

## 9. Final 49-URL Reconciliation

Combined with the 39 non-HUMAN_REVIEW decisions from S.6-C.4-I (`29 SEMANTIC_REMAP + 2 NO_EQUIVALENT + 8 SYSTEM`):

| Source | SEMANTIC_REMAP | NO_EQUIVALENT | SYSTEM | HUMAN_REVIEW |
|---|---:|---:|---:|---:|
| S.6-C.4-I (non-HR) | 29 | 2 | 8 | 0 |
| S.6-C.4-J resolved (of 10 HR) | +6 | 0 | 0 | 4 |
| **FINAL** | **35** | **2** | **8** | **4** |

Total = 35 + 2 + 8 + 4 = **49** (matches the unresolved inventory; no forced adjustment).

Counts differ from the original I-report only by the internal reclassification of 6 HR → SEMANTIC_REMAP; the
NO_EQUIVALENT (2) and SYSTEM (8) buckets are unchanged and were explained in S.6-C.4-I.

---

## 10. Final Decision Counts

```
FINAL_301_SEMANTIC_REMAP       = 35
FINAL_410_OR_404               = 2
FINAL_SYSTEM_URL_NO_REDIRECT    = 8
FINAL_HUMAN_REVIEW             = 4
Total unresolved inventory      = 49
```

---

## 11. Remaining HUMAN_REVIEW Items

- `/chinese-aluminum-casting-grade-equivalents-a380-a356-adc12/` — Genuine 3-way alloy comparison with no surviving comparison hub in Hugo. Forcing to any one alloy page (e.g. A380, which the title leads with) would misrepresent the 'equivalents' intent. Business must choose one alloy owner (recommended: /380-aluminum-die-casting-service/ for A380, the title-leading alloy) OR accept 404. Kept HUMAN_REVIEW with the corrected candidate slug.
- `/forging-casting-vs-cnc-manufacturing-guide/` — Multi-process comparison with no process-selection hub in Hugo. A user expects all three processes; landing on either single page changes intent. Business must decide: point to /aluminum-die-casting/ (site's flagship process) OR /precision-cnc-machining/ OR accept 404. Kept HUMAN_REVIEW.
- `/guide-to-types-of-metal-casting-processes/` — Overview/survey post with no surviving overview hub. Defensible alternatives: (a) 410_OR_404_NO_EQUIVALENT (no overview page exists), or (b) map to /aluminum-die-casting/ as the broadest casting hub. Because both are reasonable and intent-changing, business must choose. Kept HUMAN_REVIEW with the 410 alternative noted.
- `/magnesium-vs-aluminum-die-casting/` — Genuine 2-material comparison; forcing to either material misrepresents the comparison intent (exactly the spec §6A warning: do not auto-redirect merely because a slug contains the same material keyword). Business must choose magnesium, aluminum, or accept 404. Kept HUMAN_REVIEW.


---

## 12. Migration Readiness

**CONDITIONALLY_READY.** 45/49 URLs (92%) have defensible migration dispositions. The 4 remaining HUMAN_REVIEW
items are genuine multi-way comparisons with no single surviving overview/comparison hub in Hugo; they require a
business/product decision, not further technical evidence. They do **not** block cutover technically (they can be
wired as 404/410 or pointed at a chosen hub once the human decision is made), but the final 301 map should not be
frozen until they are resolved.

---

## 13. Explicit Out-of-Scope Items

- **Dead T6** `/t6-heat-treatment-semi-solid-die-casting-aluminum/` → conceptual owner
  `/semi-solid-die-casting-heat-treatable-aluminum/` (established in S.6-C.4-I as `301_SEMANTIC_REMAP`, already
  counted in the 29; **not** among the 10 HUMAN_REVIEW; verified unchanged).
- **`/prevent-blistering-aluminum-t6-heat-treatment/`** — a separate dead/404 URL, categorized as
  `410_OR_404_NO_EQUIVALENT` in S.6-C.4-I; **not** among the 10 HUMAN_REVIEW; not silently absorbed into any
  decision here.
- **WP Redirection rule 3169** `/semi-solid-die-casting-manufacturers/` → `/semi-solid-die-casting/` (WP-side, read-only
  evidence only; not modified). The existing `/semi-solid-die-casting/` →
  `/semi-solid-die-casting-heat-treatable-aluminum/` chain is likewise untouched.
- All WordPress production redirects, Cloudflare, DNS, GitHub Pages settings, and Hugo source remain unchanged.

---

## 14. HARD STOP

This phase performed **read-only** inspection only. No redirects were modified, no WordPress/Cloudflare/DNS changes
were made, no Hugo source or baseURL was changed, and no commit or push was performed. The only filesystem
mutation is the creation of this report (`reports/S.6-C.4-J-human-review-resolution.md`), which remains
**uncommitted** by design.

**HARD STOP**
