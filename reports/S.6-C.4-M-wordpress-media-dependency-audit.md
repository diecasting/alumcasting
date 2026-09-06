# S.6-C.4-M — WordPress Media Dependency Migration Audit

**Mode:** READ-ONLY ONLY  ·  **Hard stop honored:** no source/config/static/WP/Cloudflare/DNS/Redirect changes; no commit/push/deploy.
**Generated:** 2026-09-05  ·  **Authoritative baseline HEAD:** `cbc5101587de947cac2337377a8cfb49d938212e`

---

## 1. Executive Summary
This audit recomputes, directly from the current Hugo repository source, every WordPress `wp-content/uploads` media reference that the AlumCasting GitHub-Pages migration still depends on. Unlike the prior pre-cutover estimate (163), the **actual source count is 164 reference occurrences resolving to only 18 unique media assets** — the earlier 163 omitted the Organization-logo URL hard-coded in `layouts/partials/seo-jsonld.html`.

Key findings:
- **164 reference occurrences across 44 Hugo content pages + 1 site-wide layout.**
- **Only 18 UNIQUE media URLs** (17 WebP + 1 PNG logo). The same 18 assets are hot-linked from many pages (max reuse: one equipment photo appears on 20 pages).
- **0 assets exist locally in Hugo** (`static/` holds only `robots.txt`; `assets/` is empty). Every referenced media file lives on the WordPress media CDN.
- **All 18 unique assets return HTTP 200** (live read-only HEAD+GET probe on 2026-09-05). **0 broken, 0 redirected, 0 403/5xx.** Availability is CONFIRMED, not inconclusive.
- **No `og:image` / `twitter:image` / featured-image meta is emitted by the Hugo layouts** (only OG/Twitter *text* fields). Therefore there is **no OG/Twitter/featured-image leakage risk** at cutover — but it also means social cards currently have no image.
- **No staging, no external-domain, no mixed-content (http) references.** All 18 use `https://alumcasting.com`.
- **Migration classification:** 17 MUST_MIGRATE + 1 HUMAN_REVIEW (Organization logo — decide localize vs. retain).
- **MEDIA_MIGRATION_STATUS = BLOCKED** — because 17 assets MUST_MIGRATE (none local), the cutover is blocked on a media-migration step (download 18 files → `static/`, rewrite 164 references).

## 2. Baseline
- Repository: `diecasting/alumcasting` (Hugo 0.163.3-extended), branch `main`.
- HEAD: `cbc5101587de947cac2337377a8cfb49d938212e` (matches frozen S.6-C.4-K baseline).
- `git ls-remote origin HEAD` = `cbc5101587de947cac2337377a8cfb49d938212e` (remote unchanged).
- Scan scope: `content/ layouts/ data/ static/ assets/ config/` + root `hugo.*`. Excluded: generated `public/`, `reports/` (audit docs), `themes/`, `.git/`.

## 3. Git State
```
HEAD            = cbc5101587de947cac2337377a8cfb49d938212e
origin/main      = cbc5101587de947cac2337377a8cfb49d938212e  (via ls-remote)
git status       = clean (only untracked report files; 0 tracked modifications)
git diff --stat  = empty
```

## 4. Total References
- **DISCOVERED_TOTAL = 164** (reference occurrences of WP `wp-content/uploads` URLs in source).
- **UNIQUE_MEDIA_URLS = 18**.
- **DUPLICATE_REFERENCES = 146** (occurrences beyond the first per unique URL).
- Reconciliation vs prior pre-cutover estimate: prior reported 163 (158 content + 5 Article JSON-LD). Recompute from source = 158 CONTENT_BODY + 5 ARTICLE_JSONLD + 1 ORGANIZATION_JSONLD (logo) = **164**. The +1 is the Organization logo in `layouts/partials/seo-jsonld.html`, not previously itemized separately.

## 5. Unique Media URLs
Total unique = 18. Extension breakdown: webp=17, png=1.
All 18 are `https://alumcasting.com/wp-content/uploads/...` (scheme https, host alumcasting.com).

## 6. Source Context Breakdown
(counts are reference occurrences)
- CONTENT_BODY: 158
- ARTICLE_JSONLD: 5
- ORGANIZATION_JSONLD: 1

- CONTENT_BODY = markdown `![alt](url)` / HTML `<img>` in page bodies.
- ARTICLE_JSONLD = front-matter `image:` on the 5 `schema_type: Article` pages (feeds Article schema via `seo-jsonld.html`).
- ORGANIZATION_JSONLD = hard-coded `logo` in `seo-jsonld.html` Organization block.
- FEATURED_IMAGE = 0 (Hugo front matter has no `featured`/`cover` image field in use).
- OG_IMAGE / TWITTER_IMAGE = 0 (no `og:image`/`twitter:image` meta emitted by `head.html`).

## 7. WP Availability Breakdown
(live read-only HEAD + GET probe, 2026-09-05; temp files cleaned)
- WP_200 = 18
- WP_REDIRECT = 0
- WP_404 = 0
- WP_403 = 0
- WP_5XX = 0
- WP_AVAILABILITY_INCONCLUSIVE = 0
- All 18 unique assets are live and reachable from the sandbox (the WP *media* CDN is not behind the WAF that blocks WP *HTML*). No asset is broken or unavailable. Availability is CONFIRMED.

