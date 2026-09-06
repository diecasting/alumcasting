# S.6-C.4-P — Production URL / BaseURL Transition Preparation

**Mode:** Controlled Local Write + Read-Only Verification
**Date:** 2026-09-05 (GMT+8)
**Repository:** `D:/Workbuddy/2026-08-31-19-30-01/alumcasting` (branch `main`)

---

## 1. PHASE STATUS

```
PHASE_6_C.4_P = PASS
```

All 16 steps executed. No HARD STOP was triggered. Three authorized change
classes were applied to the local repository only; no production system was
touched.

---

## 2. BASELINE

```
P_BASELINE_HEAD              = cbc5101587de947cac2337a8cfb49d938212e
P_BASELINE_ORIGIN_MAIN       = cbc5101587de947cac2337a8cfb49d938212e  (git ls-remote origin HEAD)
P_INITIAL_STATUS             = 39 content + 1 layout (seo-jsonld.html) modified (N);
                               18 static/images/ untracked (N); N/O/M/L/I/J/K reports untracked
```

The working tree at start of P matched the authorized N/O state exactly. HEAD
and the remote `origin` HEAD both equal `cbc5101587de947cac2337a8cfb49d938212e`.
The local `origin/main` ref is not fetched locally but `git ls-remote` confirms
the remote baseline — consistent with prior phases (O, etc.).

---

## 3. STEP 1 — URL GENERATION AUDIT

| Item | Finding |
|------|---------|
| CURRENT_BASEURL | `https://diecasting.github.io/alumcasting/` (hugo.toml:5) |
| STAGING_DOMAIN_REFERENCE_COUNT (hardcoded source) | 0 |
| PRODUCTION_DOMAIN_REFERENCE_COUNT (hardcoded source) | 1 — Organization `@id` = `https://alumcasting.com/#organization` (seo-jsonld.html:7); by-design brand identity, NOT a staging artifact |
| PERMALINK_GENERATION | Central `{{ .Permalink }}` (head.html:11 canonical, :22 og:url) |
| CANONICAL_GENERATION | `{{ .Permalink }}` — derived from baseURL |
| OG_URL_GENERATION | `{{ .Permalink }}` — derived from baseURL |
| HARD_CODED_STAGING_URLS | 0 |
| INTERNAL_STAGING_PATHS (`/alumcasting/` in source) | 0 — the only `/alumcasting/` in source were the 164 N-localized media refs, all confirmed media-only |

**Conclusion:** BaseURL is the single source of truth; canonical/og/sitemap
derive centrally; no conflicting baseURLs; no hardcoded staging URLs. The only
`/alumcasting/` strings in source were the N media references, fully
distinguishable from internal links. **STEP 1 PASS — transition is safe.**

---

## 4. STEP 2 — AUTHORIZED PRODUCTION BASEURL CHANGE

```
OLD:  https://diecasting.github.io/alumcasting/
NEW:  https://alumcasting.com/
```

Applied to the single `baseURL` key in `hugo.toml` (line 5) only. No other
configuration, layout, content, or data file was modified. `relativeURLs=false`
remains, so all absolute URLs re-derive from the new baseURL automatically.

---

## 5. STEP 3 — MEDIA PREFIX TRANSITION

Scoped, surgical replacement — **NO global `/alumcasting/` replace was performed.**

```
/alumcasting/images/<file>   →   /images/<file>
```

Method: a controlled script operated only on the 164 N-localized media
references (regex `/alumcasting/images/` → `/images/`). Verified afterward:

- References rewritten: **164 / 164**
- Residual `/alumcasting/` in source after rewrite: **0**
- Files changed: **40** (39 content + `layouts/partials/seo-jsonld.html`)

The Organization `@id` `https://alumcasting.com/#organization` and its
`url` were untouched (they are not `/alumcasting/` paths). Media scope was
unambiguous → **STEP 3 PASS.**

---

## 6. STEP 4 — CNAME PREPARATION

`static/CNAME` did not exist prior to P. Created with exactly:

```
alumcasting.com
```

No `https://`, `/`, or `www.` prefix. This is GitHub Pages production-domain
preparation only; **DNS / Cloudflare were NOT modified** (explicitly out of
scope per HARD RULES).

---

## 7. STEP 5 — PRE-BUILD SOURCE SAFETY AUDIT

`git diff` restricted to exactly three authorized change classes:

1. **BaseURL transition** — `hugo.toml` (1 line).
2. **Exact localized media prefix transition** — 40 files, 161 media lines.
3. **CNAME preparation** — new file `static/CNAME`.

