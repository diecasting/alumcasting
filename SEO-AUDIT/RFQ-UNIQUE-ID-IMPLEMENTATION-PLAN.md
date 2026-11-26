# RFQ Unique ID Implementation Plan

Date: 2026-11-25

Production SHA: `0bdb005af33ecb2f996bd3db0b1618943e825d35`

Historical status: `PARTIALLY VERIFIED / IMPLEMENTATION PLAN UPDATED / NO CHANGES AUTHORIZED`

Current status: `TWO RULES DELETED — SPECIFICATION COMPLETE — IMPLEMENTATION NOT AUTHORIZED`

## 1. Executive Summary

Recommendation: use one random, non-PII `rfq_id` as a correlation key across:

1. The website form submission.
2. Formspree receipt and notification email.
3. The GA4 `generate_lead` event, after confirmed form-submission success.
4. An approved internal RFQ register or CRM.

Recommended format:

```text
RFQ-YYYYMMDD-XXXXXXXXXXXX
```

The date is UTC. The suffix is 12 uppercase random hexadecimal characters from a
cryptographically strong random source. This provides 48 bits of randomness,
which is safer than the proposed 8-character suffix and remains short enough for
email and support use.

`rfq_id` is a pseudonymous technical identifier, not proof of identity and not a
GA4 user ID. It becomes personal-data-adjacent when joined to a Formspree receipt
or CRM record and must be access-controlled accordingly.

Implementation complexity: **Medium-High** because it spans the form, Formspree
notification behavior, Google tag lead-rule consolidation and reconciliation.

External administrator evidence was recorded on 2026-11-25 in
`SEO-AUDIT/RFQ-EXTERNAL-ADMIN-VERIFICATION-REPORT.md`. It confirms that the
client can pass `rfq_id`, but does not yet permit a “one success, one lead”
guarantee because the public Google tag contains two identical URL-based
`generate_lead` creation rules.

## 2. Existing Architecture Findings

### Website Form

Relevant files:

- `layouts/shortcodes/formspree.html`
- `layouts/shortcodes/formspree-ko.html`

Current form:

- POSTs to `https://formspree.io/f/xpqgbdly`.
- Uses a static hidden `_subject` field.
- Has no `rfq_id` field.
- Has no client-generated submission ID.
- Binds one submit listener through `f.dataset.a3bound`.
- Relies on `gtag` being available for the AJAX path.
- Falls back to native `f.submit()` on Formspree errors and network failures.
- Replaces the form only after a JSON response with `ok` or `success`.

The current `data-a3bound` guard prevents duplicate listener binding. It does not
prevent two rapid user submissions or duplicate Formspree records.

### Client `generate_lead`

Current call:

```text
gtag('event', 'generate_lead', {
  currency: 'USD',
  value: 10,
  lead_type: 'rfq',
  form_name: 'contact-form',
  form_id: 'rfq-form'
})
```

It runs only after a client-confirmed JSON success. It does not include:

- RFQ ID
- Submission timestamp
- Formspree receipt ID
- A deduplication guard

The current native fallback can submit without emitting this client event.

### Public Google Tag Configuration

The public `GT-M38DWB5D` Google tag configuration contains:

- GA4 destination `G-B48RJMJ2W6`.
- Enhanced form measurement producing `form_start`/`form_submit`.
- Conversion marking for `generate_lead`.
- Two identical event-creation rules that generate `generate_lead` from
  `page_view` when `page_location` contains `thank-you`.
- Automatic email, phone and name/address detection.
- Google Signals enabled.
- No existing `rfq_id` parameter.

The two container-created rules cannot reliably carry the proposed random
`rfq_id` because they are URL/page-view based. They must be reviewed and retired
or consolidated before a one-ID-per-lead architecture can be guaranteed.

The available general GTM account exposed `GTM-NH3766HW`, not the Production
Google tag. The GA4 Admin Google tag settings and the public `GT-M38DWB5D`
resource were inspected separately. The Google tag Versions view reports an
empty live version while public settings contain generated lead rules; this
source-of-configuration discrepancy remains unresolved and requires analytics
owner confirmation.

### Formspree

- The live form stores custom fields; `annual_volume` is directly visible in
  existing submissions.