## 8. Hugo Local Match Breakdown
- EXISTS_IN_HUGO = 0
- ALREADY_LOCAL = 0
- NOT_FOUND_LOCAL = 18
- `static/` contains only `robots.txt`; `assets/` is empty; no page bundles carry media. **Every referenced asset is external to Hugo.**

## 9. Duplicate Analysis
A. URL-level duplicate: 146 occurrences are repeats of the 18 unique URLs (same normalized URL referenced from multiple pages).
B. File-level duplicate: no reliable duplicate evidence (no `-scaled`/`-WxH` variant siblings detected within the set; all 18 are distinct base files).
- EXACT_DUPLICATE = 0, LIKELY_DUPLICATE = 0, NOT_DUPLICATE = 18, UNKNOWN = 0.
- Note: two Article pages (`aluminum-to-magnesium-conversion-weight-reduction` and `thixocasting-vs-rheocasting-comparison`) intentionally share the same image URL `vertically-integrated-manufacturing-process-casting-to-finishing.webp` — this is a content-design choice, not a duplicate to de-duplicate.

## 10. Article JSON-LD Image Audit
5 Article pages (schema_type=Article) each carry a front-matter `image:` that feeds the Article JSON-LD `image` field. 4 unique URLs (two articles share one):

| # | Article page | Article @id | image URL | WP status | local? | classification |
|---|---|---|---|---|---|---|
| 1 | /a356-semi-solid-casting-benefits-expert-guide/ | https://alumcasting.com/a356-semi-solid-casting-benefits-expert-guide/#article | https://alumcasting.com/wp-content/uploads/2026/03/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | WP_200 | no | MUST_MIGRATE (P1) |
| 2 | /aluminum-to-magnesium-conversion-weight-reduction/ | https://alumcasting.com/aluminum-to-magnesium-conversion-weight-reduction/#article | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | WP_200 | no | MUST_MIGRATE (P1) |
| 3 | /a356-aluminum-die-casting-porosity-control/ | https://alumcasting.com/a356-aluminum-die-casting-porosity-control/#article | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | WP_200 | no | MUST_MIGRATE (P1) |
| 4 | /thixocasting-vs-rheocasting-comparison/ | https://alumcasting.com/thixocasting-vs-rheocasting-comparison/#article | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | WP_200 | no | MUST_MIGRATE (P1) |
| 5 | /iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ | https://alumcasting.com/iatf-16949-high-tolerance-automotive-cnc-machining-supplier/#article | https://alumcasting.com/wp-content/uploads/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | WP_200 | no | MUST_MIGRATE (P1) |

- All 5 Article images are MUST_MIGRATE (P1). None has a local equivalent.
- 3 of the 5 image URLs are ALSO reused as CONTENT_BODY images on other pages (e.g., the A356 porosity image appears on Home, About, Aluminum Die Casting, etc.). This is normal reuse; migrating the single file satisfies both contexts.

## 11. Featured Image Audit
- FEATURED_IMAGE_WP_MEDIA = 0. No `featured`/`cover` front-matter image field is used; no featured-image leak.
- The only schema-level *image* outside Article is the Organization `logo` (ORGANIZATION_JSONLD) — see Section 13.

## 12. OG / Twitter Image Audit
- OG_IMAGE_WP_MEDIA = 0, TWITTER_IMAGE_WP_MEDIA = 0.
- `layouts/partials/head.html` emits only OG/Twitter *text* fields (og:title, og:description, og:url, twitter:title, twitter:description). There is **no `og:image` / `twitter:image` tag**.
- Consequence: at cutover there is **no OG/Twitter image leakage** (nothing points at WP), BUT social sharing cards currently have no image. Adding an OG image (local) is a recommended future enhancement, outside this audit scope.

## 13. Organization JSON-LD / Logo Audit
- The Organization block in `seo-jsonld.html` hard-codes `logo` = `https://alumcasting.com/wp-content/uploads/2026/03/KingShip-Logo.png` (WP_200, 200x40).
- Classification: **HUMAN_REVIEW** (P1). Decision needed: (a) localize to `static/KingShip-Logo.png` and update the layout, or (b) retain the hot-link if the WP media CDN is kept operational post-cutover. No evidence currently supports CAN_RETAIN, so it is held as HUMAN_REVIEW.
- This logo is emitted on **every page** via Organization JSON-LD; if not migrated it remains a WP dependency site-wide.

## 14. Alt Text Audit
- ALT_PRESENT: 154
- ALT_MISSING: 9
- ALT_EMPTY: 1

- 154 body images have descriptive alt text (good).
- ALT_MISSING (9): 5 Article-JSON-LD images (schema has no alt) + 1 Organization logo + 3 body images lacking alt. Recommend adding alt to the 3 body images during migration.

## 15. Staging / External / Mixed-Content Audit
- STAGING_MEDIA_REFERENCES = 0
- EXTERNAL_MEDIA_REFERENCES = 0
- MIXED_CONTENT_REFERENCES = 0
- All 18 references use `https://alumcasting.com/wp-content/uploads/...`. No `diecasting.github.io`, staging, localhost, or `http://` (mixed-content) references detected.