Programmatic verification (`diffsafe2.py`) against the committed (pre-N) HEAD:

- Media-only line changes: **161** (pure `wp-content/uploads/…` → `/images/…` substitutions; surrounding markdown/alt text byte-identical)
- Unrelated line changes: **1** (the authorized `hugo.toml` baseURL line)
- No H1 / title / description / front matter / body / schema / navigation / CSS / JS / image-binary changes.

**STEP 5 PASS — DIFF_SAFETY_STATUS = PASS.**

---

## 8. STEP 6 — HUGO BUILD

```
rm -rf public
hugo --gc --minify
EXIT CODE = 0
BUILD_STATUS = PASS
Pages            = 48   (unchanged from N/O baseline)
Static files     = 20   (19 + new CNAME)
public/images/   = 18 media files
public/CNAME     = present (16 bytes: "alumcasting.com\n")
public/robots.txt= present
```

---

## 9. STEP 7 — PRODUCTION CANONICAL / OG URL VERIFICATION

Generated HTML (44 HTML files):

```
CANONICAL_TOTAL        = 44
CANONICAL_PRODUCTION   = 44   (https://alumcasting.com/…)
CANONICAL_STAGING      = 0
CANONICAL_OTHER        = 0
canonical per page     = exactly 1 (self-canonical)

OG_URL_TOTAL           = 44
OG_URL_PRODUCTION      = 44
OG_URL_STAGING         = 0
OG_URL_OTHER           = 0
```

No `diecasting.github.io/alumcasting` canonical or og:url remains.

---

## 10. STEP 8 — MEDIA URL VERIFICATION

```
/alumcasting/images/ in source or public = 0
/images/  in source                       = 164   (post-P form)
/images/  references in generated HTML    = 207
/wp-content/ in generated HTML             = 0
diecasting.github.io in generated HTML    = 0

public/images/ files expected = 18
public/images/ files present   = 18
SHA-256 source vs public       = 18/18 MATCH (0 transformation)
```

Article JSON-LD image URLs and Organization logo URL: no WP media, no staging,
no mixed-content.

---

## 11. STEP 9 — ORGANIZATION SCHEMA VERIFICATION

```
@id  = https://alumcasting.com/#organization   (UNCHANGED — by design)
url  = https://alumcasting.com/                (UNCHANGED)
logo = /images/KingShip-Logo.webp              (local; WebP 200×40; verified in O)
```

The baseURL change did **not** alter the Organization `@id` (it was never
derived from baseURL). 44/44 generated Organization blocks valid.

---

## 12. STEP 10 — SITEMAP VERIFICATION

```
SITEMAP_URLS            = 44
SITEMAP_PRODUCTION_URLS = 44   (https://alumcasting.com/…)
SITEMAP_STAGING_URLS    = 0
```

---

## 13. STEP 11 — ROBOTS VERIFICATION

`public/robots.txt` content:

```
User-agent: *
Allow: /
```

- No staging-domain sitemap reference.
- No production sitemap line was added (original policy unchanged; Hugo does
  not auto-inject a Sitemap directive into a user-supplied static robots.txt).
- Out of P scope to alter robots policy (HARD RULE 5).

---

## 14. STEP 12 — INTERNAL LINK SAFETY

```
OLD_GITHUB_PROJECT_URLS (/alumcasting/foo/) = 0
OLD_FULL_STAGING_URLS (diecasting.github.io/alumcasting) = 0
BROKEN_INTERNAL_LINKS  = 105   (SEE NOTE BELOW)
```

**NOTE — 105 broken internal links are PRE-EXISTING, not introduced by P.**

Evidence:
- The slugs in question (e.g. `/semi-solid-die-casting-manufacturers/`,
  `/quality-control/`, `/magnesium-vs-aluminum-die-casting/`) exist in the
  committed HEAD source (`git show HEAD:content/…` confirms the links were
  present before N and P).
- P's `git diff` added **zero** non-media links (verified: no `href`/`/slug/`
  additions outside the media-prefix and baseURL changes).
- The links are clean root-relative `/slug/` paths — NOT `/alumcasting/`
  project paths and NOT staging references, so they do not trigger the STEP 12
  HARD STOP (which applies only to ambiguous/unclassified `/alumcasting/`
  references).

Root cause: these are related-article / cross-navigation links to WordPress
topics that fall outside the 48-page Hugo migration scope. They are a
content/redirect-scope matter (candidate for the earlier redirect-audit
pipeline, phases L/I/J/K), and are **explicitly out of P's write authority**
(HARD RULE 5 forbids content/link edits).

