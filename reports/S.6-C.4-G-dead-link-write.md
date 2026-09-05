# S.6-C.4-G — Dead-Link Minimal Write

**Phase status:** `S.6-C.4-G = PASS`
**Date:** 2026-09-05
**Mode:** Repository-only write (no WP / Cloudflare / DNS / deploy)

---

## 1. Pre-write git state

```text
 M content/cold-chamber-die-casting-services.md
 M content/sand-casting-services.md
 M content/semi-solid-die-casting-heat-treatable-aluminum.md
 D content/semi-solid-die-casting.md
?? reports/
```

(Pre-existing S.6-C.4-F working-tree modifications preserved; not reverted.)

Pre-write reference count for `/t6-heat-treatment-semi-solid-die-casting-aluminum/`: **7** (gate PASS → write proceeded).

## 2. Exact 7 source files

1. `content/cold-chamber-die-casting-services.md` (line 57)
2. `content/custom-aluminum-die-casting-for-ev-powertrain-components.md` (line 48)
3. `content/gravity-die-casting-manufacturer.md` (line 44)
4. `content/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles.md` (line 48)
5. `content/manufacturing-capabilities.md` (line 56)
6. `content/medical-device-component-machining.md` (line 41)
7. `content/sand-casting-services.md` (line 48)

## 3. Exact 7 replacements (anchor text preserved)

| # | File | Old (dead) link | New (owner) link |
| - | ---- | --------------- | ---------------- |
| 1 | cold-chamber-die-casting-services.md | `[T6 heat treatments while entirely preventing blistering](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` | `[T6 heat treatments while entirely preventing blistering](/semi-solid-die-casting-heat-treatable-aluminum/)` |
| 2 | custom-aluminum-die-casting-for-ev-powertrain-components.md | `[T6 heat treatments](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` | `[T6 heat treatments](/semi-solid-die-casting-heat-treatable-aluminum/)` |
| 3 | gravity-die-casting-manufacturer.md | `[T6 heat treatment](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` | `[T6 heat treatment](/semi-solid-die-casting-heat-treatable-aluminum/)` |
| 4 | liquid-cooled-aluminum-cooling-plates-for-electric-vehicles.md | `[t6 heat treatment semi solid die casting aluminum](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` | `[t6 heat treatment semi solid die casting aluminum](/semi-solid-die-casting-heat-treatable-aluminum/)` |
| 5 | manufacturing-capabilities.md | `[T6 heat treatment processes](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` | `[T6 heat treatment processes](/semi-solid-die-casting-heat-treatable-aluminum/)` |
| 6 | medical-device-component-machining.md | `[T6 heat treatment](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` | `[T6 heat treatment](/semi-solid-die-casting-heat-treatable-aluminum/)` |
| 7 | sand-casting-services.md | `[T6 heat treatment](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` | `[T6 heat treatment](/semi-solid-die-casting-heat-treatable-aluminum/)` |

Only the URL path inside the markdown link changed; surrounding prose, headings, titles, metadata, schema, images, CSS, layouts, navigation, and redirects are untouched.

## 4. Before count

`/t6-heat-treatment-semi-solid-die-casting-aluminum/` references in `content/` before write = **7**.

## 5. After old URL count

`/t6-heat-treatment-semi-solid-die-casting-aluminum/` references in `content/` after write = **0**.

## 6. After new URL count

The 7 previously-dead references now point to `/semi-solid-die-casting-heat-treatable-aluminum/` = **7**.
(Repo-wide new-URL references are higher because S.6-C.4-F `related_services` + navigation/schema legitimately reference the owner; generated `public/` contains 14 such slug matches — expected, not a failure.)

## 7. Diff scope

`git diff --name-only` (all 9 are `content/*.md`; no `layouts/` `data/` `static/` `config/` changed):

```text
content/cold-chamber-die-casting-services.md
content/custom-aluminum-die-casting-for-ev-powertrain-components.md
content/gravity-die-casting-manufacturer.md
content/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles.md
content/manufacturing-capabilities.md
content/medical-device-component-machining.md
content/sand-casting-services.md
content/semi-solid-die-casting-heat-treatable-aluminum.md   (pre-existing S.6-C.4-F)
content/semi-solid-die-casting.md                          (deleted, pre-existing S.6-C.4-F)
```