## 16. Page-Level Impact
Pages ordered by number of unique WP-media dependencies (top of risk):

| Page (production-relative) | ref occurrences | unique media | MUST_MIGRATE | HUMAN_REVIEW | Article JSON-LD |
|---|---|---|---|---|---|
| /aluminum-die-casting/ | 10 | 10 | 10 | 0 | 0 |
| /a356-aluminum-die-casting-porosity-control/ | 8 | 7 | 8 | 0 | 1 |
| /a383-aluminum-die-casting-service/ | 7 | 7 | 7 | 0 | 0 |
| /am60b-magnesium-alloy-die-casting-suppliers/ | 7 | 7 | 7 | 0 | 0 |
| /automotive-cnc-machining-equipment-list/ | 7 | 7 | 7 | 0 | 0 |
| /az91d-magnesium-die-casting-automotive-parts/ | 7 | 7 | 7 | 0 | 0 |
| /cold-chamber-die-casting-services/ | 7 | 7 | 7 | 0 | 0 |
| /die-cast-aluminum-electric-motor-housing-suppliers/ | 7 | 7 | 7 | 0 | 0 |
| /manufacturing-capabilities/ | 7 | 7 | 7 | 0 | 0 |
| /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | 7 | 7 | 7 | 0 | 0 |
| / | 7 | 6 | 7 | 0 | 0 |
| /adc12-die-casting-cnc-machining/ | 6 | 6 | 6 | 0 | 0 |
| /explosion-proof-aluminum-enclosures-oil-gas-industry/ | 6 | 6 | 6 | 0 | 0 |
| /magnesium-die-casting-services/ | 7 | 6 | 7 | 0 | 0 |
| /precision-cnc-machining/ | 6 | 6 | 6 | 0 | 0 |
| /about-alumcasting-die-casting-expert/ | 5 | 5 | 5 | 0 | 0 |
| /large-scale-5000t-aluminum-die-casting-factory-china/ | 5 | 5 | 5 | 0 | 0 |
| /maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | 5 | 5 | 5 | 0 | 0 |
| /services/ | 5 | 5 | 5 | 0 | 0 |
| /a356-semi-solid-casting-benefits-expert-guide/ | 5 | 4 | 5 | 0 | 1 |
| /die-casting-tooling/ | 4 | 4 | 4 | 0 | 0 |
| /custom-aluminum-die-casting-for-ev-powertrain-components/ | 2 | 2 | 2 | 0 | 0 |
| /ev-battery-housing-die-casting/ | 2 | 2 | 2 | 0 | 0 |
| /high-pressure-die-casting-process-quality/ | 2 | 2 | 2 | 0 | 0 |
| /high-tolerance-automotive-cnc-machining/ | 2 | 2 | 2 | 0 | 0 |
| /liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/ | 2 | 2 | 2 | 0 | 0 |
| /semi-solid-die-casting-heat-treatable-aluminum/ | 2 | 2 | 2 | 0 | 0 |
| /thin-wall-die-casting-tooling/ | 2 | 2 | 2 | 0 | 0 |
| /380-aluminum-die-casting-service/ | 1 | 1 | 1 | 0 | 0 |
| /aluminum-to-magnesium-conversion-weight-reduction/ | 2 | 1 | 2 | 0 | 1 |
| /automotive-die-casting-parts/ | 1 | 1 | 1 | 0 | 0 |
| /gravity-die-casting-manufacturer/ | 1 | 1 | 1 | 0 | 0 |
| /iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ | 2 | 1 | 2 | 0 | 1 |
| /medical-device-component-machining/ | 1 | 1 | 1 | 0 | 0 |
| /porosity-control-x-ray-inspection-castings/ | 1 | 1 | 1 | 0 | 0 |
| /sand-casting-services/ | 1 | 1 | 1 | 0 | 0 |
| /stainless-steel-precision-machining/ | 1 | 1 | 1 | 0 | 0 |
| /surface-finishing-aluminum-magnesium-casting/ | 1 | 1 | 1 | 0 | 0 |
| /thixocasting-vs-rheocasting-comparison/ | 2 | 1 | 2 | 0 | 1 |

- Every one of the ~44 published pages depends on ≥1 WP media asset.
- Highest-reuse asset: `CMM-Inspection-Equipment.webp` (20 pages), `X-Ray-Detector.webp` (19), `5000t-aluminum-die-casting-machine...webp` (19).
- The Home page (`/`) and several pillar pages reuse the same shared equipment/process photos, which keeps the *unique* migration set small (18).

## 17. Full Media Inventory (per reference occurrence)
All 164 occurrences. Columns: ID, source_file, line, page, context, normalized_url, filename, ext, wp_status, local_match, duplicate_status, dims, file_size, alt_status, classification, risk.

