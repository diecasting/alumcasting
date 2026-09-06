# S.6-C.4-O — Media Migration Verification Report

**MODE:** STRICT READ-ONLY VERIFICATION
**OBJECTIVE:** Verify S.6-C.4-N media migration is real, complete, and safe to enter the production URL / baseURL phase.
**Date:** 2026-09-05
**Repository:** `D:/Workbuddy/2026-08-31-19-30-01/alumcasting` (diecasting/alumcasting, branch main)
**Hugo:** v0.163.3-extended (pinned)

---

## 1. Executive Summary

S.6-C.4-O is a **strict read-only** verification of the S.6-C.4-N media migration. All 22 verification dimensions were exercised against the live working tree, the N manifest, the M inventory, the built `public/` output, and the authoritative WP source bytes.

**Result: S.6-C.4-O = PASS.** Every pass criterion in the FINAL DECISION block is satisfied:

- 18/18 migrated media files exist, are byte-identical to N's downloaded evidence (SHA-256), and have correct dimensions and WebP signature.
- 164/164 audited WP references were localized; 0 WP media references remain anywhere in source or generated output.
- 164/164 local reference targets resolve; 5/5 Article JSON-LD images and 1/1 Organization logo are local; Organization `@id` is unchanged.
- 0 staging media, 0 unexpected external media, 0 mixed-content media references.
- Hugo build PASS (48 pages / 19 static files); `public/` contains all 18 assets with SHA-identical bytes and 0 malformed paths.
- Source diff is limited to media-URL substitutions only (40 files, 0 unrelated changes).
- HEAD and origin (remote) are unchanged; no commit / push / deploy occurred.

`MEDIA_MIGRATION_VERIFICATION_STATUS = READY` for the production baseURL / URL-normalization phase.

---

## 2. Baseline

| Item | Expected | Actual | Status |
|------|----------|--------|--------|
| HEAD | `cbc5101587de947cac2337a8cfb49d938212e` | `cbc5101587de947cac2337a8cfb49d938212e` | PASS |
| origin (remote HEAD via `git ls-remote`) | `cbc5101587de947cac2337a8cfb49d938212e` | `cbc5101587de947cac2337a8cfb49d938212e` | PASS |
| N result — AUTHORIZED_UNIQUE_ASSETS | 18 | 18 | PASS |
| N result — LOCALIZED | 18 | 18 | PASS |
| N result — HASH_MATCH | 18 | 18 | PASS |
| N result — DIMENSION_MATCH | 18 | 18 | PASS |
| N result — WP_REFERENCES_AFTER | 0 | 0 | PASS |
| N result — LOCAL_REFERENCE_TARGETS_VALID | 164/164 | 164/164 | PASS |

The local `origin/main` ref is not materialized in this sandbox clone; the remote HEAD was confirmed equal to the baseline via `git ls-remote origin HEAD`.

---

## 3. Git State

```
HEAD            = cbc5101587de947cac2337a8cfb49d938212e
origin (remote) = cbc5101587de947cac2337a8cfb49d938212e
git status --short:
  40 ×  M  content/*.md  +  layouts/partials/seo-jsonld.html   (N-authorized source URL rewrites)
  ??  static/images/                                        (18 new media files, N-authorized)
  ??  reports/*.md / *.csv                                  (M/N/L audit + redirect reports, untracked)
```

No source modifications beyond the N-authorized set were introduced by O. The O report (`reports/S.6-C.4-O-media-migration-verification.md`) is the only new file added by this phase and is itself a report (permitted).

---

## 4. N Migration Evidence

From `reports/S.6-C.4-N-media-migration-manifest.csv` (18 rows, all `MIGRATED` / `VERIFIED`):