**P did not create or worsen any internal-link breakage. This is a documented
pre-existing migration-scope artifact, not a P defect.**

---

## 15. STEP 13 — FULL REGRESSION

```
JSON-LD parse errors          = 0
Organization schema valid     = 44/44
Article schema image valid    = 5/5
Breadcrumb (nested) valid     = present, 0 errors
Media: 18/18 assets, 0 WP, 0 staging, 0 missing
Full-project diecasting.github.io refs = 0
```

HTML status: PASS. JSON-LD status: PASS. Build status: PASS.

---

## 16. STEP 14 — DIFF SAFETY FINAL CHECK

Authorized change classes:

```
1. BaseURL transition          (hugo.toml)
2. Exact localized media prefix transition (/alumcasting/images/ → /images/)
3. CNAME preparation           (static/CNAME)
```

`git diff --name-only` contains only: 39 `content/*.md` + `layouts/partials/seo-jsonld.html` + `hugo.toml`.
No source modification outside these three classes. **STEP 14 PASS.**

---

## 17. STEP 15 — NO COMMIT / PUSH / DEPLOY

```
HEAD         = cbc5101587de947cac2337a8cfb49d938212e  (UNCHANGED)
ORIGIN_MAIN  = cbc5101587de947cac2337a8cfb49d938212e  (UNCHANGED, ls-remote)
COMMIT       = NO
PUSH         = NO
DEPLOY       = NO
```

Working tree retains the P-authorized uncommitted changes (permitted).

---

## 18. FINAL METRICS

```
PHASE_6_C.4_P STATUS          = PASS

BASELINE_HEAD                = cbc5101587de947cac2337a8cfb49d938212e
ORIGIN_MAIN                  = cbc5101587de947cac2337a8cfb49d938212e
WORKTREE                     = N/O changes + P auth changes (uncommitted)

OLD_BASEURL                  = https://diecasting.github.io/alumcasting/
NEW_BASEURL                  = https://alumcasting.com/

CNAME                        = alumcasting.com (created)

CANONICAL:  production=44  staging=0
OG:         production=44  staging=0
SITEMAP:    production=44  staging=0

MEDIA:      18/18 present, SHA match
            old prefix (/alumcasting/images/) = 0
            new prefix (/images/)              = 164 source / 207 HTML refs
            WP refs = 0
            broken media refs = 0

ORGANIZATION_ID     = https://alumcasting.com/#organization (unchanged)
ORGANIZATION_URL    = https://alumcasting.com/ (unchanged)
ORGANIZATION_LOGO   = /images/KingShip-Logo.webp (local, WebP 200×40)

JSON-LD = PASS   HTML = PASS   BUILD = PASS
OLD STAGING URL REFS = 0
OLD PROJECT PATH REFS = 0

UNAUTHORIZED_CHANGES = 0
SOURCE_FILES_CHANGED = 40 (39 content + seo-jsonld.html) + hugo.toml + CNAME(new)

COMMIT = NO  PUSH = NO  DEPLOY = NO
DNS = NO  CLOUDFLARE = NO  WORDPRESS = NO
```

---

## 19. RISKS / FOLLOW-UP (not P defects)

1. **Pre-existing broken internal links (105).** Links to non-migrated WP
   topics. Recommend a dedicated link-audit + redirect decision (extends the
   L/I/J/K redirect pipeline) before or alongside production cutover. Out of P
   scope to fix.
2. **CNAME is prepared but DNS/Cloudflare are NOT switched.** Activating
   `alumcasting.com` on GitHub Pages requires the domain's DNS to point at
   GitHub and the custom-domain verification to complete — a separate,
   explicitly out-of-scope operations step (HARD RULES prohibit DNS/Cloudflare
   writes).
3. **35 semantic remaps and 4 human-review URL decisions** from phases I/J/K/L
   remain unimplemented at the edge (Cloudflare/persistent layer). These are
   independent of the baseURL transition and belong to the cutover phase.

---

## 20. RECOMMENDATION

The local repository is now internally consistent for production hosting at
`https://alumcasting.com/`:
- All media is self-hosted under `/images/` (0 WP dependency, 0 staging).
- All canonical/og/sitemap URLs are production-domain.
- Organization identity unchanged.
- Diff scope strictly limited to the three authorized classes.

**S.6-C.4-P = PASS.** The transition is safe to carry into the cutover phase
once DNS/Cloudflare and the edge redirects are separately authorized and
applied. No commit/push/deploy was performed.
