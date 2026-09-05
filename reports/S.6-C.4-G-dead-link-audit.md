# S.6-C.4-G — Semi-Solid Dead-Link Audit

## Status

```
S.6-C.4-G = AUDIT_COMPLETE
```

Read-only discovery phase. **No files were modified.** No git, no production, no deployment actions taken. HARD STOP honored.

---

## 0. Scope & Method

- **In scope:** the single dead slug `/t6-heat-treatment-semi-solid-die-casting-aluminum/` and every reference to it in the current repo.
- **Out of scope (per spec §2):** all other 404s, redirects, canonical/title/H1/schema, other internal linking, SEO copy, images, Cloudflare, WordPress, DNS, GitHub Pages domain, sitemap, robots, GSC.
- **Search surface:** `content/`, `layouts/`, `data/`, `static/` (repo root `D:\Workbuddy\2026-08-31-19-30-01\alumcasting`).
- **Method:** case-insensitive literal grep for `t6-heat-treatment-semi-solid-die-casting-aluminum` (both `/slug/` and bare `/slug` forms), plus a Markdown/link-variant pass. All hits read in context.

---

## 1. Current Dead-Link State

The slug `/t6-heat-treatment-semi-solid-die-casting-aluminum/` has **no backing content file** and returns **404** on the live GitHub Pages surface. It is referenced by **exactly 7 files**, all inside `content/` as in-body Markdown links (not front matter, not `related_services`).

- `layouts/` → **0** hits
- `data/` → **0** hits
- `static/` → **0** hits

So the dead link is confined to 7 editorial body links across 7 pages. No template, data, or static reference exists.

---

## 2. All Reference Files & Contexts

| # | Source file | Line | Anchor text (verbatim) | Surrounding context |
|---|-------------|------|------------------------|---------------------|
| 1 | `content/gravity-die-casting-manufacturer.md` | 44 | `[T6 heat treatment]` | "Because GDC uses a slower filling rate, the level of entrapped air is significantly lower. This is critical because it allows us to perform [T6 heat treatment] without fear." *(annotated "page deferred — not in Batch 1")* |
| 2 | `content/custom-aluminum-die-casting-for-ev-powertrain-components.md` | 48 | `[T6 heat treatments]` | "A356 aluminum responds exceptionally well to comprehensive [T6 heat treatments], helping to avoid brittle fractures under sudden load stresses." |
| 3 | `content/cold-chamber-die-casting-services.md` | 57 | `[T6 heat treatments while entirely preventing blistering]` | "A356 combined with our specialized vacuum techniques enables flawless [T6 heat treatments while entirely preventing blistering]." |
| 4 | `content/medical-device-component-machining.md` | 41 | `[T6 heat treatment]` | "we often recommend [semi-solid casting](/semi-solid-die-casting-heat-treatable-aluminum/) followed by [T6 heat treatment]. This ensures that when the part hits our 5-axis CNC machines, the grain structure is stable…" |
| 5 | `content/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles.md` | 48 | `[t6 heat treatment semi solid die casting aluminum]` | "A356 alloy provides exceptional thermal transfer rates and elongation properties when paired with an optimized [t6 heat treatment semi solid die casting aluminum] process." |
| 6 | `content/manufacturing-capabilities.md` | 56 | `[T6 heat treatment processes]` | "the semi-solid metal flow produces lower porosity, improved mechanical performance, and compatibility with [T6 heat treatment processes]." *(annotated "page deferred — not in Batch 1")* |
| 7 | `content/sand-casting-services.md` | 48 | `[T6 heat treatment]` | "To reach peak performance, we almost always recommend a [T6 heat treatment] to stabilize the grain structure." |

---

## 3. Candidate Target Pages (existing-repo evidence only)

Per spec §5/§7, candidate targets must be derived from pages that **actually exist** in the repo.