- 18 unique WP media URLs were downloaded to `static/images/`.
- Every `local_sha256` equals its `source_sha256` (byte-identical transfer).
- `source_reference_count` sums to **164**, matching M's discovered reference count.
- The Organization logo (`KingShip-Logo.png`, mislabeled `.png` by WordPress) was correctly identified as WebP (RIFF/WEBPVP8X) and deliberately renamed to `KingShip-Logo.webp` to avoid a GitHub Pages MIME mismatch.

---

## 5. 18 Asset Verification

All 18 files present in `static/images/` as regular files. The complete asset table (Section 24 of this report) lists every file with its SHA-256, dimensions, and reference count. Summary:

- 18/18 files exist.
- 18/18 are regular files.
- 18/18 carry the `.webp` extension.
- 18/18 yield a WebP magic-byte signature (`RIFF` + `WEBP`).
- 18/18 byte sizes equal the N manifest `local_size`.

---

## 6. Hash Verification

A read-only script (`/tmp/o_verify/files.py`) recomputed SHA-256 for every file in `static/images/` and compared against the N manifest `local_sha256`.

- **HASH_MATCH = 18/18.** No byte drift between N's downloaded evidence and the current working-tree files.
- Known correction honored: `KingShip-Logo.webp` SHA `203ede2bed9f9f82f5516e833df65154daaa400074c5af2c732b98904cf1d05e` matches.

---

## 7. Dimension Verification

Dimensions decoded directly from WebP container headers (VP8X / VP8 / VP8L) and compared to N manifest `local_width`/`local_height`.

- **DIMENSION_MATCH = 18/18.**
- `vertically-integrated-manufacturing-process-casting-to-finishing.webp` = **800 × 800** (the M-audit probe misreported 413 × 8234; N corrected this and O independently confirms 800 × 800).
- `KingShip-Logo.webp` = **200 × 40** (WebP RGBA), as recorded by N.

---

## 8. File Type Verification

| Check | Result |
|-------|--------|
| `.webp` extension ↔ WebP signature | 18/18 consistent |
| No `.png` file containing WebP bytes | PASS (the one WebP-in-.png case was renamed to `.webp` by N) |
| `KingShip-Logo.webp` MIME/signature | `image/webp`, `WEBP`, 200×40 — deliberate N correction, confirmed |

---

## 9. 164 Reference Reconciliation

| Metric | Value |
|--------|-------|
| M discovered references | 164 |
| N rewritten references | 164 |
| O verified references (source-level `/alumcasting/images/` occurrences) | 164 |
| Old WP URLs remaining in source | 0 |
| New local references in source | 164 |

Every one of the 164 audited references was converted (old WP URL removed, corresponding local URL present). No reference disappeared without conversion (verified: 0 old WP URLs present, 164 local URLs present, and they map 1:1 through the N manifest).

---

## 10. Local Target Verification

All 164 local media references resolve to a file physically present in `static/images/`:

- **LOCAL_TARGETS_VALID = 164/164.**
- Breakdown by context: CONTENT_BODY = 158, ARTICLE_JSONLD = 5, ORGANIZATION_JSONLD = 1, OTHER = 0 (sum = 164).
- No local reference points to a nonexistent file.

---

## 11. Article JSON-LD Verification

5 Article pages (`schema_type: "Article"`) carry a front-matter `image:` that feeds the Article schema's `image` property:

| Page | Article @id | image | local? | dims |
|------|-------------|-------|-------|------|
| a356-aluminum-die-casting-porosity-control | …#article | /alumcasting/images/A356-aluminum-die-casting-porosity-control.webp | yes | 1200×675 |
| a356-semi-solid-casting-benefits-expert-guide | …#article | /alumcasting/images/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | yes | 734×898 |
| aluminum-to-magnesium-conversion-weight-reduction | …#article | /alumcasting/images/vertically-integrated-manufacturing-process-casting-to-finishing.webp | yes | 800×800 |
| iatf-16949-high-tolerance-automotive-cnc-machining-supplier | …#article | /alumcasting/images/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | yes | 734×898 |
| thixocasting-vs-rheocasting-comparison | …#article | /alumcasting/images/vertically-integrated-manufacturing-process-casting-to-finishing.webp | yes | 800×800 |