- The current form uses a hidden `_subject` field.
- Formspree documents duplicate submissions when users click repeatedly.
- A custom field is useful for identification and reconciliation, but it is not
  an automatic deduplication mechanism.
- The official email-subject mechanism uses a field named `subject` and supports
  variables such as `{{ rfq_id }}`.
- The current `_subject` field is observed and stored as a custom field, but the
  available evidence does not prove that it controls the notification Subject
  header. Do not remove it until the documented `subject` replacement is tested.
- Custom email templates support `{{ rfq_id }}` but the current Settings UI
  requires a Business upgrade.
- `rfq_id` is expected to be accepted as a custom field. A real receipt, default
  notification-email rendering and a synthetic end-to-end test remain required.

Official references:

- [Formspree email subject line](https://help.formspree.io/articles/building-your-form/email-subject-line/)
- [Formspree duplicate submissions](https://help.formspree.io/articles/troubleshooting/delay-on-submit-or-double-submissions)
- [Formspree custom email templates](https://help.formspree.io/fr/articles/advanced-features/setting-custom-email-templates)

## 3. Proposed RFQ ID Architecture

### ID Generation

- Generate in the browser when the form initializes, before the first submit.
- Use `crypto.randomUUID()` when available.
- Otherwise use `crypto.getRandomValues(new Uint8Array(6))`.
- Encode six random bytes as 12 uppercase hexadecimal characters.
- Use the UTC date to produce `RFQ-YYYYMMDD-XXXXXXXXXXXX`.
- Never derive the ID from email, phone, company, user ID, IP address, device ID,
  timestamp alone or another personal value.
- Do not place the ID in the page URL or query string.

### Hidden Formspree Field

Proposed HTML:

```html
<input id="rfq-id" type="hidden" name="rfq_id" value="">
```

The browser populates it before the submit listener processes the form. The same
value is therefore included in:

- AJAX FormData.
- A JS-enabled native form fallback.
- The Formspree receipt.

An entirely no-JavaScript submission cannot generate a browser-random ID with the
current static-site architecture. The form should continue to work, but the ID
will be blank. A guaranteed no-JS ID would require an approved server or edge
submission endpoint and is out of scope.

### Email Notification

Minimum:

- Keep hidden `name="rfq_id"`.
- Have the Formspree administrator verify that the default notification email
  includes the custom `rfq_id` field.

Preferred email subject:

```html
<input
  type="hidden"
  name="subject"
  value="AlumCasting RFQ {{ rfq_id }} — New Inquiry">
```

This follows Formspree’s documented subject-variable syntax. The current
`_subject` field must be tested before migration because it uses a different
field name.

If the current notification template does not clearly display the ID:

- Use the project email template with `{{ rfq_id }}` in the body or subject.
- Confirm plan/project eligibility with the Formspree administrator.
- Do not purchase or change plan under this assessment.

## 4. Proposed Files

Likely implementation files:

1. `layouts/shortcodes/formspree.html`
2. `layouts/shortcodes/formspree-ko.html` only if the Korean form remains an
   active convert route and its tracking is separately approved.

Likely not required:

- `layouts/partials/analytics.html`: GT/GA4 loader remains unchanged.
- `hugo.toml`: no new ID setting is needed.

External configurations likely involved:

- Formspree notification/email template.
- GT/GTM removal or consolidation of the two URL-based `generate_lead` rules.
- GA4/BigQuery validation access.

No Ads/G-CONV source change is required because Ads conversion is out of scope.

## 5. Submission Lifecycle

### First Submission

1. Form loads and receives a valid `rfq_id`.
2. User submits.
3. Submit handler immediately:
   - Captures the ID.
   - Sets an in-flight state.
   - Disables the submit button.
   - Stores the ID as the pending logical attempt.
4. FormData contains the same hidden `rfq_id`.
5. Formspree receives and records it.

### AJAX Success

1. A successful Formspree JSON response is received.
2. The browser checks that this `rfq_id` has not already emitted a lead.
3. Emit one `generate_lead` event with `rfq_id`.
4. Show the success message.
5. Clear the pending state.

### Known Failure

- Keep the same `rfq_id`.
- Re-enable an explicit retry action only after the request is known to have
  failed.
- A retry of the same logical submission reuses the ID.

### Unknown Network Outcome

This is the critical limitation:

- A transport failure does not prove Formspree rejected the request.
- Automatically calling `f.submit()` after an unknown outcome can create a
  duplicate Formspree record.
- Recommended behavior: do not auto-submit after an ambiguous network failure.
  Show a clear status and require an explicit retry that reuses the same ID, or
  direct the user to the existing email route.
- The ID lets support identify possible duplicates; it does not make Formspree
  deduplicate them.

### JS-Enabled Native Fallback

- If `gtag` is unavailable before AJAX begins, the browser can use native POST.
- The hidden ID is already populated and travels with the form.
- Never emit a client `generate_lead` on the fallback path.
- A later GA4 receipt cannot be claimed unless an independent receipt mechanism
  confirms success.

## 6. Duplicate Protection

Required controls:

1. A synchronous in-flight flag.
2. Disable the submit button on submit.
3. Ignore additional submit events while in flight.
4. Reuse the same ID for explicit retry of the same logical attempt.
5. Maintain a per-tab/session set of IDs for which `generate_lead` was emitted.
6. Never emit a second GA4 lead for an ID already in the emitted set.
7. On success, clear the pending attempt.
8. On a new form session or confirmed completion, generate a new ID.

Formspree does not automatically deduplicate based on a custom `rfq_id`.
Therefore:

- Duplicate prevention must happen before a second request.
- Duplicate detection must reconcile IDs in Formspree/CRM.
- A duplicate record with the same ID is evidence of an ambiguous/retried
  submission and must be reviewed.

### sessionStorage

Optional:

- Store the pending ID and form/page key in `sessionStorage`.
- Use it only for an explicit same-attempt retry.
- Do not silently reuse a pending ID for a new unrelated inquiry.
- Clear it after confirmed success.
- Fall back to in-memory/DOM storage if sessionStorage is unavailable.

## 7. GA4 Event Design

Proposed event:

```text
gtag('event', 'generate_lead', {
  rfq_id: 'RFQ-YYYYMMDD-XXXXXXXXXXXX',
  lead_type: 'rfq',
  form_name: 'contact-form',
  form_id: 'rfq-form'
})
```

Rules:

- Fire only after a confirmed form-submission success.
- Do not include name, email, phone, company, message or drawing reference.
- Keep `rfq_id` out of page URLs and referrer values.
- For a successful single event, GA4 will contain one event with that parameter.
- GA4 does not independently deduplicate generic events based on an event
  parameter. Duplicate suppression must be implemented in the website.

### GTM Requirement

The two page-view/thank-you `generate_lead` rules create public tag IDs 18 and
19 and conflict with one-ID-per-event guarantees. Before implementation:

- Identify both rules and their triggers.
- Confirm whether the Google tag Versions/workspace and public generated settings
  represent the intended live configuration.
- Retire or consolidate them with GTM-owner approval.
- Keep the client successful-submission event as the controlled lead signal, or
  document a different single source of truth.

No GTM change is authorized now.

## 8. Custom Dimension and BigQuery

Do not register `rfq_id` as a GA4 custom dimension without a specific reporting
need.

Google defines high-cardinality dimensions as more than 500 unique values per
day. A high-cardinality RFQ ID can increase report rows and cause values to be
condensed into `(other)`. It is an identifier, not a useful aggregate dimension.

- Use DebugView for an immediate parameter check.
- Use a redacted request capture to confirm the transmitted value.
- Use BigQuery Export for durable event-parameter queries and joins to
  Formspree/CRM.
- BigQuery is not required merely to display one test event, but it is recommended
  for long-term detailed reconciliation.

Official references:

- [GA4 cardinality](https://support.google.com/analytics/answer/12226705?hl=en)
- [GA4 BigQuery Export schema](https://support.google.com/analytics/answer/7029846?hl=en)

BigQuery Export may require property/project access and billing or quota review.
No cost estimate is made.

## 9. Formspree Reconciliation

Minimum fields for an internal RFQ export/register:

- `rfq_id`
- Formspree receipt ID
- Formspree received timestamp
- Website form page
- GA4 event timestamp, if captured
- Submission status
- Spam/duplicate status
- Qualification status
- Owner

Join strategy:

- Primary join key: random `rfq_id`.
- Secondary check: timestamp proximity and receipt ID.
- Never join using email or phone in GA4.

## 10. Privacy Considerations

- `rfq_id` must be random and non-derived.
- It is pseudonymous, not anonymous, once it links to a receipt or CRM row.
- Store it with the same access controls and retention rules as the RFQ record.
- Do not expose it in public URLs, logs, screenshots or third-party analytics
  reports.
- Do not use it as a GA4 `user_id` or cross-site identity.
- Do not include other form values in GA4.
- Do not assume hashing a personal value creates a safe identifier.
- C1/C2 remain frozen; any event/identifier processing requires privacy review.

## 11. Synthetic Test Plan

Do not execute until explicitly approved.

### Test Identity

- Name: `TRACKING TEST`
- Company: `TRACKING TEST — DO NOT CONTACT`
- Email: a dedicated approved test inbox
- Phone: blank or reserved internal test value
- Message: `Synthetic RFQ ID verification`
- Drawing reference: blank
- No customer information or drawing

### Positive Test

1. Load the Contact page and record the generated `rfq_id`.
2. Confirm format and random generation.
3. Submit once in a controlled test environment.
4. Confirm the network FormData contains the same ID.
5. Confirm the Formspree receipt contains the same ID.
6. Confirm the notification email contains the same ID.
7. Confirm one client `generate_lead` contains the same ID.
8. Confirm GA4 DebugView/Realtime event receipt and parameter, subject to display
   limitations.
9. If BigQuery is connected, confirm one event row contains the same ID.
10. Reconcile the ID with the Formspree receipt.

### Negative and Retry Tests

1. Failed HTTP/validation response: no success lead event.
2. Network failure: no success event without confirmed receipt.
3. Rapid double click: one in-flight request and one lead event.
4. Same-attempt retry: same ID and no duplicate lead event.
5. New inquiry: new ID.
6. JS unavailable: form may submit without generated ID; no GA4 success claim.
7. Native fallback with JS available: ID is submitted; no client success event.
8. Two container lead paths: confirm they are disabled or cannot double-fire.
9. Check event payloads contain no PII.

### Acceptance Criteria

- ID format is valid and non-PII.
- One logical attempt has one ID.
- Formspree receipt and email show the same ID.
- GA4 lead event uses the same ID only after confirmed success.
- Failed/unknown outcomes never emit success.
- Duplicate clicks and retry are identifiable.
- Standard GA4 reporting does not require `rfq_id` as a high-cardinality custom
  dimension.
- No Ads conversion implementation is introduced.

## 12. Required Approvals

| Approval | Owner |
|---|---|
| Source patch for form hidden field and ID lifecycle | SEO/content, engineering, release owner |
| Replace `_subject` with documented dynamic `subject` syntax | Formspree administrator, business owner |
| Retire/consolidate public lead rules 18 and 19 | Google tag/analytics administrator |
| Clarify empty live version versus public generated settings | Google tag/analytics administrator |
| Confirm default notification shows `rfq_id` | Formspree administrator or approved test |
| Decide whether to rename the shared `Kingship Web RFQ` form | Business/Formspree owner |
| GA4 DebugView/BigQuery verification | Analytics owner |
| Synthetic test submission | Website/business owner |
| Privacy review of pseudonymous ID and retention | Privacy/legal |
| Deployment | Separate release approval |

No GTM, GA4, Formspree or source change is authorized by this plan.

2026-11-26 amendment: the earlier request to clarify the Google tag “empty live
version” is no longer a blocker for this source patch. The GA4 property-level
custom events and their generated public tag configuration are directly
observable; see Section 14.

## 13. Workstream Complexity

| Workstream | Complexity |
|---|---|
| Random ID generation and hidden field | Low-Medium |
| Retry/double-click behavior | Medium |
| Email notification confirmation | Medium; depends on Formspree plan |
| Remove duplicate container lead rules | Medium |
| GA4 DebugView/BigQuery verification | Medium-High; account access required |
| Formspree/CRM reconciliation | High |

## Final Status

PARTIALLY VERIFIED — SPECIFIC ADMIN ACCESS REQUIRED — NO SOURCE OR ACCOUNT CHANGES — DEPLOYMENT NOT AUTHORIZED

## 14. 2026-11-26 Final Readiness Amendment

This dated amendment supersedes the readiness decision above while preserving
the earlier assessment as historical evidence.

### A. Final Event-Rule Finding

GA4 custom configurations for destination `G-B48RJMJ2W6` contain two separate
custom events, both named `generate_lead`:

```text
event_name equals page_view
AND
page_location contains thank-you
```

Both have `Copy parameters from the source event` disabled. The public Google
tag represents them as independent event-creation components, public tag IDs 19
and 18, with priorities 6 and 5.

Conclusion:

- They are two independent definitions with identical conditions.
- They are functionally duplicate and potentially overlapping.
- No condition makes them mutually exclusive.
- Actual duplicate GA4 receipt is `NOT VERIFIED` because no matching page view
  or RFQ test was executed.
- `generate_lead` is also marked as a GA4 key event; this is separate from event
  creation.

Minimum safe correction:

1. Disable both URL-based custom events.
2. Keep the `generate_lead` key-event designation.
3. Use the website’s successful Formspree JSON callback as the single lead
   signal.
4. Add `rfq_id` only to that callback after success.

### B. Independent Client Callback

The English form still contains one client call:

```text
gtag('event', 'generate_lead', {
  currency: 'USD',
  value: 10,
  lead_type: 'rfq',
  form_name: 'contact-form',
  form_id: 'rfq-form'
})
```

It executes after `res.ok || res.success`. The current success transition replaces
the form in place and does not navigate to a `thank-you` URL.

Configuration-based inference:

- A normal successful AJAX submission is expected to emit one client
  `generate_lead`.
- The two URL rules should not match merely because the rendered success copy
  contains “Thank you.”
- Runtime one-to-one correlation remains `NOT VERIFIED`.

The current code has no in-flight flag or disabled submit button. Duplicate
requests and duplicate client events therefore remain possible.

### C. Final ID Generation Contract

Generate the ID from exactly six cryptographically secure random bytes:

```text
bytes = crypto.getRandomValues(new Uint8Array(6))
suffix = 12 uppercase hexadecimal characters
date = UTC date formatted YYYYMMDD
rfq_id = RFQ-YYYYMMDD-XXXXXXXXXXXX
```

Do not use `Math.random`. Do not derive the value from email, phone, company,
project details, IP address, browser fingerprint or timestamp alone.

The ID is created before the first submit and stored in:

- The hidden `rfq_id` field.
- The pending same-attempt browser state.
- A per-tab emitted-ID set used to suppress a second GA4 lead event.

An explicit retry of the same logical attempt reuses the ID. A genuinely new RFQ
generates a new ID. A no-JavaScript submission continues to work but has no
browser-generated ID.

### D. Final Data Contract

| Layer | Field / event | Rule |
|---|---|---|
| Website | `rfq_id` hidden field | Exact generated value |
| Formspree receipt | `rfq_id` | Same exact value; no PII |
| Email subject | `subject` containing `{{ rfq_id }}` | Documented; requires approval and test |
| Email body | Visible `rfq_id` | Only after current-account behavior is verified |
| GA4 | `generate_lead.rfq_id` | Emit only after Formspree JSON success |
| GA4 dimension | `rfq_id` | Do not register by default |
| Future CRM | `rfq_id` | Same value; future integration only |

Email status is deliberately split:

- Platform support for arbitrary field variables: `VERIFIED DOCUMENTATION`.
- Current Free plan: `VERIFIED`.
- Default email automatically shows a future `rfq_id`: `NOT VERIFIED`.
- Existing `_subject` controls the Subject header: `NOT VERIFIED`.
- Documented `subject` supports `{{ rfq_id }}`: `VERIFIED DOCUMENTATION`.

### E. Lifecycle Contract

| Object | Definition | Does not mean |
|---|---|---|
| Submission attempt | Browser begins one logical submit with an ID | Formspree received it |
| Formspree accepted submission | Formspree returns successful JSON and records the request | The lead is qualified |
| GA4 lead event | One successful client callback sends `generate_lead` with the ID | Human sales qualification |
| Qualified RFQ | A person reviews and accepts the enquiry as valid, relevant and unique | A browser-side state |

`rfq_id` does not provide server-side deduplication.

### F. Final Dependency Register

| Dependency | Status | Resolver |
|---|---|---|
| Disable both URL-based custom events | REQUIRED BEFORE RELEASE | Analytics administrator for GA4 property `527174041` / Google tag `GT-M38DWB5D` |
| Approve `_subject` to `subject` migration | REQUIRED | Formspree owner for form `xpqgbdly` + release owner |
| Verify default email displays ID | NOT VERIFIED | Formspree owner or approved test |
| Approve synthetic submission and GA4 check | REQUIRED | Business website owner + analytics administrator |
| Approve ID retention/access | REQUIRED | Privacy/legal reviewer |
| Authorize implementation and release | REQUIRED | Website release owner |

CRM and Google Ads are not dependencies for this source patch.

### G. Final Decision

`READY AFTER EXTERNAL CONFIGURATION CORRECTION`

The source patch may be prepared only after source approval. Production
activation and one-success/one-lead claims remain blocked until the duplicate
custom events and email behavior are resolved and the approved correlation test
passes.

## 15. 2026-11-26 External Correction Approval Package

This dated amendment is approval preparation only. It does not authorize GA4,
Google tag, Formspree or source changes.

Status:

`EXTERNAL CORRECTION REQUIRED — IMPLEMENTATION NOT AUTHORIZED`

## 16. 2026-11-26 Authorized Correction Attempt

This amendment records the result of the authorized attempt. Earlier plan
content remains historical.

Final execution status:

`BLOCKED — NO CONFIGURATION CHANGES MADE`

Pre-change identity:

| Item | Value |
|---|---|
| Account label | `SKSME Alumcasting` |
| Property ID | `527174041` |
| Stream | `Alumcasting` |
| Measurement ID | `G-B48RJMJ2W6` |
| Google tag | `GT-M38DWB5D` |

Two matching custom events were present:

| Row | Event | Conditions | Public component |
|---:|---|---|---|
| 1 | `generate_lead` | `event_name equals page_view`; `page_location contains thank-you` | Tag `19`, priority `6` |
| 2 | `generate_lead` | `event_name equals page_view`; `page_location contains thank-you` | Tag `18`, priority `5` |

Rule A was opened and matched the requested conditions. The detail screen exposed
no reversible pause or disable control. The only mutation choices were:

- Copy.
- Delete event configuration.

The approved instruction prohibited deletion and required a stop when only
deletion was available. No rule was modified or deleted.

Source validation:

- The AJAX success callback still calls `generate_lead`.
- It remains independent of URL-based rules.
- No source file was changed.

Execution timestamp:

`2026-11-26T08:12:01+08:00`.

No test RFQ, email, GA4 runtime test or configuration mutation occurred.

A separate deletion authorization and rollback plan are required before a future
attempt.

### A. Exact GA4 Rules

Property: `527174041`.

Stream: `G-B48RJMJ2W6`.

Both custom events:

```text
Custom event name: generate_lead
Condition 1: event_name equals page_view
Condition 2: page_location contains thank-you
Copy parameters from source event: OFF
```

| Display order | Public tag component | Priority |
|---:|---:|---:|
| 1 | `19` | `6` |
| 2 | `18` | `5` |

The administrator must verify the two visible rows immediately before any
action. If the UI offers no reversible disable/archive option and only deletion
is available, implementation must stop for separate destructive-change
approval.

### B. Change Isolation

Disabling these two URL-based rules does not edit the site callback:

- English: `layouts/shortcodes/formspree.html`.
- Existing Korean route: `layouts/shortcodes/formspree-ko.html`.

Both call `gtag('event', 'generate_lead', ...)` directly after Formspree returns
`ok` or `success`. The callback does not depend on either URL rule.

Required preservation:

- GA4 destination `G-B48RJMJ2W6`.
- `generate_lead` key-event marking.
- Automatic-event settings.
- Google Signals.
- User-provided data settings.
- Consent-related settings.
- Formspree.
- Website source.

### C. Desired Surviving Source

After source implementation, exactly one client-side event should survive:

```text
generate_lead
  rfq_id: RFQ-YYYYMMDD-XXXXXXXXXXXX
  lead_type: rfq
  form_name: contact-form
  form_id: rfq-form
```

It must execute only after a successful Formspree JSON response. No URL-based
custom event should create a lead from navigation alone.

### D. Administrator Request

The exact request is maintained in
`SEO-AUDIT/RFQ-EXTERNAL-ADMIN-VERIFICATION-REPORT.md`, section
“Administrator Change Request”.

Business approval must state:

- the GA4 property and stream;
- both exact conditions;
- whether the available mechanism is disable or deletion;
- that no Google tag or other GA4 setting may change;
- the evidence required before and after.

### E. Post-Change Gate

The correction is complete only when:

1. Both matching URL rules are disabled or removed.
2. `generate_lead` remains a key event.
3. All other Google tag settings remain unchanged.
4. The public tag no longer generates `generate_lead` from a matching page view.
5. An approved test proves a controlled `thank-you` URL does not create a lead.
6. An approved successful submission produces one client `generate_lead` with
   the same `rfq_id`.
7. A failed submission and duplicate click produce no false lead.

Until all checks pass, source implementation and release remain unauthorized.

### F. Formspree Subject Approval — Separate Track

Proposed future replacement:

```html
<input
  type="hidden"
  name="subject"
  value="AlumCasting RFQ {{ rfq_id }} — New Inquiry">
```

This requires:

- Formspree owner approval.
- Business release owner approval.
- An approved receipt/email test.
- No plan upgrade.

The Free plan does not provide custom email templates. The default notification
body’s future `rfq_id` behavior is `NOT VERIFIED`. Website source must not rely
on that behavior yet.

### G. Final Authorization State

| Decision | Status |
|---|---|
| Exact correction documented | YES |
| External rule change approved | NO |
| External rule change implemented | NO |
| Formspree subject migration approved | NO |
| Source implementation approved | NO |
| Deployment approved | NO |

`EXTERNAL CORRECTION REQUIRED — IMPLEMENTATION NOT AUTHORIZED`

## 17. 2026-11-26 Post-Deletion Specification Closure

External correction is complete.

Configuration status:

`TWO RULES DELETED — CONFIGURATION AND RECOVERY EVIDENCE VERIFIED`

Source-specification status:

`READY FOR SOURCE IMPLEMENTATION — IMPLEMENTATION NOT AUTHORIZED`

Runtime status:

`NOT VERIFIED — SEPARATELY AUTHORIZED TEST RFQ REQUIRED`

### Confirmed Event Topology

The two GA4 URL-based `generate_lead` custom events were deleted:

- `event_name equals page_view`
- `page_location contains thank-you`
- Destination `G-B48RJMJ2W6`
- Copy source parameters off

The GA4 custom-event list is empty. The `generate_lead` key-event designation
remains enabled.

The desired and only planned website lead source is:

```text
Formspree JSON response success
→ one gtag('event', 'generate_lead', ...)
→ event parameters include rfq_id
```

### Exact Source Scope

Implementation candidate:

1. `layouts/shortcodes/formspree.html`

Preserve unchanged:

- `layouts/shortcodes/formspree-ko.html` pending the existing Korean-route
  decision.
- `layouts/partials/analytics.html`.
- `hugo.toml`.
- Formspree endpoint and all existing fields.

### Submission Flow

1. Form initialization gets or creates a pending `rfq_id`.
2. UTC date plus six cryptographic random bytes create:
   `RFQ-YYYYMMDD-XXXXXXXXXXXX`.
3. Hidden `rfq_id` is submitted with Formspree data.
4. A synchronous in-flight lock prevents a second submit.
5. The submit button is disabled.
6. A known failure re-enables an explicit retry and retains the same ID.
7. An unknown network outcome must not auto-native-submit; it requires explicit
   retry or fallback instructions.
8. A Formspree successful JSON response emits at most one client
   `generate_lead`.
9. The event includes the same `rfq_id` and existing non-PII RFQ parameters.
10. Successful completion clears the pending attempt.

### Native Fallback

- No-JavaScript submission remains functional.
- A no-JS submission has no generated `rfq_id`.
- JS-enabled native fallback can submit the hidden ID but must not emit a
  client-side success event.
- Native fallback receipt must be reconciled by an approved server or manual
  process; it cannot be inferred from browser success.

### Data Contract

| Layer | Value |
|---|---|
| Website hidden field | `rfq_id` |
| Formspree request/receipt | Same `rfq_id` |
| GA4 event parameter | `generate_lead.rfq_id` |
| Email subject | `subject` with `{{ rfq_id }}` only after separate approval |
| Email body | Same ID only after account behavior is verified |
| Future CRM field | Same `rfq_id`; out of current implementation scope |
| GA4 custom dimension | Not required |
| Server-side Formspree deduplication | Not available / not claimed |

### Formspree Approval Dependency

Current evidence:

- Free plan is verified.
- Email notification plugin is enabled.
- Existing custom fields are stored.
- Default email rendering of a future `rfq_id` is `NOT VERIFIED`.
- Current `_subject` behavior as the Subject header is `NOT VERIFIED`.
- Documented `subject` variable substitution is `VERIFIED DOCUMENTATION`.
- Custom templates require Business and are not approved.

The source implementation may add the hidden `rfq_id` after source authorization,
but must not claim email presentation until separately verified.

### Required Verification After Source Implementation

1. Production and Staging build.
2. Hidden field and generated format check.
3. No-PII inspection.
4. In-flight and duplicate-click behavior.
5. Failed response: no `generate_lead`.
6. Unknown network result: no automatic ambiguous resubmission.
7. Successful AJAX response: one `generate_lead`.
8. Same `rfq_id` in browser request and GA4 event.
9. Same ID in Formspree receipt.
10. Email behavior checked only under separate Formspree approval.
11. New RFQ generates a new ID.
12. Runtime event count checked through DebugView.

No source change is authorized by this specification closure.

## 18. 2026-11-26 Implementation and Production Verification

Final status:

`PRODUCTION RELEASE COMPLETE — SOME MEASUREMENT GAPS REMAIN`

The previously proposed design is now implemented in release commit
`209ec0fd386dedf2ec424fc517a3895dfc96cbee`. The source diff from baseline
`0bdb005` contains only:

- `layouts/partials/analytics.html`
- `layouts/shortcodes/formspree.html`

The implementation generates `RFQ-YYYYMMDD-XXXXXXXXXXXX`, uses six
cryptographically secure random bytes, preserves the same ID during known
retries, prevents concurrent double submission, emits one client
`generate_lead` only after confirmed Formspree success, and keeps native
fallback from claiming a GA4-confirmed success.

### Deployment

| Layer | Status |
|---|---|
| Source implementation | `VERIFIED COMPLETE` |
| Production deployment | `VERIFIED COMPLETE` |
| Formspree receipt | `VERIFIED` |
| Notification email inbox receipt | `NOT VERIFIED` |
| GA4 actual event receipt | `VERIFIED` |

Production `main` is merge commit
`9f141e1380d5985826bf3468ea29c223d55d602b`, containing release commit
`209ec0f`. GitHub Actions run `45760487830` and GitHub Pages deployment
`8077162596` both report success for `9f141e1`.

An ordinary fast-forward push was rejected after the remote ref advanced. A
subsequent fetch confirmed that the remote had advanced to the expected merge
commit. No force push, duplicate release branch or replacement source commit was
created.

### Runtime Result

One controlled production RFQ was submitted and then not repeated:

| Field | Observed value |
|---|---|
| RFQ ID | `RFQ-20261126-BF29E3AD3BA6` |
| Formspree submission | `174445634` |
| Stored subject | `AlumCasting RFQ RFQ-20261126-BF29E3AD3BA6 — New Inquiry` |
| GA4 Realtime `generate_lead` count | `1` |
| Matching GA4 collect requests | `1` |
| GA4 `rfq_id` | Same as Formspree |
| Personal information in GA4 event | None observed |

The dynamic Formspree `subject` contract is verified at the stored-submission
layer. Actual email inbox receipt and default email-body rendering remain
`NOT VERIFIED`; Formspree’s notification action status `processed` is not treated
as independent inbox delivery evidence.

### Rollback

Rollback remains prepared but unexecuted. The safe procedure is:

1. Revert source commit `209ec0f` in a new isolated branch.
2. Run Production and Staging builds and RFQ validation.
3. Deploy through the normal pull-request and GitHub Actions path.
4. Verify the resulting production commit and RFQ page.

Force push and production history rewrite are not part of the rollback plan.

No additional production RFQ test is required for this implementation record.