- `content/semi-solid-die-casting-heat-treatable-aluminum.md` → **EXISTS**. Slug `/semi-solid-die-casting-heat-treatable-aluminum/`. Title: *"Semi-Solid Die Casting (SSM) | T6 Heat-Treatable Aluminum Parts"*. Contains dedicated `### T6 Heat Treatable` section, a T6 comparison table row ("T6 Solution Heat Treatment … Fully Compatible"), and an FAQ *"Why is semi-solid casting better for heat treatment than standard die casting?"*. This is the canonical SSM + T6 page established by S.6-C.4-F.
- `content/prevent-blistering-aluminum-t6-heat-treatment.md` → **DOES NOT EXIST (404)**. Referenced elsewhere (gravity:46, custom:48, liquid-cooled:48) but it is itself a dead link — out of scope here, NOT usable as a target.
- No standalone `t6-heat-treatment` page, no generic `aluminum-heat-treatment` page, no other heat-treatment-dedicated slug exists in `content/`.

**Conclusion:** the **only** existing page whose topic matches the dead slug ("T6 heat treatment of semi-solid / heat-treatable aluminum") is the final SSM owner `/semi-solid-die-casting-heat-treatable-aluminum/`.

---

## 4. Per-Reference Mapping Table (recommended actions)

| # | Source file | Source page/slug | Link context | Current target | Recommended target | Reason |
|---|-------------|------------------|--------------|----------------|-------------------|--------|
| 1 | `gravity-die-casting-manufacturer.md` | `/gravity-die-casting-manufacturer/` | "perform [T6 heat treatment] without fear" (low-porosity casting → T6) | `/t6-heat-treatment-semi-solid-die-casting-aluminum/` (404) | `/semi-solid-die-casting-heat-treatable-aluminum/` | Dead slug is explicitly SSM+T6; final owner is the canonical T6 Heat-Treatable Aluminum page. Anchor "T6 heat treatment" → page that owns T6 heat treatment of heat-treatable aluminum. Not the deleted generic `/semi-solid-die-casting/` (per §6). |
| 2 | `custom-aluminum-die-casting-for-ev-powertrain-components.md` | `/custom-aluminum-die-casting-for-ev-powertrain-components/` | "A356 … comprehensive [T6 heat treatments]" | 404 | `/semi-solid-die-casting-heat-treatable-aluminum/` | Final owner documents A356/A357 T6 heat treatment; exact topic match. |
| 3 | `cold-chamber-die-casting-services.md` | `/cold-chamber-die-casting-services/` | "A356 + vacuum … flawless [T6 heat treatments while entirely preventing blistering]" | 404 | `/semi-solid-die-casting-heat-treatable-aluminum/` | Final owner covers T6 + blistering prevention (its T6 section explains blister-free solution heat treatment). Strong match. |
| 4 | `medical-device-component-machining.md` | `/medical-device-component-machining/` | "recommend [semi-solid casting] → final owner, followed by [T6 heat treatment]" | 404 | `/semi-solid-die-casting-heat-treatable-aluminum/` | The very same sentence already links "semi-solid casting" to the final owner; "T6 heat treatment" is the same page's core T6 topic. Exact intent match. |
| 5 | `liquid-cooled-aluminum-cooling-plates-for-electric-vehicles.md` | `/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/` | "optimized [t6 heat treatment semi solid die casting aluminum] process" | 404 | `/semi-solid-die-casting-heat-treatable-aluminum/` | Anchor text mirrors the dead slug verbatim; maps to the page that owns SSM+T6. |
| 6 | `manufacturing-capabilities.md` | `/manufacturing-capabilities/` | "semi-solid metal flow … compatibility with [T6 heat treatment processes]" | 404 | `/semi-solid-die-casting-heat-treatable-aluminum/` | Paragraph is literally about SSM flow + T6; final owner is the SSM+T6 page. |
| 7 | `sand-casting-services.md` | `/sand-casting-services/` | "almost always recommend a [T6 heat treatment] to stabilize grain structure" | 404 | `/semi-solid-die-casting-heat-treatable-aluminum/` | Final owner covers T6 heat treatment to stabilize grain / maximize tensile strength. Good match. |

