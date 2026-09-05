# S.6-C.4-F — Semi-Solid SSM Owner Consolidation

## 1. Executive Summary

**Status: `S.6-C.4-F = HOLD`**

Based on `S.6-C.4-E = PASS — B3 CONFIRMED`, this phase consolidates the two near-duplicate Semi-Solid Die Casting pages. The designated canonical owner is `/semi-solid-die-casting-heat-treatable-aluminum/` (final owner); the generic `/semi-solid-die-casting/` is retired.

**Hugo-side work is COMPLETE and VERIFIED (READY FOR REVIEW):**
- Generic SSM content file deleted → no longer generates an indexable duplicate page.
- 3 in-scope internal links remapped/removed (2 `related_services` → final owner; 1 self-ref removed).
- Local build `hugo --gc --minify` exits 0 (48 pages, was 49). Final owner verified: HTTP 200-equivalent, H1=1, self-canonical, `index,follow`, valid JSON-LD (Organization + WebPage + nested BreadcrumbList).
- Duplicate `<title>` eliminated on the Hugo/GitHub Pages surface.
- No unique factual content lost (generic had no facts absent from final owner).
- 7 deferred `/t6-heat-treatment-…/` dead links NOT touched.

**Production WordPress redirect changes are BLOCKED → HOLD per spec §13.**
The sandbox cannot read or write WP Redirection rules (REST `/rule` → 404, `/redirect` → 401). Therefore the two production redirect actions — (a) retarget 3169 to a clean 1-hop 301, and (b) remove the generic→heat-treatable alias — require authorized WP admin action. Exact admin steps are listed in §12. No WP/Cloudflare/DNS writes were performed.

---

## 2. Pre-Change State (read-only, re-confirmed this session)

Live HTTP (NoRedirect + browser UA, 2026-09-05):

| URL | Status | Location / Hops | Final |
|-----|--------|----------------|-------|
| `/semi-solid-die-casting/` (generic) | 301 | → `/semi-solid-die-casting-heat-treatable-aluminum/` | 200 |
| `/semi-solid-die-casting-heat-treatable-aluminum/` | 200 | — | 200 |
| `/semi-solid-die-casting-manufacturers/` (3169) | 301 | → `/semi-solid-die-casting/` → `/semi-solid-die-casting-heat-treatable-aluminum/` | 200 (2-HOP) |

Production chain unchanged from S.6-C.4-E. 3169 is a **2-hop chain**; generic still 301-aliases to heat-treatable.

Hugo sources: both pages shared the identical `<title>` "Semi-Solid Die Casting (SSM) | T6 Heat-Treatable Aluminum Parts". Generic page was the cleaner/richer body; final owner had pre-existing markdown defects (stray `**`, a duplicated `## Frequently Asked Questions` H2) — sufficient to serve as owner, not rewritten (spec §5 "DO NOT rewrite unnecessarily").

---

## 3. Final Owner Decision

**CANONICAL SSM OWNER = `/semi-solid-die-casting-heat-treatable-aluminum/`** (per spec §0, following S.6-C.4-E B3).

Final owner content sufficiency check (§5): covers SSM process, T6/heat-treatable aluminum, A356/A357, manufacturing/service intent, FAQ×3, SSM-vs-HPDC table, valid H1, self-canonical, valid schema. **No rewrite required** — kept as-is. Pre-existing markdown defects noted as FOLLOW-UP only (§19), not fixed.

---

## 4. Generic Page Handling

Generic `content/semi-solid-die-casting.md` **deleted** (90 lines). In the Hugo/GitHub Pages architecture the URL naturally stops generating → **404** (allowed end-state per §6: 404/410/natural non-generation; no new 301 created, no redirect target chosen). This removes the duplicate indexable SSM page on the Hugo surface.

On production WordPress, generic still 301→heat-treatable (UNCHANGED — admin-deferred, see §12).

---

## 5. Content Consolidation

Generic vs final-owner diff (S.6-C.4-E + this session): identical FAQ×3, identical SSM-vs-HPDC table, identical Target Industries list, identical T6/Zero-Porosity/Superior-Precision sections. Final owner additionally carries an X-ray microstructure image and a `{{< formspree >}}` CTA. **No unique factual content in generic** → no migration needed, **no content loss**.

---

## 6. 3169 Redirect Change

Per S.6-C.4-D, 3169 → `/semi-solid-die-casting/` was already written to production WP. This phase must retarget it to `/semi-solid-die-casting-heat-treatable-aluminum/` (single 1-hop 301) to eliminate the 2-hop chain.

**BLOCKED:** WP Redirection writes are not possible from the sandbox (REST 404/401). The existing 3169 rule is the only one and must be edited (not duplicated) by admin. → §12.

---

## 7. Internal Link Changes

Only links explicitly referencing the generic slug were touched (strict scope; the 9 "page deferred" 3169 links and 7 `/t6-heat-treatment…/` dead links are out of scope this phase):

| File | Change |
|------|--------|
| `content/cold-chamber-die-casting-services.md` | `related_services`: `/semi-solid-die-casting/` → `/semi-solid-die-casting-heat-treatable-aluminum/` |
| `content/sand-casting-services.md` | `related_services`: `/semi-solid-die-casting/` → `/semi-solid-die-casting-heat-treatable-aluminum/` |
| `content/semi-solid-die-casting-heat-treatable-aluminum.md` | `related_services`: removed dangling self-ref `/semi-solid-die-casting/` |

No other links modified; no unrelated fixes.

---

## 8. Redirect Validation