| ID | source_file | line | page | context | normalized_url | filename | ext | wp | local | dup | dims | size | alt | class | risk |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | content/380-aluminum-die-casting-service.md | 36 | /380-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/post-processing-secondary-operations-thread-inserting-assembly.webp | post-processing-secondary-operations-thread-inserting-assembly.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 45450 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 2 | content/a356-aluminum-die-casting-porosity-control.md | 16 | /a356-aluminum-die-casting-porosity-control/ | ARTICLE_JSONLD | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_MISSING | MUST_MIGRATE | P1 |
| 3 | content/a356-aluminum-die-casting-porosity-control.md | 30 | /a356-aluminum-die-casting-porosity-control/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 4 | content/a356-aluminum-die-casting-porosity-control.md | 39 | /a356-aluminum-die-casting-porosity-control/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 5 | content/a356-aluminum-die-casting-porosity-control.md | 54 | /a356-aluminum-die-casting-porosity-control/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 6 | content/a356-aluminum-die-casting-porosity-control.md | 61 | /a356-aluminum-die-casting-porosity-control/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 7 | content/a356-aluminum-die-casting-porosity-control.md | 61 | /a356-aluminum-die-casting-porosity-control/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_MISSING | MUST_MIGRATE | P2 |
| 8 | content/a356-aluminum-die-casting-porosity-control.md | 61 | /a356-aluminum-die-casting-porosity-control/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_MISSING | MUST_MIGRATE | P2 |
| 9 | content/a356-aluminum-die-casting-porosity-control.md | 61 | /a356-aluminum-die-casting-porosity-control/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_MISSING | MUST_MIGRATE | P2 |
| 10 | content/a356-semi-solid-casting-benefits-expert-guide.md | 16 | /a356-semi-solid-casting-benefits-expert-guide/ | ARTICLE_JSONLD | https://alumcasting.com/wp-content/uploads/2026/03/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | semi-solid-casting-ssm-zero-porosity-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 44104 | ALT_MISSING | MUST_MIGRATE | P1 |
| 11 | content/a356-semi-solid-casting-benefits-expert-guide.md | 41 | /a356-semi-solid-casting-benefits-expert-guide/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | semi-solid-casting-ssm-zero-porosity-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 44104 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 12 | content/a356-semi-solid-casting-benefits-expert-guide.md | 68 | /a356-semi-solid-casting-benefits-expert-guide/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 13 | content/a356-semi-solid-casting-benefits-expert-guide.md | 73 | /a356-semi-solid-casting-benefits-expert-guide/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 14 | content/a356-semi-solid-casting-benefits-expert-guide.md | 76 | /a356-semi-solid-casting-benefits-expert-guide/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 15 | content/a383-aluminum-die-casting-service.md | 29 | /a383-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 16 | content/a383-aluminum-die-casting-service.md | 44 | /a383-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 17 | content/a383-aluminum-die-casting-service.md | 53 | /a383-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 18 | content/a383-aluminum-die-casting-service.md | 56 | /a383-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 19 | content/a383-aluminum-die-casting-service.md | 61 | /a383-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 20 | content/a383-aluminum-die-casting-service.md | 68 | /a383-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 21 | content/a383-aluminum-die-casting-service.md | 73 | /a383-aluminum-die-casting-service/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 22 | content/about-alumcasting-die-casting-expert.md | 39 | /about-alumcasting-die-casting-expert/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 23 | content/about-alumcasting-die-casting-expert.md | 60 | /about-alumcasting-die-casting-expert/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 24 | content/about-alumcasting-die-casting-expert.md | 94 | /about-alumcasting-die-casting-expert/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 25 | content/about-alumcasting-die-casting-expert.md | 113 | /about-alumcasting-die-casting-expert/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 26 | content/about-alumcasting-die-casting-expert.md | 138 | /about-alumcasting-die-casting-expert/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 27 | content/adc12-die-casting-cnc-machining.md | 61 | /adc12-die-casting-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 28 | content/adc12-die-casting-cnc-machining.md | 66 | /adc12-die-casting-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 29 | content/adc12-die-casting-cnc-machining.md | 71 | /adc12-die-casting-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 30 | content/adc12-die-casting-cnc-machining.md | 76 | /adc12-die-casting-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 31 | content/adc12-die-casting-cnc-machining.md | 101 | /adc12-die-casting-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 32 | content/adc12-die-casting-cnc-machining.md | 105 | /adc12-die-casting-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 33 | content/aluminum-die-casting.md | 32 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 34 | content/aluminum-die-casting.md | 100 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 35 | content/aluminum-die-casting.md | 106 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 36 | content/aluminum-die-casting.md | 112 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 37 | content/aluminum-die-casting.md | 124 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 38 | content/aluminum-die-casting.md | 149 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 39 | content/aluminum-die-casting.md | 159 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | semi-solid-casting-microstructure-vs-xray-porosity-test.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 900x490 | 53356 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 40 | content/aluminum-die-casting.md | 165 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/magnesium-die-casting-automotive-parts.webp | magnesium-die-casting-automotive-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 900x490 | 33284 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 41 | content/aluminum-die-casting.md | 182 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 42 | content/aluminum-die-casting.md | 188 | /aluminum-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 43 | content/aluminum-to-magnesium-conversion-weight-reduction.md | 16 | /aluminum-to-magnesium-conversion-weight-reduction/ | ARTICLE_JSONLD | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 413x8234 | 117970 | ALT_MISSING | MUST_MIGRATE | P1 |
| 44 | content/aluminum-to-magnesium-conversion-weight-reduction.md | 29 | /aluminum-to-magnesium-conversion-weight-reduction/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 413x8234 | 117970 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 45 | content/am60b-magnesium-alloy-die-casting-suppliers.md | 34 | /am60b-magnesium-alloy-die-casting-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 46 | content/am60b-magnesium-alloy-die-casting-suppliers.md | 43 | /am60b-magnesium-alloy-die-casting-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 47 | content/am60b-magnesium-alloy-die-casting-suppliers.md | 50 | /am60b-magnesium-alloy-die-casting-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 48 | content/am60b-magnesium-alloy-die-casting-suppliers.md | 53 | /am60b-magnesium-alloy-die-casting-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 49 | content/am60b-magnesium-alloy-die-casting-suppliers.md | 62 | /am60b-magnesium-alloy-die-casting-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 50 | content/am60b-magnesium-alloy-die-casting-suppliers.md | 65 | /am60b-magnesium-alloy-die-casting-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 51 | content/am60b-magnesium-alloy-die-casting-suppliers.md | 70 | /am60b-magnesium-alloy-die-casting-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 52 | content/automotive-cnc-machining-equipment-list.md | 22 | /automotive-cnc-machining-equipment-list/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 53 | content/automotive-cnc-machining-equipment-list.md | 29 | /automotive-cnc-machining-equipment-list/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 54 | content/automotive-cnc-machining-equipment-list.md | 36 | /automotive-cnc-machining-equipment-list/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 55 | content/automotive-cnc-machining-equipment-list.md | 43 | /automotive-cnc-machining-equipment-list/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 56 | content/automotive-cnc-machining-equipment-list.md | 54 | /automotive-cnc-machining-equipment-list/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 57 | content/automotive-cnc-machining-equipment-list.md | 78 | /automotive-cnc-machining-equipment-list/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 58 | content/automotive-cnc-machining-equipment-list.md | 88 | /automotive-cnc-machining-equipment-list/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 59 | content/automotive-die-casting-parts.md | 54 | /automotive-die-casting-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | semi-solid-casting-ssm-zero-porosity-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 44104 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 60 | content/az91d-magnesium-die-casting-automotive-parts.md | 34 | /az91d-magnesium-die-casting-automotive-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 61 | content/az91d-magnesium-die-casting-automotive-parts.md | 47 | /az91d-magnesium-die-casting-automotive-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 62 | content/az91d-magnesium-die-casting-automotive-parts.md | 61 | /az91d-magnesium-die-casting-automotive-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 63 | content/az91d-magnesium-die-casting-automotive-parts.md | 64 | /az91d-magnesium-die-casting-automotive-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 64 | content/az91d-magnesium-die-casting-automotive-parts.md | 71 | /az91d-magnesium-die-casting-automotive-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 65 | content/az91d-magnesium-die-casting-automotive-parts.md | 74 | /az91d-magnesium-die-casting-automotive-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 66 | content/az91d-magnesium-die-casting-automotive-parts.md | 79 | /az91d-magnesium-die-casting-automotive-parts/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 67 | content/cold-chamber-die-casting-services.md | 35 | /cold-chamber-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 68 | content/cold-chamber-die-casting-services.md | 72 | /cold-chamber-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 69 | content/cold-chamber-die-casting-services.md | 75 | /cold-chamber-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 70 | content/cold-chamber-die-casting-services.md | 78 | /cold-chamber-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 71 | content/cold-chamber-die-casting-services.md | 91 | /cold-chamber-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 72 | content/cold-chamber-die-casting-services.md | 94 | /cold-chamber-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 73 | content/cold-chamber-die-casting-services.md | 97 | /cold-chamber-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 74 | content/custom-aluminum-die-casting-for-ev-powertrain-components.md | 38 | /custom-aluminum-die-casting-for-ev-powertrain-components/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/custom-die-casting-mold-design-tooling-fabrication.webp | custom-die-casting-mold-design-tooling-fabrication.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 67246 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 75 | content/custom-aluminum-die-casting-for-ev-powertrain-components.md | 41 | /custom-aluminum-die-casting-for-ev-powertrain-components/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/400-units-5-axis-4-axis-cnc-machining-workshop.webp | 400-units-5-axis-4-axis-cnc-machining-workshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 31468 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 76 | content/die-cast-aluminum-electric-motor-housing-suppliers.md | 43 | /die-cast-aluminum-electric-motor-housing-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 77 | content/die-cast-aluminum-electric-motor-housing-suppliers.md | 48 | /die-cast-aluminum-electric-motor-housing-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 78 | content/die-cast-aluminum-electric-motor-housing-suppliers.md | 53 | /die-cast-aluminum-electric-motor-housing-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 79 | content/die-cast-aluminum-electric-motor-housing-suppliers.md | 72 | /die-cast-aluminum-electric-motor-housing-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 80 | content/die-cast-aluminum-electric-motor-housing-suppliers.md | 76 | /die-cast-aluminum-electric-motor-housing-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 81 | content/die-cast-aluminum-electric-motor-housing-suppliers.md | 80 | /die-cast-aluminum-electric-motor-housing-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 82 | content/die-cast-aluminum-electric-motor-housing-suppliers.md | 84 | /die-cast-aluminum-electric-motor-housing-suppliers/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | semi-solid-casting-microstructure-vs-xray-porosity-test.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 900x490 | 53356 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 83 | content/die-casting-tooling.md | 31 | /die-casting-tooling/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 84 | content/die-casting-tooling.md | 100 | /die-casting-tooling/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 85 | content/die-casting-tooling.md | 106 | /die-casting-tooling/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 86 | content/die-casting-tooling.md | 112 | /die-casting-tooling/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 87 | content/ev-battery-housing-die-casting.md | 23 | /ev-battery-housing-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 88 | content/ev-battery-housing-die-casting.md | 39 | /ev-battery-housing-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 413x8234 | 117970 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 89 | content/explosion-proof-aluminum-enclosures-oil-gas-industry.md | 41 | /explosion-proof-aluminum-enclosures-oil-gas-industry/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 90 | content/explosion-proof-aluminum-enclosures-oil-gas-industry.md | 46 | /explosion-proof-aluminum-enclosures-oil-gas-industry/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 91 | content/explosion-proof-aluminum-enclosures-oil-gas-industry.md | 51 | /explosion-proof-aluminum-enclosures-oil-gas-industry/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 92 | content/explosion-proof-aluminum-enclosures-oil-gas-industry.md | 73 | /explosion-proof-aluminum-enclosures-oil-gas-industry/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 93 | content/explosion-proof-aluminum-enclosures-oil-gas-industry.md | 77 | /explosion-proof-aluminum-enclosures-oil-gas-industry/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 94 | content/explosion-proof-aluminum-enclosures-oil-gas-industry.md | 81 | /explosion-proof-aluminum-enclosures-oil-gas-industry/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | semi-solid-casting-microstructure-vs-xray-porosity-test.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 900x490 | 53356 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 95 | content/gravity-die-casting-manufacturer.md | 30 | /gravity-die-casting-manufacturer/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 413x8234 | 117970 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 96 | content/high-pressure-die-casting-process-quality.md | 24 | /high-pressure-die-casting-process-quality/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 97 | content/high-pressure-die-casting-process-quality.md | 37 | /high-pressure-die-casting-process-quality/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 413x8234 | 117970 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 98 | content/high-tolerance-automotive-cnc-machining.md | 65 | /high-tolerance-automotive-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 60084 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 99 | content/high-tolerance-automotive-cnc-machining.md | 70 | /high-tolerance-automotive-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | lightweight-magnesium-die-casting-3c-aerospace-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 72658 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 100 | content/iatf-16949-high-tolerance-automotive-cnc-machining-supplier.md | 16 | /iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ | ARTICLE_JSONLD | https://alumcasting.com/wp-content/uploads/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 60084 | ALT_MISSING | MUST_MIGRATE | P1 |
| 101 | content/iatf-16949-high-tolerance-automotive-cnc-machining-supplier.md | 38 | /iatf-16949-high-tolerance-automotive-cnc-machining-supplier/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 60084 | ALT_EMPTY | MUST_MIGRATE | P1 |
| 102 | content/large-scale-5000t-aluminum-die-casting-factory-china.md | 51 | /large-scale-5000t-aluminum-die-casting-factory-china/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 103 | content/large-scale-5000t-aluminum-die-casting-factory-china.md | 56 | /large-scale-5000t-aluminum-die-casting-factory-china/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 104 | content/large-scale-5000t-aluminum-die-casting-factory-china.md | 61 | /large-scale-5000t-aluminum-die-casting-factory-china/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 105 | content/large-scale-5000t-aluminum-die-casting-factory-china.md | 66 | /large-scale-5000t-aluminum-die-casting-factory-china/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 106 | content/large-scale-5000t-aluminum-die-casting-factory-china.md | 71 | /large-scale-5000t-aluminum-die-casting-factory-china/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 107 | content/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles.md | 38 | /liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/custom-die-casting-mold-design-tooling-fabrication.webp | custom-die-casting-mold-design-tooling-fabrication.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 67246 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 108 | content/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles.md | 41 | /liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/400-units-5-axis-4-axis-cnc-machining-workshop.webp | 400-units-5-axis-4-axis-cnc-machining-workshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 31468 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 109 | content/magnesium-die-casting-services.md | 28 | /magnesium-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/magnesium-die-casting-automotive-parts.webp | magnesium-die-casting-automotive-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 900x490 | 33284 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 110 | content/magnesium-die-casting-services.md | 68 | /magnesium-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 111 | content/magnesium-die-casting-services.md | 82 | /magnesium-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 112 | content/magnesium-die-casting-services.md | 129 | /magnesium-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 113 | content/magnesium-die-casting-services.md | 150 | /magnesium-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 114 | content/magnesium-die-casting-services.md | 167 | /magnesium-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 115 | content/magnesium-die-casting-services.md | 194 | /magnesium-die-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 116 | content/manufacturing-capabilities.md | 48 | /manufacturing-capabilities/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 117 | content/manufacturing-capabilities.md | 62 | /manufacturing-capabilities/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | semi-solid-casting-microstructure-vs-xray-porosity-test.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 900x490 | 53356 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 118 | content/manufacturing-capabilities.md | 76 | /manufacturing-capabilities/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 119 | content/manufacturing-capabilities.md | 91 | /manufacturing-capabilities/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 120 | content/manufacturing-capabilities.md | 106 | /manufacturing-capabilities/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 121 | content/manufacturing-capabilities.md | 119 | /manufacturing-capabilities/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 122 | content/manufacturing-capabilities.md | 121 | /manufacturing-capabilities/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 123 | content/maximum-wall-thickness-projected-area-limits-5000t-die-casting.md | 26 | /maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 124 | content/maximum-wall-thickness-projected-area-limits-5000t-die-casting.md | 44 | /maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 125 | content/maximum-wall-thickness-projected-area-limits-5000t-die-casting.md | 62 | /maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 126 | content/maximum-wall-thickness-projected-area-limits-5000t-die-casting.md | 67 | /maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 127 | content/maximum-wall-thickness-projected-area-limits-5000t-die-casting.md | 78 | /maximum-wall-thickness-projected-area-limits-5000t-die-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 128 | content/medical-device-component-machining.md | 28 | /medical-device-component-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | lightweight-magnesium-die-casting-3c-aerospace-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 72658 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 129 | content/porosity-control-x-ray-inspection-castings.md | 51 | /porosity-control-x-ray-inspection-castings/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 60084 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 130 | content/precision-cnc-machining.md | 45 | /precision-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 131 | content/precision-cnc-machining.md | 87 | /precision-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 132 | content/precision-cnc-machining.md | 140 | /precision-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 133 | content/precision-cnc-machining.md | 146 | /precision-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 134 | content/precision-cnc-machining.md | 152 | /precision-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 135 | content/precision-cnc-machining.md | 158 | /precision-cnc-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 136 | content/sand-casting-services.md | 28 | /sand-casting-services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/semi-solid-casting-ssm-zero-porosity-structural-parts.webp | semi-solid-casting-ssm-zero-porosity-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 44104 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 137 | content/semi-solid-die-casting-heat-treatable-aluminum.md | 51 | /semi-solid-die-casting-heat-treatable-aluminum/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/semi-solid-casting-microstructure-vs-xray-porosity-test.webp | semi-solid-casting-microstructure-vs-xray-porosity-test.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 900x490 | 53356 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 138 | content/semi-solid-die-casting-heat-treatable-aluminum.md | 62 | /semi-solid-die-casting-heat-treatable-aluminum/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 60084 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 139 | content/services.md | 34 | /services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 140 | content/services.md | 123 | /services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 141 | content/services.md | 129 | /services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 142 | content/services.md | 131 | /services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 143 | content/services.md | 141 | /services/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 144 | content/stainless-steel-precision-machining.md | 26 | /stainless-steel-precision-machining/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | lightweight-magnesium-die-casting-3c-aerospace-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 72658 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 145 | content/surface-finishing-aluminum-magnesium-casting.md | 81 | /surface-finishing-aluminum-magnesium-casting/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/lightweight-magnesium-die-casting-3c-aerospace-parts.webp | lightweight-magnesium-die-casting-3c-aerospace-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 72658 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 146 | content/thin-wall-die-casting-tooling.md | 45 | /thin-wall-die-casting-tooling/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/custom-die-casting-mold-design-tooling-fabrication.webp | custom-die-casting-mold-design-tooling-fabrication.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 67246 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 147 | content/thin-wall-die-casting-tooling.md | 46 | /thin-wall-die-casting-tooling/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | precision-multi-axis-cnc-machining-tight-tolerance-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 734x898 | 60084 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 148 | content/thixocasting-vs-rheocasting-comparison.md | 16 | /thixocasting-vs-rheocasting-comparison/ | ARTICLE_JSONLD | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 413x8234 | 117970 | ALT_MISSING | MUST_MIGRATE | P1 |
| 149 | content/thixocasting-vs-rheocasting-comparison.md | 29 | /thixocasting-vs-rheocasting-comparison/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/03/vertically-integrated-manufacturing-process-casting-to-finishing.webp | vertically-integrated-manufacturing-process-casting-to-finishing.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 413x8234 | 117970 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 150 | content/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness.md | 24 | /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 151 | content/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness.md | 38 | /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 152 | content/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness.md | 49 | /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/Leakaging-Testing-Equipment.webp | Leakaging-Testing-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 34910 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 153 | content/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness.md | 54 | /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 154 | content/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness.md | 57 | /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 155 | content/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness.md | 62 | /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/SureTech-650-Surface-treatment.webp | SureTech-650-Surface-treatment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 67908 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 156 | content/vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness.md | 65 | /vacuum-assisted-die-casting-vs-conventional-hpdc-air-tightness/ | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 157 | content/_index.md | 60 | / | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/06/A356-aluminum-die-casting-porosity-control.webp | A356-aluminum-die-casting-porosity-control.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1200x675 | 19688 | ALT_PRESENT | MUST_MIGRATE | P1 |
| 158 | content/_index.md | 65 | / | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2022/10/5000t-aluminum-die-casting-machine-large-structural-parts.webp | 5000t-aluminum-die-casting-machine-large-structural-parts.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 1920x1080 | 77526 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 159 | content/_index.md | 69 | / | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 160 | content/_index.md | 108 | / | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/TF1949-ISO9001-Certification.webp | TF1949-ISO9001-Certification.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 25708 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 161 | content/_index.md | 114 | / | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/CMM-Inspection-Equipment.webp | CMM-Inspection-Equipment.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 21238 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 162 | content/_index.md | 118 | / | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/X-Ray-Detector.webp | X-Ray-Detector.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x500 | 29068 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 163 | content/_index.md | 122 | / | CONTENT_BODY | https://alumcasting.com/wp-content/uploads/2026/05/High-Precision-CNC-Wokshop.webp | High-Precision-CNC-Wokshop.webp | webp | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 800x600 | 43242 | ALT_PRESENT | MUST_MIGRATE | P2 |
| 164 | layouts/partials/seo-jsonld.html | 11 | (site-wide: layouts/partials/seo-jsonld.html) | ORGANIZATION_JSONLD | https://alumcasting.com/wp-content/uploads/2026/03/KingShip-Logo.png | KingShip-Logo.png | png | WP_200 | NOT_FOUND_LOCAL | NOT_DUPLICATE | 200x40 | 2220 | ALT_MISSING | HUMAN_REVIEW | P1 |

