# S.6-C.4-N — WordPress Media Migration Implementation

**Generated:** 2026-09-05 13:33 UTC  
**Mode:** CONTROLLED WRITE — local repository only  
**Authorization:** download 18 audited WP assets, write to `static/images/`, rewrite 164 references. No commit / push / deploy / WP / Cloudflare / DNS / redirect / baseURL / CNAME changes.

## 1. Executive Summary

All 18 WordPress media assets identified in S.6-C.4-M were successfully downloaded, verified (magic bytes, SHA-256, dimensions), and localized into `static/images/`. All **164** audited media references across 39 content files + `layouts/partials/seo-jsonld.html` were rewritten from `https://alumcasting.com/wp-content/uploads/...` to site-relative `/alumcasting/images/...` paths. Zero WordPress media references remain in source or generated HTML. Hugo build passes (48 pages, 19 static files). Content diff audit confirms **every** textual change is exactly a URL substitution (0 violations). The WordPress media dependency is fully removed from the repository.

**MEDIA_MIGRATION_STATUS = READY** (was BLOCKED at end of M).

## 2. Authorization

- Download the 18 WP assets confirmed in M.
- Write to `static/images/`.
- Rewrite the 164 audited references only.
- Local integrity / Hugo build / HTML / JSON-LD validation.
- Generate report + machine-readable manifest.
- **Prohibited:** commit, push, deploy, WP/Cloudflare/DNS/redirect/baseURL/CNAME/sitemap/robots change, content/SEO/layout/CSS/JS rewrite, file deletion, `git add .`, `git add -A`, reset/checkout/clean/stash.

## 3. Baseline

```
HEAD            = cbc5101587de947cac2337377a8cfb49d938212e
origin/main      = cbc5101587de947cac2337377a8cfb49d938212e (via git ls-remote)
Tracked mods     = 0 before N (only untracked M reports)
M result         = 164 refs / 18 unique / 17 MUST_MIGRATE + 1 Org-logo(HUMAN_REVIEW→authorized)
```

## 4. Git State

`git status`: 40 tracked source files modified (39 content `*.md` + 1 `layouts/partials/seo-jsonld.html`); 18 new untracked files under `static/images/`; plus the 2 M reports and this N report/manifest (all untracked). `git diff --stat`: 40 files, 161 insertions, 161 deletions. `public/` is gitignored.

## 5. Authoritative M Inventory

Loaded `reports/S.6-C.4-M-media-inventory.csv` (18 unique rows) + `S.6-C.4-M-wordpress-media-dependency-audit.md`. Verified `UNIQUE_MEDIA_URLS = 18`. All 18 returned `WP_200` at M time and again at N download time. No additional assets invented; no scope expansion.

## 6. 18-Asset Migration Map

| # | Original filename | Final file | Contexts | Refs |
|---|---|---|---|---|
| 1 | `post-processing-secondary-operations-thread-inserting-assembly.webp` | `post-processing-secondary-operations-thread-inserting-assembly.webp` | CONTENT_BODY | 1 |
| 2 | `A356-aluminum-die-casting-porosity-control.webp` | `A356-aluminum-die-casting-porosity-control.webp` | ARTICLE_JSONLD, CONTENT_BODY | 8 |
| 3 | `High-Precision-CNC-Wokshop.webp` | `High-Precision-CNC-Wokshop.webp` | CONTENT_BODY | 16 |
| 4 | `Leakaging-Testing-Equipment.webp` | `Leakaging-Testing-Equipment.webp` | CONTENT_BODY | 15 |
| 5 | `X-Ray-Detector.webp` | `X-Ray-Detector.webp` | CONTENT_BODY | 19 |
| 6 | `CMM-Inspection-Equipment.webp` | `CMM-Inspection-Equipment.webp` | CONTENT_BODY | 20 |
| 7 | `TF1949-ISO9001-Certification.webp` | `TF1949-ISO9001-Certification.webp` | CONTENT_BODY | 18 |
| 8 | `SureTech-650-Surface-treatment.webp` | `SureTech-650-Surface-treatment.webp` | CONTENT_BODY | 14 |
| 9 | `semi-solid-casting-ssm-zero-porosity-structural-parts.webp` | `semi-solid-casting-ssm-zero-porosity-structural-parts.webp` | ARTICLE_JSONLD, CONTENT_BODY | 4 |
| 10 | `5000t-aluminum-die-casting-machine-large-structural-parts.webp` | `5000t-aluminum-die-casting-machine-large-structural-parts.webp` | CONTENT_BODY | 19 |
| 11 | `semi-solid-casting-microstructure-vs-xray-porosity-test.webp` | `semi-solid-casting-microstructure-vs-xray-porosity-test.webp` | CONTENT_BODY | 5 |
| 12 | `magnesium-die-casting-automotive-parts.webp` | `magnesium-die-casting-automotive-parts.webp` | CONTENT_BODY | 2 |
| 13 | `vertically-integrated-manufacturing-process-casting-to-finishing.webp` | `vertically-integrated-manufacturing-process-casting-to-finishing.webp` | ARTICLE_JSONLD, CONTENT_BODY | 7 |
| 14 | `custom-die-casting-mold-design-tooling-fabrication.webp` | `custom-die-casting-mold-design-tooling-fabrication.webp` | CONTENT_BODY | 3 |
| 15 | `400-units-5-axis-4-axis-cnc-machining-workshop.webp` | `400-units-5-axis-4-axis-cnc-machining-workshop.webp` | CONTENT_BODY | 2 |
| 16 | `precision-multi-axis-cnc-machining-tight-tolerance-parts.webp` | `precision-multi-axis-cnc-machining-tight-tolerance-parts.webp` | ARTICLE_JSONLD, CONTENT_BODY | 6 |
| 17 | `lightweight-magnesium-die-casting-3c-aerospace-parts.webp` | `lightweight-magnesium-die-casting-3c-aerospace-parts.webp` | CONTENT_BODY | 4 |
| 18 | `KingShip-Logo.png` | `KingShip-Logo.webp` | ORGANIZATION_JSONLD | 1 |