| Test | Expected | Result |
|------|----------|--------|
| 3169 → final owner (1 hop 301→200) | 301 → `/semi-solid-die-casting-heat-treatable-aluminum/` → 200 | **PENDING** — WP admin action required (sandbox blocked) |
| generic → NOT 301→heat-treatable | 404/410 on GitHub Pages | **PASS (Hugo)** — file deleted; still 301 on WP (admin) |
| final owner → 200 | 200 | **PASS** — build output verified |

---

## 9. Final Owner SEO Validation

- HTTP 200 (build artifact present): **PASS**
- H1 = 1: **PASS**
- robots = `index,follow`: **PASS**
- canonical = self (`…/semi-solid-die-casting-heat-treatable-aluminum/`): **PASS**
- JSON-LD valid: **PASS** (2 blocks — Organization + WebPage; WebPage contains nested `breadcrumb` BreadcrumbList with 2 items). No Article added, no FAQPage added, schema not redesigned.

---

## 10. Schema Validation

`layouts/partials/seo-jsonld.html` unchanged. Final owner emits WebPage + Organization reference + BreadcrumbList exactly as the global engine produces for `schema_type: WebPage`. Valid JSON (parsed successfully).

---

## 11. Dead-Link Deferral

The 7 references to `/t6-heat-treatment-semi-solid-die-casting-aluminum/` (404, no backing page) are **explicitly DEFERRED** to **S.6-C.4-G — Semi-Solid Link Hygiene**. Not created, not redirected, not modified, not deleted this phase.

---

## 12. Git Diff Scope

Working tree (uncommitted; `git status --short`):

```
 M content/cold-chamber-die-casting-services.md
 M content/sand-casting-services.md
 M content/semi-solid-die-casting-heat-treatable-aluminum.md
 D content/semi-solid-die-casting.md
```

`git diff --stat`: 4 files changed, 2 insertions(+), 93 deletions(-). `public/` is gitignored (regenerated output; stale `public/semi-solid-die-casting/` manually removed to reflect accurate local state). **No pre-existing user work modified or overwritten.** Repository head unchanged at `51e480a`.

---

## 13. Pre-existing Work Preservation

`git status --short` before edits was empty (clean @ `51e480a`). All modifications are strictly within the 4 SSM-consolidation files. No `git add -A`, `git reset`, `git checkout`, `git clean`, or `git stash` used. Pre-existing work preserved: **YES**.

---

## 14. Production Safety

WP Redirection REST probed read-only:
- `GET /wp-json/redirection/v1/rule` → **404**
- `GET /wp-json/redirection/v1/redirect` → **401**

Sandbox cannot read or write production redirect rules. No WAF bypass, no permission/cert/auth/DB-schema changes, no plugin security changes. Per spec §13, production redirect changes require authorized WP admin UI. **Admin actions (minimal, exact):**

1. WP admin → Redirection: edit existing rule `3169 /semi-solid-die-casting-manufacturers/ → /semi-solid-die-casting/` so target = `/semi-solid-die-casting-heat-treatable-aluminum/` (single 1-hop 301; do NOT create a second 3169 rule).
2. WP admin → Redirection: delete/disable rule `/semi-solid-die-casting/ → /semi-solid-die-casting-heat-treatable-aluminum/` so generic no longer 301-aliases.
3. Re-verify live: 3169 = 1 hop 301 → final owner 200; generic = NOT 301 → heat-treatable.

---

## 15. Final Status

```
S.6-C.4-F = HOLD

Final SSM owner            = /semi-solid-die-casting-heat-treatable-aluminum/   (Hugo: verified; WP: unchanged but designated)
Generic page handling      = DELETED on Hugo (404, no indexable duplicate); WP generic->heat-treatable alias PENDING admin
3169 redirect              = PENDING admin (sandbox cannot write WP Redirection) — target must become final owner, 1 hop
Redirect chain             = Hugo: none (generic gone); WP: 2-hop remains until admin retargets 3169
Duplicate title           = ELIMINATED on Hugo (generic deleted); WP PENDING admin
Content consolidation      = generic deleted, no unique facts lost
Canonical                  = final owner self-canonical (PASS)
Indexability               = final owner index,follow (PASS)
H1                         = 1 (PASS)
Schema                     = valid WebPage + Organization + BreadcrumbList (PASS)
Internal links             = 2 related_services remapped to final owner + 1 self-ref removed; 3169/t6 links untouched
7 dead links               = DEFERRED to S.6-C.4-G (not touched)
Git diff scope             = 4 in-scope content files only (clean)
Pre-existing work preserved = YES

Report: reports/s6-c4-f-semi-solid-owner-consolidation.md
JSON:   reports/s6-c4-f-semi-solid-owner-consolidation.json

Commit = NOT AUTHORIZED
Push   = NOT AUTHORIZED
Deploy = NOT AUTHORIZED

NEXT ACTION = STOP (await admin WP Redirection changes, then re-run §8 validation; do NOT enter S.6-C.4-G)
HARD STOP = YES
WRITES_PERFORMED = YES (Hugo source only); WP_REDIRECT_WRITES = NO
```

---

## Appendix — FOLLOW-UP (recorded, not fixed)

- Final owner page has pre-existing markdown defects (stray `**`, a duplicated `## Frequently Asked Questions` H2). Quality improvement only; not in S.6-C.4-F scope.
- 7 `/t6-heat-treatment-semi-solid-die-casting-aluminum/` dead links → S.6-C.4-G.
- 9 `"(page deferred)"` 3169 links resolve via the 3169→final-owner WP redirect once admin applies it (not modified this phase).