- **ARTICLE_JSONLD = 5/5 local.** 4 unique URLs; `vertically-integrated…` shared by 2 articles (matches N/M evidence).
- 0 Article JSON-LD images contain `wp-content`, `alumcasting.com/wp-content`, `staging`, or `http://`.
- Generated JSON-LD parsed cleanly (0 parse errors); 5/5 Article blocks carry a valid local `image`.

---

## 12. Organization Logo Verification

`layouts/partials/seo-jsonld.html`:

- Organization `@id` = `https://alumcasting.com/#organization` — **unchanged**.
- Organization `name` = `AlumCasting`, `legalName`, `url` — **unchanged**.
- `logo` = `/alumcasting/images/KingShip-Logo.webp` — local.
- `static/images/KingShip-Logo.webp` exists, WebP, 200×40 — confirmed.
- Generated JSON-LD: Organization `logo` = `/alumcasting/images/KingShip-Logo.webp`, `@id` unchanged.

---

## 13. Generated Public Media Verification

`hugo --gc --minify` → `public/` (build detail in Section 15).

- `public/images/` contains **18/18** migrated assets; `set(public/images) == set(static/images)`.
- **SHA-256 of every public asset == SHA-256 of its static source** → 0 unexpected transformation. Bytes preserved exactly.
- No Hugo image processing occurred (`Processed images: 0`).

---

## 14. Generated HTML Verification

Scan of all 44 generated HTML files:

- **Generated WP media references = 0.**
- **Generated staging media references = 0** (the only `diecasting.github.io` strings are canonical/og:url, which are explicitly OUT OF SCOPE per Task 9).
- **Generated mixed-content (http://) media references = 0.**
- **Generated local media references = 207 occurrences, 0 missing targets** (207 > 164 because JSON-LD `image` and `srcset` repeat some URLs; all resolve).
- **Malformed media URLs = 0** (no `/alumcasting/alumcasting/`, `//images/`, `/images/images/`, etc.).

---

## 15. Generated JSON-LD Verification

Parsed every `<script type=application/ld+json>` (note: Hugo `--minify` strips attribute quotes, so the unquoted form was matched).

- Article JSON-LD: **5/5 valid**, all `image` local, 0 WP/staging/mixed.
- Organization JSON-LD: **1/1 valid**, `logo` local, `@id` unchanged.
- **JSON parse errors = 0.**

---

## 16. Staging Media Audit

- Source media references to `diecasting.github.io`, `staging`, `localhost`, `127.0.0.1` = **0**.
- Generated media references to the same = **0**.
- Existing canonical/og:url staging references are intentionally deferred (see Section 22) and are NOT counted as media leakage.

---

## 17. External Media Audit

- WP production media references remaining: **0**.
- Unrelated external media references: **0** (no new external `<img>`/srcset/background/JSON-LD logo/og:image/twitter:image beyond the pre-N baseline).
- The only absolute URLs in media contexts are the 18 local `/alumcasting/images/` paths.

---

## 18. Mixed Content Audit

- `http://` inside any image/media reference (source or generated) = **0**.

---

## 19. Source Diff Safety

`git diff` analyzed for all 40 modified source files:

- **Modified files = 40**, all URL-substitution only.
- Total changed lines: 161 removed / 161 added, **0 unrelated**.
- Every removed line contains an `alumcasting.com/wp-content/uploads` URL.
- Every added line contains an `/alumcasting/images/` URL.
- `layouts/partials/seo-jsonld.html` changed **only** the Organization logo URL line; `@id`, `name`, `legalName`, `url`, schema type, and all other schema properties are byte-identical.
- No wording, H1, title, meta-description, canonical, schema-logic, navigation, CSS, or JS changes.

---

## 20. Current URL Prefix

- **CURRENT_MEDIA_PREFIX = `/alumcasting/images/`** for all 164 references (verified 164/164).
- No media URL is hardcoded to the production domain.
- No media URL points to staging.
- This prefix is correct for the current GitHub Pages **project** deployment (`baseURL = https://diecasting.github.io/alumcasting/`). It is intentionally NOT normalized in O.

---

## 21. Future Production URL Conversion Matrix (read-only, preparation only)

DO NOT apply in O. After `baseURL = https://alumcasting.com/`, the project prefix `/alumcasting/` disappears and media paths become `/images/<file>`.

| asset | current_path | future_production_path | source_reference_count |
|-------|--------------|------------------------|-------------------------|
| post-processing-secondary-operations-thread-inserting-assembly.webp | /alumcasting/images/post-processing-secondary-operations-thread-inserting-assembly.webp | /images/post-processing-secondary-operations-thread-inserting-assembly.webp | 1 |
| A356-aluminum-die-casting-porosity-control.webp | /alumcasting/images/A356-aluminum-die-casting-porosity-control.webp | /images/A356-aluminum-die-casting-porosity-control.webp | 8 |
| High-Precision-CNC-Wokshop.webp | /alumcasting/images/High-Precision-CNC-Wokshop.webp | /images/High-Precision-CNC-Wokshop.webp | 16 |
| Leakaging-Testing-Equipment.webp | /alumcasting/images/Leakaging-Testing-Equipment.webp | /images/Leakaging-Testing-Equipment.webp | 15 |
| X-Ray-Detector.webp | /alumcasting/images/X-Ray-Detector.webp | /images/X-Ray-Detector.webp | 19 |
| CMM-Inspection-Equipment.webp | /alumcasting/images/CMM-Inspection-Equipment.webp | /images/CMM-Inspection-Equipment.webp | 20 |
| TF1949-ISO9001-Certification.webp | /alumcasting/images/TF1949-ISO9001-Certification.webp | /images/TF1949-ISO9001-Certification.webp | 18 |
| SureTech-650-Surface-treatment.webp | /alumcasting/images/SureTech-650-Surface-treatment.webp | /images/SureTech-650-Surface-treatment.webp | 14 |
| semi-solid-casting-ssm-zero-porosity-structural-parts.webp | /alumcasting/images/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | /images/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | 4 |
| 5000t-aluminum-die-casting-machine-large-structural-parts.webp | /alumcasting/images/5000t-aluminum-die-casting-machine-large-structural-parts.webp | /images/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 19 |
| semi-solid-casting-microstructure-vs-xray-porosity-test.webp | /alumcasting/images/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | /images/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | 5 |
| magnesium-die-casting-automotive-parts.webp | /alumcasting/images/magnesium-die-casting-automotive-parts.webp | /images/magnesium-die-casting-automotive-parts.webp | 2 |
| vertically-integrated-manufacturing-process-casting-to-finishing.webp | /alumcasting/images/vertically-integrated-manufacturing-process-casting-to-finishing.webp | /images/vertically-integrated-manufacturing-process-casting-to-finishing.webp | 7 |
| custom-die-casting-mold-design-tooling-fabrication.webp | /alumcasting/images/custom-die-casting-mold-design-tooling-fabrication.webp | /images/custom-die-casting-mold-design-tooling-fabrication.webp | 3 |
| 400-units-5-axis-4-axis-cnc-machining-workshop.webp | /alumcasting/images/400-units-5-axis-4-axis-cnc-machining-workshop.webp | /images/400-units-5-axis-4-axis-cnc-machining-workshop.webp | 2 |
| precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | /alumcasting/images/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | /images/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | 6 |
| lightweight-magnesium-die-casting-3c-aerospace-parts.webp | /alumcasting/images/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | /images/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | 4 |
| KingShip-Logo.webp | /alumcasting/images/KingShip-Logo.webp | /images/KingShip-Logo.webp | 1 |

Sum of `source_reference_count` = 164. 18 rows.

---

## 22. P0-1 BaseURL / Canonical Status

- `hugo.toml`: `baseURL = "https://diecasting.github.io/alumcasting/"`, `relativeURLs = false`.
- A comment in `hugo.toml` documents the planned production swap to `https://alumcasting.com/`.
- Current `/alumcasting/images/` is fully compatible with GitHub Pages **project** deployment.
- After `baseURL = https://alumcasting.com/`, the expected future media URL is `/images/<file>` (the `/alumcasting/` project prefix is dropped).
- **P0-1 BASEURL/CANONICAL = DEFERRED.** The current staging canonical/og:url leakage (`https://diecasting.github.io/alumcasting/`) remains intentionally unresolved and is **NOT an O failure**. This phase changes nothing about baseURL or canonical generation.

---

## 23. Final Risk Assessment

| Risk | Likelihood | Impact | Status |
|------|-----------|--------|--------|
| WP media dependency at cutover | None | — | Eliminated (0 references) |
| Broken images after cutover (missing local file) | None | — | 164/164 resolve |
| MIME/format corruption | None | — | 18/18 correct signatures; KingShip-Logo corrected |
| Mixed-content warnings on HTTPS prod | None | — | 0 `http://` media |
| Staging media leakage | None | — | 0 |
| Accidental schema/SEO regression | None | — | Diff safety 0 unrelated |
| Byte drift during build | None | — | public SHA == source SHA |
| Production baseURL swap breaking paths | Low | Medium | Covered by Section 21 matrix; mechanical `/alumcasting/images/` → `/images/` replacement required at cutover |

**Residual action for the next phase (not part of O):** when `baseURL` is switched to root domain, perform the single mechanical prefix replacement `/alumcasting/images/` → `/images/` across the 40 source files (the only change needed; no asset moves).

---

## 24. Complete 18-Asset Table

| id | old WP URL (short) | new local URL | local file | ref count | source SHA-256 (sha256) | dims | fmt |
|----|--------------------|---------------|------------|-----------|--------------------------|------|-----|
| 1 | …/2026/03/post-processing-secondary-operations-thread-inserting-assembly.webp | /alumcasting/images/post-processing-secondary-operations-thread-inserting-assembly.webp | post-processing-secondary-operations-thread-inserting-assembly.webp | 1 | a728307924477b6c94e4796ff2ea2660f424097d1a62304a06da1a6fa2be9b62 | 734×898 | webp |
| 2 | …/2026/06/A356-aluminum-die-casting-porosity-control.webp | /alumcasting/images/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | 8 | 4ee88e987b8a146ae4795a60cc1203455f79de6399dad4051d12a635f3586658 | 1200×675 | webp |
| 3 | …/2026/05/High-Precision-CNC-Wokshop.webp | /alumcasting/images/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | 16 | 709996d309fc908e1087c2c95d5ecb77d2299cfb30a443af0d91fc378b941d7c | 800×600 | webp |
| 4 | …/2026/05/Leakaging-Testing-Equipment.webp | /alumcasting/images/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | 15 | 3c54452aee04302892e01cfa4933da4260f4c3e9df8372b444b79763fa0f51a8 | 800×600 | webp |
| 5 | …/2026/05/X-Ray-Detector.webp | /alumcasting/images/X-Ray-Detector.webp | X-Ray-Detector.webp | 19 | 8836dba3d63f2238224f94827d349d9453fa354a4a245ba200c3a8daf9375c79 | 800×500 | webp |
| 6 | …/2026/05/CMM-Inspection-Equipment.webp | /alumcasting/images/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | 20 | 3e86d34dd90c1c1d15bce1033397897f1f575e9cbf36c6efc8fc012a64b86582 | 800×500 | webp |
| 7 | …/2026/05/TF1949-ISO9001-Certification.webp | /alumcasting/images/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | 18 | cd42312eb9b04653142d1b27feef914dd7486c884eda04abd47e18505a1a549c | 800×500 | webp |
| 8 | …/2026/05/SureTech-650-Surface-treatment.webp | /alumcasting/images/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | 14 | b2729b3436f15d4afc3de503e664c1aa36c7a6d41e7d0e21dd9de1703dbbe31d | 800×600 | webp |
| 9 | …/2026/03/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | /alumcasting/images/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | semi-solid-casting-ssm-zero-porosity-structural-parts.webp | 4 | 56266f5ff6a604c7fe7e83e390d06941871180d8a573593ad7ecab6679ec872b | 734×898 | webp |
| 10 | …/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | /alumcasting/images/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | 19 | bcd9da344c68c69bb2f10cc3aac241e4d5ff4c6d458571d04f7793568a77ed70 | 1920×1080 | webp |
| 11 | …/2026/06/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | /alumcasting/images/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | semi-solid-casting-microstructure-vs-xray-porosity-test.webp | 5 | 427f5da29643c28c85446b94db525e941b6ad5fef9f537d7ef6477bcf2bbc43f | 900×490 | webp |
| 12 | …/2026/06/magnesium-die-casting-automotive-parts.webp | /alumcasting/images/magnesium-die-casting-automotive-parts.webp | magnesium-die-casting-automotive-parts.webp | 2 | f373984c69076176cd57d92b3adb1066ed35618ec14f47e8a8c30f27d7165923 | 900×490 | webp |
| 13 | …/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | /alumcasting/images/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | 7 | f52e557d70eda4f455f443bc127b2d0b645087fc1ff07c237a5ea9abc6482c2a | 800×800 | webp |
| 14 | …/2026/03/custom-die-casting-mold-design-tooling-fabrication.webp | /alumcasting/images/custom-die-casting-mold-design-tooling-fabrication.webp | custom-die-casting-mold-design-tooling-fabrication.webp | 3 | e13d3e6c673d6f5bca49569928887813693cadb67d03740905e475f492b81aa0 | 734×898 | webp |
| 15 | …/2022/10/400-units-5-axis-4-axis-cnc-machining-workshop.webp | /alumcasting/images/400-units-5-axis-4-axis-cnc-machining-workshop.webp | 400-units-5-axis-4-axis-cnc-machining-workshop.webp | 2 | a95317f04f1b8869934be8993ec67bfd6da1d8846f22c0cd926c7aa67e22b489 | 734×898 | webp |
| 16 | …/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | /alumcasting/images/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | 6 | ec954fb47c544aa8e16fd18275ef823e5c753c1f6b4bcc5f97c4c766d90ca5e8 | 734×898 | webp |
| 17 | …/2026/03/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | /alumcasting/images/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | lightweight-magnesium-die-casting-3c-aerospace-parts.webp | 4 | 774a6bc8c088ed69d4461860ea093eca482dd516e2afbf818dd00a48bffed8bc | 734×898 | webp |
| 18 | …/2026/03/KingShip-Logo.png (→ .webp) | /alumcasting/images/KingShip-Logo.webp | KingShip-Logo.webp | 1 | 203ede2bed9f9f82f5516e833df65154daaa400074c5af2c732b98904cf1d05e | 200×40 | webp |

All `source SHA-256` values above equal the verified `local SHA-256` (byte-identical). All dimensions verified by O.

---

## Recommendation

**S.6-C.4-O = PASS.** The S.6-C.4-N media migration is verified real, complete, and internally consistent. The repository is safe to advance to the production baseURL / URL-normalization phase. The only remaining mechanical step at cutover is the prefix replacement `/alumcasting/images/` → `/images/` (covered by the Section 21 matrix). No other media-related work is required. P0-1 (canonical/baseURL) remains deferred and is handled by a separate phase.

**No commit, push, deploy, or production change was performed by O.**