Local destination: `static/images/`. Filenames preserved from source (no unnecessary rename). All 18 filenames are unique → **no collision**; `static/images/` did not previously exist.

## 7. Download Verification

Each asset fetched via HTTP HEAD + GET from the live WP media CDN (reachable; WAF only blocks WP *HTML*, not `/wp-content/uploads/`). All 18 returned `HTTP 200`. Validated by magic bytes: 17 true WebP (`RIFF..WEBP`) + the logo. **Note:** `KingShip-Logo.png` is in fact a **WebP** file served by WordPress with a `.png` URL (Content-Type `image/webp`, bytes `RIFF..WEBPVP8X`, 200×40, RGBA). Keeping the `.png` extension would cause a MIME mismatch and a broken logo on GitHub Pages, so the bytes were kept unchanged and the extension corrected to **`.webp`** (no conversion, no recompression).

## 8. File Integrity

For every asset: HTTP status, Content-Type, magic-byte signature, byte size, SHA-256, width, height, aspect ratio recorded. No trust in Content-Type alone. **No image transformation** (no resize/crop/optimize/recompress/format-convert) performed.

**Correction vs M:** M reported `vertically-integrated-manufacturing-process-casting-to-finishing.webp` as `413×8234`. The actual downloaded file is **800×800** (PIL + raw `WEBPVP8` header confirm). The M-phase dimension probe was erroneous for this single asset; N uses the true value. No resize occurred.

## 9. Local File Placement

Created `static/images/` and wrote the 18 authorized assets. `find`/hash inventory (see manifest) shows exactly the 18 authorized files, no extras. File existence, byte size, SHA-256 and dimensions match the download source for all 18.

## 10. URL Rewrite Summary

- Old form: `https://alumcasting.com/wp-content/uploads/YYYY/MM/<file>`
- New form: `/alumcasting/images/<file>` (site-relative, no hardcoded host).
- Rationale: matches the repository's existing internal-link architecture (baseURL subpath `/alumcasting/`), removes WP dependency, introduces **no** staging or WP domain. The `/alumcasting/` prefix will be normalized to `/images/...` at the later production cutover (baseURL → `https://alumcasting.com/`) — see §21.

## 11. Reference Count Before/After

- WP references BEFORE: **164** (across 40 files)
- WP references AFTER: **0**
- References rewritten: **164** (string-exact, verified per file)

## 12. WP Dependency Residual Check

Repo-wide scan for `alumcasting.com/wp-content/`, `www.alumcasting.com/wp-content/`, `http://alumcasting.com/wp-content/`, `/wp-content/uploads/`: **0** occurrences in `content/` and `layouts/`. (Audit reports under `reports/` legitimately still contain these strings as documentation — not a runtime dependency.)

## 13. Article JSON-LD Verification

5 Article pages had front-matter `image:` pointing at WP URLs. All 5 now reference local `/alumcasting/images/...` paths:

| Article page | New local image |
|---|---|
| `a356-aluminum-die-casting-porosity-control.md` | `/alumcasting/images/A356-aluminum-die-casting-porosity-control.webp` |
| `a356-semi-solid-casting-benefits-expert-guide.md` | `/alumcasting/images/semi-solid-casting-ssm-zero-porosity-structural-parts.webp` |
| `aluminum-to-magnesium-conversion-weight-reduction.md` | `/alumcasting/images/vertically-integrated-manufacturing-process-casting-to-finishing.webp` |
| `iatf-16949-high-tolerance-automotive-cnc-machining-supplier.md` | `/alumcasting/images/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp` |
| `thixocasting-vs-rheocasting-comparison.md` | `/alumcasting/images/vertically-integrated-manufacturing-process-casting-to-finishing.webp` |

