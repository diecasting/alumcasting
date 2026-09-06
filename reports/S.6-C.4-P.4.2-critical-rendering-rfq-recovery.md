# S.6-C.4-P.4.2 — Critical Rendering & RFQ Form Recovery

**Date:** 2026-09-06
**Phase status:** `PHASE_6_C.4_P.4.2 = PASS`
**Objective:** Fix the two P0 regressions confirmed in P.4.1 — (1) site-wide HTML content was being re-escaped into literal text, and (2) the B2B RFQ Formspree forms were rendered as escaped text and therefore non-functional.

---

## 1. Baseline (STEP 0)

| Item | Value |
|------|-------|
| HEAD | `cbc5101587de947cac2337a8cfb49d938212e` |
| origin/main (via `git ls-remote`) | `cbc5101587de947cac2337a8cfb49d938212e` |
| Content `.md` files | 44 |
| Rendered HTML documents audited | 46 (52 Hugo "Pages" incl. sitemap/RSS/404/internal) |
| Static assets | 21 |

Working tree contains the pre-existing P.3 / P.4 baseline modifications (media migration, UX/UI system, `hugo.toml` baseURL). No HARD STOP triggered.

---

## 2. P0-1 Reproduction (STEP 1)

Pre-fix scan of all 46 generated HTML documents:

| Escaped token | Pages affected |
|---------------|----------------|
| `&lt;p` | 44 |
| `&lt;strong` | 41 |
| `&lt;form` | 30 |
| `&lt;label` | 30 |
| `&amp;amp;` (double-escape smoking gun) | 31 |

`ESCAPED_HTML_BEFORE` = **44** pages (any escaped body/ form tag). Visitors saw raw HTML source, not rendered content.

## 3. Root Cause (STEP 2)

Three layout templates assigned `.Content` to a variable, then ran `replaceRE` (which returns a plain string), then printed the string:

```go-html-template
{{ $c := .Content }}
{{ $c = replaceRE "<h1[^>]*>.*?</h1>" "" $c }}
{{ $c }}            ← printed as a string → Hugo re-escapes it
```

Files:
- `layouts/_default/single.html` (lines 13–15)
- `layouts/_default/list.html` (lines 13–15)
- `layouts/index.html` (lines 23–25)

`git show HEAD:single.html` confirmed the pre-P.4 template was simply `{{ .Content }}` (renders correctly). The P.4 rework introduced the string round-trip. The `replaceRE` H1-strip was intentionally preserving a unique-H1 rule (43 content files contain a body `# H1`), so the strip logic had to be **kept**, not deleted.

## 4. Rendering Fix (STEP 3)

Minimal, single-line safe fix in all 3 templates — keeps the H1 dedup, adds `safeHTML` so output is not re-escaped:

```go-html-template
{{ .Content | replaceRE "<h1[^>]*>.*?</h1>" "" | safeHTML }}
```

No change to: CSS, partials, hero/text, SEO metadata, canonical, Schema, URLs, internal links, or JS dependency (still zero JS). P.4 UX/UI system preserved intact.

## 5. P0-2 Reproduction (STEP 4)

The `{{< formspree >}}` shortcode embeds the form **inside** `.Content`. Because `.Content` was escaped, the form rendered as `&lt;form class="rfq-form" action="https://formspree.io/f/xpqgbdly" method="POST"&gt;` on all 30 pages that embed it.

| Metric | Value |
|--------|-------|
| FORMS_EXPECTED (pages embedding shortcode) | 30 |
| REAL_HTML_FORMS (before fix) | 0 |
| ESCAPED_FORMS (before fix) | 30 |

Authorized endpoint: `https://formspree.io/f/xpqgbdly`. No legacy WP/PHP endpoint present in source.

## 6. Formspree Repair (STEP 5)

Because the form lives inside `.Content`, the STEP 3 `safeHTML` fix simultaneously restored **real** `<form>` elements. No separate form rewrite was required (and none performed — per the "do not redesign the form" rule). The shortcode itself was already correct:

```html
<form class="rfq-form" action="https://formspree.io/f/xpqgbdly" method="POST">
  <input type="hidden" name="_subject" value="AlumCasting RFQ — New Inquiry">
  <label>Name *<input type="text" name="name" required></label>
  <label>Email *<input type="email" name="email" required></label>
  <label>Company<input type="text" name="company"></label>
  <label>Phone<input type="tel" name="phone"></label>
  <label>Project details *<textarea name="message" rows="6" required></textarea></label>
  <button type="submit">Send RFQ</button>
</form>
```

## 7. Endpoint Verification (STEP 6)

- `action` = `https://formspree.io/f/xpqgbdly` (only endpoint, authorized) ✓
- `method` = `POST` ✓
- Email field `type=email` ✓
- 3 `required` fields (name, email, message) ✓
- Submit button `type=submit` ✓
- No WP/PHP action, no staging endpoint, no old/secondary Formspree endpoint, no localhost, no wrong domain ✓

## 8. Whole-Site Form Audit (STEP 7)

| Metric | Value |
|--------|-------|
| TOTAL_EXPECTED_FORMS | 30 |
| REAL_HTML_FORMS | 30 |
| ESCAPED_FORMS | **0** |
| FORM_ACTIONS | `{'https://formspree.io/f/xpqgbdly'}` |
| FORM_METHODS | `{'POST'}` |
| WRONG_FORMSPREE_ENDPOINTS | **0** |
| LEGACY_WP_FORM_REFS | **0** |
| LEGACY_PHP_FORM_REFS | **0** |

Service / industry / homepage use a CTA → `/contact/` 2-step RFQ path (CTA link, no inline form) — by design; `/contact/` and the 30 shortcode pages carry the inline form.

## 9. HTML Regression (STEP 8)

Post-fix scan of all 46 documents:

| Escaped token | Pages affected (after) |
|---------------|------------------------|
| `&lt;p` / `&lt;strong` / `&lt;em` / `&lt;ul` / `&lt;ol` / `&lt;li` / `&lt;table` | 0 |
| `&lt;form` / `&lt;label` / `&lt;input` / `&lt;textarea` / `&lt;button` | 0 |
| `&amp;amp;` double-escape | 0 |

`ESCAPED_HTML_AFTER` = **0**. Real `<p>`, `<h2>`, `<ul>`, `<table>`, `<form>`, `<input>`, `<button>` tags all present and valid.

## 10. H1 / SEO Regression (STEP 9)

- **H1 per page = 1 on all 46 pages** (P.4 unique-H1 logic preserved). `H1_REGRESSION = 0`.
- `<title>` missing = 0; canonical all `alumcasting.com` = 0 bad.
- `name=description` / `name=robots` missing on 2 pages = `public/categories/index.html`, `public/tags/index.html` — these are **Hugo auto-generated taxonomy list pages**, pre-existing and untouched by P.4.2 (not part of the 44 content pages). No P.4.2-introduced regression.
- `SEO_REGRESSION = 0`.

## 11. Schema Regression (STEP 10)

- JSON-LD parse errors = **0**.
- `@type` distribution identical to P.4.1 baseline: Organization 46, WebPage 46, Article 6, FAQPage 3.
- Organization `@id` = `https://alumcasting.com/#organization` on all 46. No Schema data altered.
- `SCHEMA_REGRESSION = 0`.

## 12. URL / Media Regression (STEP 11)

- `diecasting.github.io` = 0 · `wp-content` = 0 · `/alumcasting/` = 0. `URL_REGRESSION = 0`.
- Local media: 207 `/images/` references, **0 missing on disk**; 5 JSON-LD image URLs, 0 missing. `MEDIA_REGRESSION = 0`. P.3 / P.4 media migration preserved.

## 13. Commercial Conversion Check (STEP 12)

Key pages verified:

| Page | Header RFQ CTA | Inline form | Endpoint OK | Submit | /contact/ link |
|------|----------------|-------------|-------------|--------|----------------|
| homepage | ✓ | (CTA→contact) | n/a | n/a | ✓ |
| aluminum-die-casting | ✓ | (CTA→contact) | n/a | n/a | ✓ |
| magnesium-die-casting-services | ✓ | (CTA→contact) | n/a | n/a | ✓ |
| automotive-die-casting-parts | ✓ | (CTA→contact) | n/a | n/a | ✓ |
| precision-die-casting-medical-equipment (P.3) | ✓ | ✓ | ✓ | ✓ | ✓ |
| hot-chamber-vs-cold-chamber-…-myth (P.3) | ✓ | ✓ | ✓ | ✓ | ✓ |
| contact | ✓ | ✓ | ✓ | ✓ | ✓ |

30 pages now expose a fully functional B2B RFQ form; the remainder funnel to `/contact/` via visible CTAs. The commercial funnel (Organic/Ads → Landing → Capability/Trust → RFQ) is now mechanically operational — the rendering/form blocker from P.4.1 is removed.

## 14. Diff Safety (STEP 14)

`git diff --name-only` → 46 files, classified:

- **A — P.4.2 authorized rendering fix:** `layouts/_default/single.html`, `layouts/_default/list.html`, `layouts/index.html` (only the `| safeHTML` change within the content block).
- **B — P.4.2 authorized RFQ fix:** 0 separate files — recovered as a direct consequence of A (form is inside `.Content`).
- **C — previous P-phase baseline:** the remaining 43 files (P.3 media migration + CRLF normalization, P.4 partials/baseof/head/seo-jsonld, `hugo.toml` baseURL). Not modified by P.4.2.
- **D — unauthorized:** **0**.

P.3 two new pages, P.4 UX/UI system, P media migration, production baseURL prep, and existing SEO/Schema fixes were **not** rolled back or overwritten.

## 15. Final Build (STEP 15)

`hugo --gc --minify` → exit 0 (129 ms). Static files = 21, rendered documents = 46 (52 Hugo page objects). No build warnings/errors attributable to P.4.2.

## 16. FINAL GATE (STEP 16)

| Gate | Result |
|------|--------|
| P0_RENDERING | **FIXED** |
| ESCAPED_HTML | **0** |
| RFQ_FORM_RENDERING | **FIXED** |
| REAL_HTML_FORMS | **30** |
| ESCAPED_FORMS | **0** |
| FORMSPREE_ENDPOINT | `https://formspree.io/f/xpqgbdly` |
| FORM_METHOD | `POST` |
| LEGACY_WP_FORM | **0** |
| LEGACY_PHP_FORM | **0** |
| H1_REGRESSION | **0** |
| SEO_REGRESSION | **0** |
| SCHEMA_REGRESSION | **0** |
| URL_REGRESSION | **0** |
| MEDIA_REGRESSION | **0** |
| BUILD | **PASS** |
| UNAUTHORIZED_CHANGES | **0** |

## 17. Report Artifacts

- `reports/S.6-C.4-P.4.2-critical-rendering-rfq-recovery.md` (this file)
- `reports/S.6-C.4-P.4.2-rfq-form-audit.csv` (per-page form audit, 46 rows)

---

## Residual notes (not P.4.2 scope, carried from P.4.1)

These survive P.4.2 (rendering fix does not change content/strategy) and remain **deferred to a later phase**:

- **P1:** Form lacks B2B qualification fields (material / process / annual volume / drawing-CAD-PDF upload). Recovery restored a working form; field enrichment is optimization, not recovery.
- **P1:** `2250T semi-solid` capability proof absent from all content; `21 years` appears on only 1 page.
- **P2:** No dedicated electronics / industrial die-casting landing pages (intent-map `MISSING_LP`).
- **P2:** 2 Hugo taxonomy pages (`categories/`, `tags/`) lack `description`/`robots` meta (auto-generated; pre-existing).

`DEFERRED_TO_LATER_PHASE` — no production, Cloudflare, DNS, redirect, or WordPress changes were made.