## 18. Migration Classification (per unique asset)
- EXISTS_IN_HUGO = 0
- ALREADY_LOCAL = 0
- DUPLICATE = 0
- MUST_MIGRATE = 17
- CAN_RETAIN = 0
- BROKEN = 0
- HUMAN_REVIEW = 1

All 17 WebP assets = MUST_MIGRATE (no local equivalent, live on WP). The 1 PNG logo = HUMAN_REVIEW (org-identity; decide localize vs retain). No asset is broken or can be retained without a decision.

## 19. P0/P1/P2/P3 Risk
- P0 (missing hero/featured) = 0
- P1 (Article JSON-LD / Organization logo schema dependency) = 5
- P2 (body images) = 13
- P3 (duplicate/optional) = 0
- No P0: the Hugo site has no featured/hero image mechanism, so there is no single point-of-failure hero. P1 covers the 4 Article image URLs + the Organization logo. The bulk (13) are P2 body images.

## 20. Cutover Risk Assessment
- **MEDIA_MIGRATION_STATUS = BLOCKED**
- If `alumcasting.com` DNS is switched to GitHub Pages *today* with no media migration:
  - All 44 pages would show **broken images** (every `<img>`/schema image still points at `alumcasting.com/wp-content/...`).
  - Broken-image risk: **HIGH** for all 44 pages.
  - Mixed content: **NONE** (all https).
  - Staging leakage: **NONE**.
  - Schema-image leakage: **YES** — Article JSON-LD `image` (5 pages) and Organization `logo` (all pages) would 404/break unless WP media CDN stays up.
  - OG/Twitter image leakage: **NONE** (no og:image emitted).