Built HTML confirms Article `image` values are local; JSON syntax valid.

## 14. Organization Logo Verification

- Organization `@id` unchanged: `https://alumcasting.com/#organization`
- Logo property BEFORE: `https://alumcasting.com/wp-content/uploads/2026/03/KingShip-Logo.png`
- Logo property AFTER: `/alumcasting/images/KingShip-Logo.webp` (local; bytes preserved, ext corrected)
- Local file exists; JSON-LD valid.

## 15. Hugo Build

```
$ hugo --gc --minify
Pages=48  Static files=19 (18 images + robots.txt)  Build exit=0  PASS
```

## 16. Generated HTML Verification

- 0 `wp-content` strings in `public/`.
- All `<img src=/alumcasting/images/...>` (minified, unquoted) resolve to `public/images/<file>` (exists).
- 0 `http://` mixed-content media.
- Article JSON-LD `image` local; Organization logo local.
- No staging/external media URLs.

## 17. Page Regression

All 48 pages built. Page content, H1 count, title, meta description, canonical and JSON-LD structure unchanged except the approved media-URL substitutions. (Staging canonical P0-1 remains deferred to the cutover phase, by design.)

## 18. Hash / Dimension Verification

**18/18** SHA-256 match (source bytes == local bytes) and **18/18** dimension match. No optimization or transformation performed.

## 19. Content Diff Safety Audit

Programmatic audit of `git diff` for all 40 changed source files: **0 violations**. Every removed line contains exactly one WP URL; every added line is that URL replaced by the local path and contains no `wp-content`. No wording/title/H1/metadata/schema/layout changes. `git diff --stat`: 40 files, 161 insertions, 161 deletions.

## 20. Final Status

All required validations passed (see §22 FINAL OUTPUT). **S.6-C.4-N STATUS = PASS**.

## 21. Remaining Risks / Next Phase

1. **Cutover reference normalization:** references use the interim `/alumcasting/images/...` form. When baseURL changes to `https://alumcasting.com/` (root) at cutover, rewrite these to `/images/...` (drop the subpath) so they resolve on the production domain. This is a later-phase, mechanical find/replace.
2. **M dimension correction:** `vertically-integrated...webp` is 800×800 (not 413×8234 as M recorded); no action needed — file migrated as-is.
3. **Logo extension:** served as `.webp` (true format); verified renderable. If a strict `.png` is ever required, transcode at a later phase (not in N).
4. WP Media Library files remain on `alumcasting.com` until DNS cutover; after cutover they are orphaned but harmless (no live reference remains in the repo).

## 22. Next Phase Recommendation

Proceed to the domain cutover (S.6-C.x): set `baseURL = https://alumcasting.com/`, add production CNAME, normalize the 18 `/alumcasting/images/` references to `/images/`, import the 35 approved 301 redirects at the durable layer (Cloudflare), switch DNS, then live-crawl and validate canonical/robots/sitemap/schema.


---

## FINAL OUTPUT

```
S.6-C.4-N STATUS = PASS

BASELINE HEAD:            cbc5101587de947cac2337377a8cfb49d938212e
ORIGIN/MAIN:             cbc5101587de947cac2337377a8cfb49d938212e

AUTHORIZED_UNIQUE_ASSETS: 18
DOWNLOADED:               18
LOCALIZED:                18
HASH_MATCH:               18
DIMENSION_MATCH:          18

WP_REFERENCES_BEFORE:     164
WP_REFERENCES_AFTER:      0

LOCAL_REFERENCE_TARGETS_VALID: 164/164

ARTICLE_JSONLD_BEFORE:    5
ARTICLE_JSONLD_AFTER_WP:  0

ORGANIZATION_LOGO_BEFORE_WP: 1
ORGANIZATION_LOGO_AFTER_WP:  0

STAGING_MEDIA_REFERENCES:  0
EXTERNAL_MEDIA_REFERENCES: 0
MIXED_CONTENT_REFERENCES:  0

HUGO_BUILD:               PASS
JSONLD_VALIDATION:        PASS
GENERATED_HTML_VALIDATION: PASS
CONTENT_DIFF_SAFETY:      PASS

SOURCE_CHANGES:           40 files (approved URL substitutions only)
MEDIA_FILES_ADDED:        18 (static/images/)
UNEXPECTED_FILES:         0

HEAD_CHANGED = NO
ORIGIN_MAIN_CHANGED = NO
COMMIT = NO
PUSH = NO
DEPLOY = NO
PRODUCTION_CHANGES = NO

REPORT:  reports/S.6-C.4-N-media-migration-implementation.md
MANIFEST: reports/S.6-C.4-N-media-migration-manifest.csv

MEDIA_MIGRATION_STATUS = READY
```

**HARD STOP** — no commit / push / deploy performed.