**Summary of recommendations:**
- **safe replacements = 7** (all remap to the final SSM owner `/semi-solid-die-casting-heat-treatable-aluminum/`).
- **remove-link recommendations = 0** (every link provides genuine navigational value to an existing T6/heat-treatable-aluminum page; removal would discard usefulness).
- **unresolved = 0** (every reference has a single, evidence-backed existing target).

---

## 5. SSM Consolidation Guard (spec §6)

- **None** of the 7 links is remapped to the deleted generic `/semi-solid-die-casting/` (that page was retired in S.6-C.4-F and returns 404 on GitHub Pages).
- All 7 are remapped to the **final SSM owner** `/semi-solid-die-casting-heat-treatable-aluminum/`, which is the correct, explicitly-permitted target per §6 ("if a link truly belongs to SSM intent … point to the final owner").
- No batch blind-replace risk: each of the 7 was judged individually (table above); all 7 happen to share the same correct target because the dead slug's topic is uniformly "SSM + T6 heat treatment" and the final owner is the sole existing page on that topic.

---

## 6. Cannibalization / Intent Check (spec §5.6 / §7)

- The final owner already treats T6 heat treatment as a core, dedicated topic (section + table row + FAQ), so pointing these 7 links there reinforces the correct canonical page rather than creating a competing one.
- No link is redirected to an unrelated page merely to clear a 404; all 7 anchors are about T6 heat treatment of (heat-treatable) aluminum, which is exactly the final owner's subject.
- No new page is created (spec §8). The dead slug is simply absorbed by the existing owner.

---

## 7. Related Observation — OUT OF SCOPE (not acted upon)

`/prevent-blistering-aluminum-t6-heat-treatment/` is referenced by 3 files (gravity:46, custom:48, liquid-cooled:48) and is **also a 404** (no backing content file). This is a *separate* dead link and is **explicitly outside S.6-C.4-G scope** (spec §2 forbids handling other 404s). It is recorded here only as a cross-reference for a future phase; **no change made**.

---

## 8. Read-Only Build Verification (spec §9)

```
$ hugo --gc --minify
Pages │ 48
BUILD_EXIT = 0
```

- `public/t6-heat-treatment-semi-solid-die-casting-aluminum/` → **ABSENT** (no page is generated for the dead slug).
- No files modified during this phase, so the build reflects the unchanged working tree.

---

## 9. Unmodified Files List (spec §11 / git safety)

No `git add / commit / push / reset / checkout / clean / stash` was performed. Full working-tree preservation confirmed:

- 4 S.6-C.4-F content changes remain staged-in-working-tree (uncommitted): `cold-chamber-die-casting-services.md`, `sand-casting-services.md`, `semi-solid-die-casting-heat-treatable-aluminum.md`, deleted `semi-solid-die-casting.md`.
- 2 S.6-C.4-F report files (untracked): `reports/s6-c4-f-semi-solid-owner-consolidation.md`, `reports/s6-c4-f-semi-solid-owner-consolidation.json`.
- This S.6-C.4-G report (untracked): `reports/S.6-C.4-G-dead-link-audit.md`.
- The 7 dead-link source files are **byte-identical** to pre-audit state (read-only; no edits applied).

---

## 10. Final Output

```
S.6-C.4-G = AUDIT_COMPLETE

7 dead-link references            = 7
safe replacements                 = 7   (all → /semi-solid-die-casting-heat-treatable-aluminum/)
remove-link recommendations      = 0
unresolved                        = 0
build                             = PASS (exit 0, 48 pages; dead-slug public dir ABSENT)
working tree preserved            = YES
commit                            = NO
push                              = NO
deploy                            = NO
```

### Next step (requires explicit write authorization)
When authorized, apply the 7 editorial remaps (change each `[anchor](/t6-heat-treatment-semi-solid-die-casting-aluminum/)` → `[anchor](/semi-solid-die-casting-heat-treatable-aluminum/)`), rebuild, and re-validate. Do **not** enter production verification or touch the out-of-scope `/prevent-blistering-aluminum-t6-heat-treatment/` link without a separate mandate.

**HARD STOP — WAIT FOR EXPLICIT WRITE AUTHORIZATION**