- Assets already safe in Hugo: **0** (none local).
- Required to reach READY: download the 18 unique files into `static/`, rewrite the 164 source references (163 in `content/*.md` + 1 in `layouts/partials/seo-jsonld.html`) to local paths, then re-validate. This is an implementation step — NOT performed in this read-only phase.

## 21. Recommended Next Phase (out of scope, read-only)
1. Download the 18 unique assets (see CSV) into `static/uploads/<year>/<month>/` preserving WP paths.
2. Decide the Organization logo: localize (preferred) or explicitly retain.
3. Rewrite references: a one-time script replacing `https://alumcasting.com/wp-content/uploads/` → `/uploads/` (or a Hugo param) across `content/` and `seo-jsonld.html`.
4. Add alt text to the 3 body images currently missing alt.
5. Optionally add a local `og:image`/`twitter:image` for social cards.
6. Re-run this audit to confirm 0 WP media references remain and MEDIA_MIGRATION_STATUS = READY.

---

## FINAL OUTPUT
```
S.6-C.4-M STATUS = PASS
HEAD: cbc5101587de947cac2337377a8cfb49d938212e
origin/main: cbc5101587de947cac2337377a8cfb49d938212e
Working tree: clean (untracked report files only; 0 tracked modifications)

DISCOVERED_TOTAL: 164
UNIQUE_MEDIA_URLS: 18
DUPLICATE_REFERENCES: 146

WP_200: 18
WP_REDIRECT: 0
WP_404: 0
WP_403: 0
WP_5XX: 0
WP_AVAILABILITY_INCONCLUSIVE: 0

EXISTS_IN_HUGO: 0
ALREADY_LOCAL: 0
DUPLICATE: 0
MUST_MIGRATE: 17
CAN_RETAIN: 0
BROKEN: 0
HUMAN_REVIEW: 1

ARTICLE_JSONLD_WP_MEDIA: 5
FEATURED_IMAGE_WP_MEDIA: 0
OG_IMAGE_WP_MEDIA: 0
TWITTER_IMAGE_WP_MEDIA: 0

STAGING_MEDIA_REFERENCES: 0
EXTERNAL_MEDIA_REFERENCES: 0
MIXED_CONTENT_REFERENCES: 0

P0: 0
P1: 5
P2: 13
P3: 0

MEDIA_MIGRATION_STATUS: BLOCKED

REPORT: reports/S.6-C.4-M-wordpress-media-dependency-audit.md
CSV: reports/S.6-C.4-M-media-inventory.csv

SOURCE_CHANGES = 0
PRODUCTION_CHANGES = 0
REDIRECT_CHANGES = 0
DNS_CHANGES = 0
CLOUDFLARE_CHANGES = 0
WP_CHANGES = 0
```

**HARD STOP — no commit / push / deploy / media move performed.**