7 files newly modified in this phase; 2 carry pre-existing S.6-C.4-F changes. **Diff scope = PASS.**

## 8. `git diff --check` result

Exit code **0** (PASS). Only CRLF→LF normalization warnings emitted (pre-existing line-ending characteristic of these files); no trailing-whitespace or whitespace-error failures.

## 9. Hugo build result

`hugo --gc --minify` → **exit 0**, 48 pages. PASS.

## 10. Generated HTML old-target count

`grep -rl "t6-heat-treatment-semi-solid-die-casting-aluminum" public/` = **0**. PASS.

## 11. Generated HTML new-target count

`grep -rl "semi-solid-die-casting-heat-treatable-aluminum" public/` = **14** (≥7). PASS — not a failure despite >7 because the owner also appears in legitimate nav/schema/related_services.

## 12. Target page validation

File: `public/semi-solid-die-casting-heat-treatable-aluminum/index.html`

- HTTP/status equivalent: **200** (generated file present)
- H1 count: **1**
- canonical: `rel=canonical href=https://diecasting.github.io/alumcasting/semi-solid-die-casting-heat-treatable-aluminum/` → **self** ✓
- robots: `index,follow` ✓
- JSON-LD: **valid** (2/2 blocks parse as JSON; Organization + WebPage with nested BreadcrumbList)

## 13. Broken-link validation

All 7 modified pages' generated HTML link to `/semi-solid-die-casting-heat-treatable-aluminum/`; target page exists → **0 broken internal links caused by this change**. PASS.

## 14. SSM consolidation regression

- `/semi-solid-die-casting/` content page: **ABSENT** (still deleted from S.6-C.4-F) ✓
- `public/semi-solid-die-casting/`: **ABSENT** ✓
- No duplicate generic SSM page recreated ✓
- Final owner `/semi-solid-die-casting-heat-treatable-aluminum/` intact ✓

## 15. `/prevent-blistering-aluminum-t6-heat-treatment/` explicitly confirmed UNTOUCHED

Present in 3 files (`custom-aluminum…:48`, `gravity-die-casting-manufacturer.md:46`, `liquid-cooled…:48`). In `custom-aluminum:48` and `liquid-cooled:48` it co-occurs on the **same physical line** as the edited dead link. `git diff` of those lines shows the `/prevent-blistering-…/` URL text **byte-identical** in both `-` (old) and `+` (new) versions — only the dead-link path swapped. It was **not** modified, redirected, or removed.

## 16. WordPress / Cloudflare / DNS confirmed untouched

Repository-only phase. No WP writes, no Redirection changes, no Cloudflare changes, no DNS changes, no cache purge, no deployment trigger. Confirmed untouched.

## 17. Commit / push / deploy status

- Commit: **NOT performed** (`git add` / `git commit` not run)
- Push: **NOT performed**
- Deploy: **NOT performed**

---

## FINAL OUTPUT

```text
S.6-C.4-G = PASS

References before = 7
References changed = 7
Old dead-link references after = 0
New target references = 7

Files changed = 7 (this phase) + 2 pre-existing S.6-C.4-F in scope
Diff scope = PASS (content/*.md only)
git diff --check = PASS

Hugo build = PASS (exit 0, 48 pages)
Target page = PASS (200 / H1=1 / self-canonical / index,follow / JSON-LD valid)
Broken links = PASS (0 broken caused by change)
SSM consolidation regression = PASS
SEO regression = PASS
Schema regression = PASS

/prevent-blistering-aluminum-t6-heat-treatment/ = UNTOUCHED

WordPress = NOT TOUCHED
Cloudflare = NOT TOUCHED
DNS = NOT TOUCHED

Commit = NOT AUTHORIZED
Push = NOT AUTHORIZED
Deploy = NOT AUTHORIZED

Report: reports/S.6-C.4-G-dead-link-write.md

NEXT ACTION = STOP
```
