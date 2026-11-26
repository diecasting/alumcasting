# RFQ External Administrator Verification Report

Date: 2026-11-25

Production SHA: `0bdb005af33ecb2f996bd3db0b1618943e825d35`

Status: `PARTIALLY VERIFIED — SPECIFIC ADMIN ACCESS REQUIRED`

## 1. Scope and Safety

This verification was read-only. Authorized, already authenticated browser
sessions were used to inspect:

- The available Google Tag Manager account.
- The GA4 web stream and Google tag settings.
- The public Google tag configuration served for `GT-M38DWB5D`.
- The Formspree form identified by endpoint `xpqgbdly`.
- Current local source and existing RFQ audit records.

No RFQ was submitted. No email was sent. No GTM, GA4, Google Ads, Formspree,
consent, privacy, form-field, source-code or Git state was changed.

Personal information visible in administration screens is omitted or redacted
from this report.

## 2. Executive Result

| Question | Result |
|---|---|
| Can a random non-PII `rfq_id` be added to the website form? | YES, technically; source implementation is not authorized |
| Can Formspree store arbitrary custom fields? | PARTIALLY VERIFIED — the live form stores `annual_volume`; direct `rfq_id` submission was not performed |
| Can Formspree show the same ID in a notification email? | PARTIALLY VERIFIED — custom fields and templates support it, but current default-email rendering was not proven |
| Can Formspree support `{{ rfq_id }}` in a subject? | YES according to official documentation, using a hidden field named `subject`; current form uses `_subject` |
| Can the client pass `rfq_id` to GA4 `generate_lead`? | YES, by adding the parameter to the existing successful-submission event; this is not authorized yet |
| Can one successful RFQ be guaranteed one GA4 lead event today? | NO — the public Google tag contains two identical page-view-based event-creation rules |
| Can duplicate Formspree records be prevented by `rfq_id` alone? | NO — Formspree documents duplicate requests and does not promise deduplication by a custom field |
| Is implementation ready? | NO — lead-rule consolidation and email/receipt verification remain unresolved |

Implementation readiness remains **Medium-High** complexity. The website change
alone is low-medium complexity; the external tracking and reconciliation
dependencies create the higher risk.

## 3. Evidence Inventory

| Evidence | Method | Result |
|---|---|---|
| Production Google tag `GT-M38DWB5D` | Public tag resource opened | VERIFIED |
| GA4 web stream `Alumcasting` | Read-only Analytics Admin inspection | VERIFIED; measurement ID `G-B48RJMJ2W6` |
| GA4 destination in Google tag settings | Read-only Google tag inspection | VERIFIED; one destination, `G-B48RJMJ2W6`, optimized, linked |
| Google tag automatic events | Read-only settings inspection | VERIFIED; page history, scroll, outbound click, form, video and download detection enabled |
| Google user-provided data | Read-only settings inspection | VERIFIED configured; automatic email, phone and name/address detection enabled |
| Two `generate_lead` creation rules | Public Google tag JSON | VERIFIED configured; runtime duplication NOT VERIFIED |
| Formspree form identity | Authenticated Formspree view | VERIFIED; endpoint and displayed form name recorded below |
| Formspree custom-field storage | Existing submission list | PARTIALLY VERIFIED; custom `annual_volume` field visible |
| Formspree notification recipient | Workflow settings | VERIFIED as one mailbox; address redacted |
| Formspree custom email template | Settings UI and official documentation | Business plan feature; not currently available |
| Current `rfq_id` parameter | Local form and public tag JSON | VERIFIED ABSENT |

External evidence URLs:

- [Public Google tag `GT-M38DWB5D`](https://www.googletagmanager.com/gtag/js?id=GT-M38DWB5D)
- [Formspree email subject documentation](https://help.formspree.io/articles/building-your-form/email-subject-line/)
- [Formspree special fields](https://help.formspree.io/articles/building-your-form/special-fields)
- [Formspree custom email templates](https://help.formspree.io/fr/articles/advanced-features/setting-custom-email-templates)
- [Formspree duplicate-submission guidance](https://help.formspree.io/articles/troubleshooting/delay-on-submit-or-double-submissions)

Local evidence:

- `layouts/shortcodes/formspree.html`
- `layouts/shortcodes/formspree-ko.html`
- `hugo.toml`
- `SEO-AUDIT/RFQ-END-TO-END-VERIFICATION-REPORT.md`
- `SEO-AUDIT/GOOGLE-ADS-RESOURCE-DECOMMISSION-RECOMMENDATION.md`

## 4. Resource Identity and Access Boundary

### Google Tag and GTM Container Are Different Resources

The value `GT-M38DWB5D` is a **Google tag ID**, not a normal `GTM-xxxxxxx`
container ID. The available general Tag Manager interface exposed only:

- Account label: `Alumcasting`.
- Container: `GTM-NH3766HW`.
- Internal container ID: `245341139`.

That container is not the Google tag configuration served by Production. Its
tags and triggers must not be attributed to `GT-M38DWB5D`.

The GA4 Admin path exposed the Google tag settings associated with the actual
Alumcasting GA4 stream:

- Google tag settings label: `G-PWNDQRH47H`.
- Internal Google tag container ID: `245344641`.
- Workspace: `2`.
- Destination: `G-B48RJMJ2W6`.
- Public tag resource: `GT-M38DWB5D`.

The public resource contains both `G-PWNDQRH47H` and `GT-M38DWB5D` and identifies
internal container `245344641`. This establishes the relationship between the
GA4 settings surface and the public Google tag configuration.

### Version-State Discrepancy

The Google tag Versions view reports:

- Version `1`.
- Name `Empty Container`.
- Published `2026-04-09`.
- “Empty Container is Live.”

Workspace `2` separately shows one Google Analytics tag for `G-B48RJMJ2W6`, zero
custom triggers, zero variables and zero pending workspace changes. The public
`GT-M38DWB5D` resource nevertheless contains automatic-event settings, user-data
settings, conversion marking and event-creation rules.

This is recorded as an unresolved Google-tag configuration-source discrepancy.
It does not invalidate the public configuration evidence, but it prevents a
claim that the Team workspace is a complete and authoritative diff of the live
Google tag behavior.

## 5. Production Google Tag Findings

### Configured Lead Paths

The public `GT-M38DWB5D` configuration contains:

| Public tag ID | Function | Finding |
|---:|---|---|
| 8 | GA4 destination/config | Tracking ID `G-B48RJMJ2W6` |
| 5 | GA4 destination | Final destination for `G-B48RJMJ2W6` |
| 20 | Conversion marking | Marks event name `generate_lead` as a conversion/key event |
| 18 | Event creation | Creates `generate_lead` when a `page_view` has `page_location` containing `thank-you` |
| 19 | Event creation | Creates the same `generate_lead` event under the same `page_view` condition |

Tags 18 and 19 are functionally identical in the public configuration. The
website form does not otherwise emit a `thank-you` event name; it emits a client
`generate_lead` only after a successful AJAX response.

These two rules can create duplicate leads for qualifying page views and do not
carry the same client-generated `rfq_id`. Runtime duplication was not observed
during this verification, so the finding is a confirmed configuration risk, not
a confirmed duplicate-event incident.

### Automatic Measurement and Personal-Data Settings

The same public configuration shows:

- Automatic page view, history, scroll, outbound click, form, video and download
  measurement enabled.
- Form interactions enabled, which provides the configured source for
  `form_start`/`form_submit`.
- Automatic user-provided data detection enabled for email, phone and
  name/address.
- Code input enabled.
- Manual selector input enabled.
- Google Signals set to `ENABLED`.
- Enhanced Conversions source configuration present.

The authenticated Google tag settings independently show the same automatic
event and user-provided-data settings. These are configuration findings. They do
not prove that personal data was transmitted in any particular submission.

### Missing `rfq_id`

No `rfq_id` exists in:

- `hugo.toml`.
- `layouts/shortcodes/formspree.html`.
- `layouts/shortcodes/formspree-ko.html`.
- The public `GT-M38DWB5D` configuration.

There is no naming collision to migrate. A new event parameter can be added to
the existing successful-submission event if approved.

### Lead Trigger and Exclusion Check

No custom `generate_lead` tag, trigger, blocking trigger or tag sequence was
found in the Google tag workspace. The lead behavior is represented by generated
Google tag settings and event-creation rules rather than normal workspace tags.

The standard GTM container `GTM-NH3766HW` contains a `Thanks Page View Trigger`,
but it is not part of the Production `GT-M38DWB5D` tag path and is excluded from
this conclusion.

## 6. Formspree Findings

### Form Identity

| Item | Observed state |
|---|---|
| Endpoint | `https://formspree.io/f/xpqgbdly` |
| Displayed form name | `Kingship Web RFQ` |
| Form enabled | YES |
| Submission archive | Enabled |
| Archive window shown | 30 days |
| Recent records | Mixed AlumCasting and KingShip submissions |
| Notification action | Email plugin |
| Recipient count | One mailbox |
| Recipient identity | Redacted |
| Reply-to behavior | Submitter email field |

The shared form name does not match AlumCasting and may create operational
confusion. Renaming it is a Formspree configuration change and is not authorized
by this report.

### Custom Fields

The form accepts and stores fields beyond the standard name/email/message set.
This is directly evidenced by the visible `annual_volume` field in stored
submissions.

Conclusion:

- `rfq_id` is expected to be accepted and stored as a custom field.
- Direct proof that `rfq_id` appears in a Formspree receipt was not produced
  because creating a test submission is prohibited.
- The current form has no `rfq_id` field.

### Subject Configuration

The present form sends a hidden `_subject` field. Existing Formspree submissions
display `_subject` as a stored custom field.

Current official Formspree documentation instead identifies a field named
`subject` as the documented notification-subject mechanism. It supports
variables such as `{{ rfq_id }}` on all plans.

The available evidence does not prove that `_subject` controls the current
email Subject header. The safest approved migration is:

```html
<input
  type="hidden"
  name="subject"
  value="AlumCasting RFQ {{ rfq_id }} — New Inquiry">
```

The existing `_subject` field should remain until the replacement is reviewed
and tested, then be removed in the same approved release.

### Email Body and Templates

Formspree custom templates support all submitted fields, including a field named
`rfq_id`, via `{{ rfq_id }}` or the `_fields` list.

The current form Settings UI offers a custom-email-template upgrade path labeled
`Upgrade to Business`; custom templates are therefore not currently available on
the observed plan.

Whether the current default notification automatically displays a new custom
`rfq_id` field is **not directly proven**. Avoid purchasing or changing plans
until a read-only example email or an approved synthetic submission confirms the
default notification behavior.

### Duplicate Submissions

Official Formspree documentation confirms that a second click can create:

- A second request.
- A second usage-quota event.
- A second submission record.

Formspree does not state that a custom `rfq_id` performs automatic
deduplication. The website therefore needs an in-flight guard, disabled submit
button and stable retry ID. The ID supports investigation; it does not replace
request-level duplicate prevention.

## 7. Readiness Matrix

| Component | Readiness | Evidence / remaining gap |
|---|---|---|
| Random `RFQ-YYYYMMDD-XXXXXXXXXXXX` generation | READY TO IMPLEMENT | Use UTC and six cryptographic random bytes |
| Hidden `name="rfq_id"` field | READY TO IMPLEMENT | Formspree custom-field precedent exists |
| Same ID in Formspree receipt | PARTIALLY VERIFIED | Requires approved synthetic test or redacted receipt example |
| Same ID in default notification email | NOT PROVEN | No test submission or authorized email inspection |
| Same ID in email subject through `subject` | DOCUMENTED / CHANGE REQUIRED | Official syntax supports `{{ rfq_id }}` |
| Same ID in GA4 `generate_lead` | READY TO IMPLEMENT AFTER APPROVAL | Add parameter only to successful client event |
| One successful submission → at most one GA4 lead | BLOCKED | Public tags 18 and 19 create duplicate URL-based lead events |
| Duplicate-click request prevention | READY TO IMPLEMENT | In-flight state and disabled button required |
| Ambiguous network retry safety | REQUIRES DESIGN APPROVAL | Do not auto-native-submit after unknown outcome |
| Native no-JS ID | NOT AVAILABLE | Static site cannot reliably generate a browser ID without JavaScript |
| Long-term GA4 reconciliation | ACCESS REQUIRED | DebugView/BigQuery access and retention decision required |

## 8. Recommended Implementation Sequence

1. Obtain approval to disable or consolidate the two public `thank-you`
   `generate_lead` creation rules.
2. Obtain approval for the source change to generate and preserve one random
   `rfq_id`, with in-flight and emitted-ID guards.
3. Replace `_subject` with documented `subject` syntax in the same form change,
   subject to Formspree/business approval.
4. Attach `rfq_id` to the existing successful `generate_lead` event only after a
   successful Formspree JSON response.
5. Confirm the default notification contains the ID. If it does not, either
   approve a Business template or use an approved receipt export instead.
6. Run one approved synthetic test using a dedicated test identity.
7. Reconcile the browser request, Formspree receipt, notification email and GA4
   event using the same ID.
8. Do not register `rfq_id` as a GA4 custom dimension by default. It is
   high-cardinality; use a constrained DebugView check and, when approved,
   BigQuery export for durable detailed reconciliation.

## 9. Approval and Access Requirements

| Requirement | Status | Why it is needed |
|---|---|---|
| Approve source form patch | REQUIRED | Adds ID generation, hidden field and duplicate guards |
| Approve `_subject` → `subject` wording | REQUIRED | Clears subject-behavior ambiguity |
| Approve removal of public lead rules 18 and 19 | REQUIRED | Prevents duplicate/non-submission lead events |
| Approve Formspree form rename | OPTIONAL / BUSINESS | Current name is `Kingship Web RFQ` |
| Clarify Google tag Versions vs public settings | ADMIN REQUIRED | Live version says empty while public settings contain lead rules |
| Confirm default email includes `rfq_id` | ADMIN/TEST REQUIRED | Current default rendering is unproven |
| Approve synthetic RFQ test | REQUIRED | Verifies receipt/email/GA4 correlation |
| Approve privacy/retention review | REQUIRED | `rfq_id` becomes pseudonymous when linked to a lead |
| Approve deployment | REQUIRED | No source or external change is currently released |

Read-only access recovery package:

- Analytics owner: provide a redacted explanation of the Event Creation rules
  that generate `generate_lead` from `page_location` containing `thank-you`.
- Analytics owner: confirm whether the Google tag Versions “Empty Container”
  state is expected while public settings contain generated lead rules.
- Formspree owner: provide a redacted notification email showing whether a
  custom hidden field appears by default, or approve the synthetic test.
- Formspree owner: confirm whether `_subject` is intentionally supported or
  should be migrated to the documented `subject` field.

No password, recovery code or account credential is required.

## 10. Safety Verification

| Check | Result |
|---|---|
| Website source changed | NO |
| B1 `analytics.html` modified | NO |
| GTM configuration changed | NO |
| Google tag/GA4 changed | NO |
| Formspree settings changed | NO |
| Production RFQ submitted | NO |
| Email or outbound message sent | NO |
| Account permissions changed | NO |
| Git commit/push/merge/deploy | NO |
| Primary mixed worktree overwritten | NO |

## 11. Source State at Verification

Primary mixed worktree:

- Path: `D:\Codex-SEO\alumcasting`.
- Branch: `main`.
- HEAD: `8fef6d85043a33efd4179c3eff0f7897ef25226e`.
- Upstream state: behind `origin/main` by four commits.
- Existing modified and untracked files were preserved.

Isolated B1 worktree:

- Path:
  `C:\Users\Ansonzhang\.codex\worktrees\alumcasting-content-release\alumcasting`.
- Branch: `codex/ops-hot-chamber-discovery-20261125`.
- HEAD: `0bdb005af33ecb2f996bd3db0b1618943e825d35`.
- Existing uncommitted B1 file:
  `layouts/partials/analytics.html`.
- No source change was made during external verification.

## Final Status

`PARTIALLY VERIFIED — SPECIFIC ADMIN ACCESS REQUIRED`

The `rfq_id` architecture is technically implementable, but it must not be
described as production-ready or deployed until the duplicate `generate_lead`
rules are resolved, the documented subject mechanism is approved, and one
privacy-approved synthetic submission proves receipt/email/GA4 correlation.

## 2026-11-26 Amendment — Final Readiness Verification

This amendment adds evidence without replacing the 2026-11-25 findings above.
The browser sessions and public tag configuration were inspected again in
read-only mode. No form was submitted, no email was opened or sent, and no
configuration or source file was changed.

Current decision:

`READY AFTER EXTERNAL CONFIGURATION CORRECTION`

### A. Duplicate GA4 Lead Rules — Resolved Configuration Findings

The GA4 custom-event interface for stream `G-B48RJMJ2W6` shows exactly two custom
events with the same name and conditions:

| Item | Rule 1 / public tag 19 | Rule 2 / public tag 18 |
|---|---|---|
| Source event | `event_name equals page_view` | `event_name equals page_view` |
| Additional condition | `page_location contains thank-you` | `page_location contains thank-you` |
| Created event name | `generate_lead` | `generate_lead` |
| Destination | `G-B48RJMJ2W6` | `G-B48RJMJ2W6` |
| Copy source parameters | Disabled | Disabled |
| Execution condition | All conditions must match | All conditions must match |
| Mutual exclusion | None | None |

The public Google tag confirms two separate `__ogt_event_create` components with
priorities `6` and `5`. The rules are therefore:

- Two independent event definitions, not one rule with two display rows.
- Functionally duplicate configurations.
- Potentially overlapping because there is no condition that makes one exclude
  the other.
- NOT VERIFIED as producing two received GA4 events at runtime. Proving runtime
  duplication requires an authorized test that activates a page view whose
  `page_location` contains `thank-you`.

`generate_lead` is also configured as a GA4 key event. That marking is a separate
setting and does not itself create another event.

The earlier Google tag “Empty Container is Live” discrepancy no longer blocks
this decision. The two custom-event rules are directly visible in GA4 property
`527174041` under stream `G-B48RJMJ2W6`, and the public tag contains their
generated event-creation components. The GTM-Lite container version record and
the GA4 property-level custom-event configuration are separate configuration
layers; the live public configuration, rather than the empty container-version
label, is the relevant evidence.

### B. Independent Client Lead Event

`layouts/shortcodes/formspree.html` contains an independent client event:

```text
gtag('event', 'generate_lead', {
  currency: 'USD',
  value: 10,
  lead_type: 'rfq',
  form_name: 'contact-form',
  form_id: 'rfq-form'
})
```

It executes once inside the successful `res.ok || res.success` callback. The
current AJAX success path replaces the form in place and does not change
`page_location`. Therefore, in the documented normal AJAX path:

- The client callback emits one `generate_lead`.
- The two URL-based creation rules do not match solely because the success
  message contains the words “Thank you.”
- A single successful AJAX RFQ likely produces one GA4 lead event.

That remains a configuration-based inference, not runtime proof.

Runtime multiplicity remains possible through:

- Two overlapping URL rules if a navigation reaches a `thank-you` URL.
- Two submitted fetch requests if the user clicks twice; the current form has no
  in-flight guard or disabled submit button.
- A native fallback followed by a Formspree redirect whose final location
  happens to contain `thank-you`.
- Unknown retry behavior after an ambiguous network outcome.

The client callback itself does not prevent duplicate events for duplicate
requests.

### C. Minimum Safe Lead-Event Correction

Recommended minimum:

1. Disable both URL-based `generate_lead` custom events, tags 18 and 19.
2. Retain `generate_lead` as a GA4 key event.
3. Keep the client callback as the controlled lead signal only after a
   confirmed Formspree JSON success.
4. Add the proposed `rfq_id` parameter to that callback.
5. If URL-based fallback measurement remains necessary, replace it later with a
   dedicated success page or an independently confirmed receipt mechanism.

Disabling only one rule would remove one duplicate but leave a lead signal that
is still disconnected from proof of a successful submission. No Google tag or
GA4 change was made.

### D. Formspree Plan and Email Capabilities

Verified account state:

| Item | Status | Evidence |
|---|---|---|
| Current plan | VERIFIED FREE | Plans page marks `FREE FOREVER` as Current; account shows `6 / 50` monthly submissions |
| Form | VERIFIED | Endpoint `xpqgbdly`; form enabled |
| Submission archive | VERIFIED | Enabled; Free plan dashboard advertises 30-day archive |
| Email notification action | VERIFIED | Enabled, one recipient mailbox, address redacted |
| Reply-to behavior | VERIFIED BY UI | Submitter `email` becomes Reply-To |
| Custom email template | NOT AVAILABLE ON CURRENT PLAN | Settings and plans require Business |
| Formspree submission record | VERIFIED | Custom `annual_volume` field appears in stored submissions |
| Default notification body | NOT VERIFIED | Platform UI does not expose the rendered default email without a submission |

Field-specific result:

| Destination | `rfq_id` result |
|---|---|
| Formspree submission record | PARTIALLY VERIFIED through current custom-field storage; direct `rfq_id` test not performed |
| Default notification email body | NOT VERIFIED |
| Email subject through documented `subject` field | VERIFIED AS PLATFORM DOCUMENTATION; not yet tested on this account |
| Email subject through existing `_subject` field | PARTIALLY VERIFIED as a stored field; Subject-header behavior NOT VERIFIED |

Official Formspree documentation states that:

- `subject` is the documented special field for notification Subject.
- `{{ rfq_id }}` can be used in a hidden `subject` value on all plans.
- Any submitted field can be referenced in a custom template.
- Custom templates require a project and the Business plan.

`_subject` is not listed in the current official special-fields documentation.
The account displays `_subject` in submission records, so it is being stored, but
that does not prove it controls the email Subject header. The two fields
therefore have different documented status, not a proven difference in this
account's actual email rendering.

No plan, template, field, notification recipient or submission was changed.

### E. Final `rfq_id` Data Contract

| Layer | Proposed contract | Status |
|---|---|---|
| Website hidden field | `name="rfq_id"` | READY |
| Format | `RFQ-YYYYMMDD-XXXXXXXXXXXX` | READY |
| Date | UTC `YYYYMMDD` | READY |
| Random suffix | Six cryptographically secure random bytes encoded as 12 uppercase hex characters | READY |
| Retry behavior | Reuse ID for retries of the same logical submission attempt | READY |
| New submission | Generate a new ID | READY |
| Formspree record | Store the exact same `rfq_id` string | PARTIALLY VERIFIED |
| Notification body | Display the same ID only if verified by this account | NOT VERIFIED |
| Notification subject | Use documented hidden `subject` with `{{ rfq_id }}` after approval | VERIFIED DOCUMENTATION / TEST REQUIRED |
| GA4 event | `generate_lead` parameter `rfq_id` after confirmed Formspree JSON success | READY |
| GA4 custom dimension | Do not register by default | NOT REQUIRED |
| Future CRM field | Store the same ID for receipt-to-opportunity reconciliation | FUTURE / OUT OF CURRENT SCOPE |

Prohibited identifier inputs:

- Email address.
- Phone number.
- Company name.
- IP address or geolocation.
- Browser fingerprint.
- Any customer or project description.

Lifecycle distinctions:

| State | Meaning |
|---|---|
| Submission attempt | The browser has begun one logical submit action with an `rfq_id`; no success is implied |
| Formspree accepted submission | Formspree returned its documented successful JSON response; the receipt should be reconcilable by the same `rfq_id` |
| GA4 lead event | The browser emitted one `generate_lead` only after Formspree success and before any second event for that ID |
| Human-qualified RFQ | A person has reviewed validity, fit, completeness and duplicate/spam status; this is not a website event |

`rfq_id` is a correlation key, not server-side deduplication. Formspree can still
store duplicate requests. Duplicate prevention must happen in the browser before
a second request, and duplicate detection must be reconciled in the receipt or
future CRM workflow.

### F. Final Readiness Decision and Dependencies

Decision:

`READY AFTER EXTERNAL CONFIGURATION CORRECTION`

Blocking external correction:

| Dependency | Status | Exact resolver |
|---|---|---|
| Disable both duplicate URL-based `generate_lead` custom events | NOT DONE / REQUIRED BEFORE RELEASE | Analytics administrator with edit authority for GA4 property `527174041` and Google tag `GT-M38DWB5D` |
| Approve migration from `_subject` to documented dynamic `subject` | APPROVAL REQUIRED | Formspree account owner for form `xpqgbdly` and business release owner |
| Confirm default email displays `rfq_id` | NOT VERIFIED | Formspree account owner or approved synthetic submission |
| Approve synthetic RFQ/GA4/DebugView test | APPROVAL REQUIRED | Business website owner and analytics administrator |
| Approve pseudonymous ID retention and access | APPROVAL REQUIRED | Privacy/legal reviewer |
| Authorize source implementation and deployment | APPROVAL REQUIRED | Website release owner |

Not blocking the proposed source implementation:

- CRM integration: `NOT REQUIRED` for the current source patch.
- Google Ads: `NOT REQUIRED`.
- Consent/CMP implementation: outside this approval scope and remains frozen.
- A custom paid email template: `NOT REQUIRED` until default-email verification
  proves it necessary.

Implementation may prepare the source patch only after the source approval. It
must not be released as one-success/one-lead until tags 18 and 19 are corrected
and the approved synthetic test is completed.

### G. Amendment Safety Record

| Check | Result |
|---|---|
| Source modified | NO |
| GTM/Google tag/GA4 modified | NO |
| Formspree modified | NO |
| RFQ submitted | NO |
| Email opened, sent or tested | NO |
| Subscription or permission changed | NO |
| Primary mixed workspace preserved | YES |
| Isolated B1 implementation preserved | YES |

Current amendment status:

`READY AFTER EXTERNAL CONFIGURATION CORRECTION`

## 2026-11-26 Amendment — External Correction Approval Package

This amendment prepares the exact administrator change request. It does not
authorize or execute the change. All prior findings remain historical evidence.

Current status:

`EXTERNAL CORRECTION REQUIRED — IMPLEMENTATION NOT AUTHORIZED`

### 1. Scope

The sole approved candidate change in this package is:

- Disable the two URL-based custom events in GA4 property `527174041`.
- Preserve the independent client-side AJAX success event.

Explicitly excluded:

- Google tag replacement or republishing.
- Changes to `GT-M38DWB5D` destination or other Google tag settings.
- Changes to GA4 key-event status.
- Automatic-event settings.
- Google Signals.
- User-provided data settings.
- Consent-related settings.
- Google Ads.
- Formspree fields, subject, template or plan.
- Website source or deployment.

### 2. Exact Rules Requested for Disablement

Both rules are in:

```text
GA4 Admin
→ Data display
→ Events
→ Custom configurations
→ Custom events
```

Property and stream:

- GA4 property ID: `527174041`.
- GA4 stream: `Alumcasting`, measurement ID `G-B48RJMJ2W6`.

| GA4 display order | Custom event | Condition 1 | Condition 2 | Source parameters | Public component |
|---:|---|---|---|---|---|
| 1 | `generate_lead` | `event_name equals page_view` | `page_location contains thank-you` | Copy source parameters: OFF | Tag ID `19`; priority `6` |
| 2 | `generate_lead` | `event_name equals page_view` | `page_location contains thank-you` | Copy source parameters: OFF | Tag ID `18`; priority `5` |

Both create the destination event `generate_lead` for
`G-B48RJMJ2W6`. They have no mutually exclusive condition.

The two rules have identical visible names and conditions. The administrator
must capture a before-change screenshot or exported evidence showing both rows
before acting. Text matching alone is insufficient because another same-named
rule could otherwise be removed by mistake.

### 3. Required Implementation Method

This package requests disablement, not a general GA4 cleanup.

1. Confirm exactly two matching custom-event rows exist.
2. If GA4 provides an approved disable/archive control, disable exactly both.
3. If GA4 provides deletion only, stop. Return the approval request with the
   available action and obtain a separate deletion Authorization. The current
   package does not authorize destructive deletion.
4. Do not edit the Google tag container manually.
5. Do not unmark `generate_lead` as a key event.
6. Do not modify any other custom event.

Rollback reference:

```text
Custom event name: generate_lead
Condition 1: event_name equals page_view
Condition 2: page_location contains thank-you
Copy parameters from source event: OFF
```

If the change causes unexpected loss of the controlled lead signal, restore
both rules or escalate before any source implementation.

### 4. Why the AJAX Success Callback Is Independent

The website callback is implemented in
`layouts/shortcodes/formspree.html` and, for the existing Korean route,
`layouts/shortcodes/formspree-ko.html`.

English behavior:

```text
After res.ok || res.success:
  gtag('event', 'generate_lead', {
    currency: 'USD',
    value: 10,
    lead_type: 'rfq',
    form_name: 'contact-form',
    form_id: 'rfq-form'
  })
```

The callback:

- Calls the Google tag directly through `gtag`.
- Does not call either GA4 custom-event rule.
- Does not depend on a `thank-you` URL.
- Does not require a Google tag container edit.
- Does not depend on the two URL-based rules existing.

Disabling the two URL-based modules therefore will not disable or edit the
independent client callback. The surviving signal will still target
`G-B48RJMJ2W6` through the existing Google tag destination, provided the key
event and destination settings remain unchanged.

This conclusion is `VERIFIED IN SOURCE`. Runtime event receipt after the
external correction remains `NOT VERIFIED` until an approved test is completed.

### 5. Desired Surviving Event Source

| Requirement | Desired state | Current state |
|---|---|---|
| Successful Formspree AJAX response | One confirmed client callback | VERIFIED IN SOURCE |
| Event name | Exactly one client-side `generate_lead` | VERIFIED IN SOURCE |
| URL-based lead creation | None | CONFIGURED TWICE; CORRECTION REQUIRED |
| `generate_lead` key-event designation | Preserved | VERIFIED ACTIVE |
| `rfq_id` event parameter | Add with source implementation | NOT IMPLEMENTED |
| PII in event parameters | None | REQUIRED |
| Duplicate-click suppression | Required in source implementation | NOT IMPLEMENTED |

The URL rules must not be treated as fallback confirmation for a form receipt.
When source implementation occurs, the client event should carry the same
random `rfq_id` stored in the Formspree request.

### 6. Administrator Change Request

Requested approver:

- Business website owner.

Requested implementing administrator:

- Authorized analytics administrator for GA4 property `527174041`, stream
  `G-B48RJMJ2W6`, and Google tag `GT-M38DWB5D`.

Request:

> In GA4 property 527174041, disable only the two listed custom events named
> `generate_lead` whose complete conditions are `event_name equals page_view`
> AND `page_location contains thank-you`, with source-parameter copying
> disabled. Preserve `generate_lead` key-event status, destination
> `G-B48RJMJ2W6`, all automatic-event settings, Google Signals, user-provided
> data settings, consent-related settings and every other Google tag setting.
> Do not change Formspree or website source. Record before/after evidence and
> the exact action method used.

If only destructive deletion is available, do not execute this request until a
separate deletion approval is recorded.

### 7. Post-Change Verification Checklist

No part of this checklist is authorized to run yet.

#### GA4 property

- [ ] Property remains `527174041`.
- [ ] Stream remains `G-B48RJMJ2W6`.
- [ ] Both exact `page_view` + `page_location contains thank-you` custom events
      are disabled or absent.
- [ ] No unrelated custom event was changed.
- [ ] `generate_lead` remains a key event.
- [ ] Automatic measurement settings are unchanged.
- [ ] Google Signals remains in its prior state.
- [ ] User-provided data settings are unchanged.
- [ ] Consent-related settings are unchanged.

#### Public Google tag configuration

- [ ] `GT-M38DWB5D` still resolves and loads.
- [ ] GA4 destination remains `G-B48RJMJ2W6`.
- [ ] No `__ogt_event_create` module remains for the two removed URL rules.
- [ ] Automatic page-view, history, scroll, outbound click, form, video and
      download settings remain unchanged.
- [ ] Google Signals value remains unchanged.
- [ ] Automatic user-data configuration remains unchanged.
- [ ] No unrelated Google tag configuration changed.

#### Approved runtime test

- [ ] Use a separately approved synthetic RFQ or equivalent test-only input.
- [ ] Record the generated non-PII `rfq_id`.
- [ ] Confirm a failed or unknown submission emits no success lead.
- [ ] Confirm a page load whose URL contains a controlled `thank-you` marker
      does not create `generate_lead`.
- [ ] Confirm one successful AJAX submission emits exactly one client
      `generate_lead`.
- [ ] Confirm the event parameter contains the same `rfq_id`.
- [ ] Confirm no name, email, phone, company, message or drawing reference is
      present in the event.
- [ ] Confirm a duplicate click produces no second request or second lead.
- [ ] Inspect GA4 DebugView and retain redacted evidence.
- [ ] Record the GA4 receipt count and testing window.

### 8. Settings That Must Remain Byte-for-Byte/Value-for-Value Unchanged

| Setting family | Required state |
|---|---|
| GA4 destination | `G-B48RJMJ2W6` |
| Google tag ID | `GT-M38DWB5D` |
| `generate_lead` key-event marking | Unchanged |
| Automatic events | Unchanged |
| Google Signals | Unchanged |
| Automatic user-provided data | Unchanged |
| Consent-related settings | Unchanged |
| Google Ads | Out of scope |
| Formspree | Unchanged |
| Website source | Unchanged |

### 9. Separate Formspree Subject Approval

This is not part of the GA4 administrator request.

Current English field:

```html
<input
  type="hidden"
  name="_subject"
  value="AlumCasting RFQ — New Inquiry">
```

Proposed future field, subject to approval and testing:

```html
<input
  type="hidden"
  name="subject"
  value="AlumCasting RFQ {{ rfq_id }} — New Inquiry">
```

Current evidence:

- Formspree documents `subject` as the Subject-header field.
- Formspree documents variable substitution on all plans.
- The existing `_subject` field is stored in submission records.
- Existing `_subject` Subject-header behavior is `NOT VERIFIED`.
- The Formspree account is on the Free plan.
- Custom email templates require Business.
- The default notification email body has not been inspected without a test
  submission and is `NOT VERIFIED` for future `rfq_id` rendering.

Required approvals for this separate change:

- Formspree form owner for endpoint `xpqgbdly`.
- AlumCasting business release owner.
- Approved synthetic submission for receipt and email verification.

No Formspree upgrade is authorized or required by this approval package.

### 10. Authorization Record

| Approval area | Required approver | Current status |
|---|---|---|
| Disable the two GA4 URL rules | Business website owner | NOT APPROVED |
| Execute GA4 correction | Authorized GA4 administrator | NOT ASSIGNED |
| `_subject` to `subject` migration | Formspree owner + business release owner | NOT APPROVED |
| Synthetic RFQ test | Business owner + analytics administrator | NOT APPROVED |
| RFQ ID source implementation | Website release owner | NOT AUTHORIZED |
| Site deployment | Separate release approval | NOT AUTHORIZED |

Final status:

`EXTERNAL CORRECTION REQUIRED — IMPLEMENTATION NOT AUTHORIZED`

## 2026-11-26 Amendment — Authorized Correction Attempt Result

This amendment records the authorized execution attempt. It does not replace
earlier evidence.

Status:

`BLOCKED — NO CONFIGURATION CHANGES MADE`

### Pre-Change Verification

| Item | Observed value | Result |
|---|---|---|
| Authenticated interface | Google Analytics Admin | VERIFIED |
| Administrator identity | Authorized browser session; identity redacted | VERIFIED |
| Account label | `SKSME Alumcasting` | VERIFIED |
| Property ID | `527174041` | VERIFIED |
| Stream | `Alumcasting` | VERIFIED |
| Measurement ID | `G-B48RJMJ2W6` | VERIFIED |
| Production Google tag | `GT-M38DWB5D` | VERIFIED |
| `generate_lead` key-event status | Enabled / checked | VERIFIED |

The custom-event inventory showed exactly two matching rows:

| Display order | Event name | Conditions | Source parameters | Public component |
|---:|---|---|---|---|
| 1 | `generate_lead` | `event_name equals page_view`; `page_location contains thank-you` | Copy OFF | Tag ID `19`; priority `6` |
| 2 | `generate_lead` | `event_name equals page_view`; `page_location contains thank-you` | Copy OFF | Tag ID `18`; priority `5` |

The public `GT-M38DWB5D` configuration independently still retains both event
creation components:

- Tag `19`, priority `6`.
- Tag `18`, priority `5`.

### Rule Detail Verification

Rule A was opened individually and confirmed:

- Custom event name: `generate_lead`.
- Condition 1: `event_name equals page_view`.
- Condition 2: `page_location contains thank-you`.
- Copy parameters from source event: unchecked.
- Destination context: `Alumcasting`, `G-B48RJMJ2W6`.

The rule detail screen did not expose an enabled/disabled or pause control. Its
overflow menu exposed only:

- Copy.
- Delete event configuration.

Because deletion is the only available mutation, the execution gate required an
immediate stop. Rule B was therefore not opened individually for a second
mutation decision. It remains visibly configured in the pre-change inventory.

### Action Result

| Check | Result |
|---|---|
| Reversible disable/pause available | NO |
| Permanent deletion available | YES |
| Rule A deleted | NO |
| Rule B deleted | NO |
| Rule conditions changed | NO |
| Any GA4 setting changed | NO |
| Google tag changed | NO |
| Formspree changed | NO |
| Source changed | NO |

No “Delete,” “Copy,” condition edit or Save control was selected.

### Source Callback Verification

The independent English AJAX callback remains present in
`layouts/shortcodes/formspree.html`:

```text
if (res && (res.ok || res.success)) {
  gtag('event','generate_lead', {
    currency: 'USD',
    value: 10,
    lead_type: 'rfq',
    form_name: 'contact-form',
    form_id: 'rfq-form'
  })
}
```

The callback is independent of the two URL-based event-creation rules and was
not changed. The equivalent callback in the existing Korean shortcode was also
not changed.

### Protected Configuration

Because no mutation was executed, protected settings remain unchanged:

- GA4 destination `G-B48RJMJ2W6`.
- Google tag `GT-M38DWB5D`.
- `generate_lead` key-event marking.
- Automatic events.
- Google Signals.
- User-provided data.
- Consent-related settings.
- Google Ads.
- Formspree.
- Website source.

Post-change field verification is `NOT APPLICABLE`; the attempted change did not
occur.

### Execution Record

- Execution timestamp:
  `2026-11-26T08:12:01+08:00`.
- Operation performed: blocked before mutation.
- Current runtime effect: `NOT VERIFIED`; no test was executed.
- Remaining external-correction requirement: unresolved.

### Required Follow-Up

Executing the requested correction now requires a separate destructive-change
authorization that explicitly approves deletion of both custom events. That
authorization must include:

- A new before-change capture.
- Individual verification of both rows.
- Confirmation that the selected row is removed after each deletion.
- A rollback recreation procedure using the exact conditions and settings above.
- Preservation of all protected configuration.

No such follow-up authorization is inferred from the current task.

## 2026-11-26 Amendment — Deletion Completed and Verified

After the required action-time confirmation, the two authorized GA4 custom
events were permanently deleted.

Final status:

`TWO RULES DELETED — CONFIGURATION AND RECOVERY EVIDENCE VERIFIED`

Runtime behavior remains separately unverified:

`RUNTIME GENERATE_LEAD BEHAVIOR — NOT VERIFIED UNTIL AUTHORIZED END-TO-END TEST`

### Identity and Target Confirmation

| Item | Confirmed value |
|---|---|
| Account label | `SKSME Alumcasting` |
| Administrator identity | Authorized browser session; redacted |
| GA4 property | `527174041` |
| Stream | `Alumcasting` |
| Measurement ID | `G-B48RJMJ2W6` |
| Google tag | `GT-M38DWB5D` |
| Rule A | Display row 1; public tag `19`; priority `6` |
| Rule B | Display row 2; public tag `18`; priority `5` |
| Rule A conditions | `event_name equals page_view`; `page_location contains thank-you` |
| Rule B conditions | `event_name equals page_view`; `page_location contains thank-you` |
| Copy source parameters | OFF for both |
| Unrelated custom events | None existed in this stream |

### Pre-Deletion Backup and Recovery Gate

Evidence folder:

`SEO-AUDIT/AUTONOMOUS/EVIDENCE/2026-11-26-RFQ-GA4-DUPLICATE-RULES/`

Backup completed:

`2026-11-26T08:26:56.6622578+08:00`

Evidence:

- Both-rule list screenshot.
- Full rule A configuration screenshot.
- Full rule B configuration screenshot.
- GA4 Create Event workflow screenshot.
- Machine-readable reconstruction record.
- Public Google tag before snapshot.
- SHA-256 hashes in `BACKUP-MANIFEST.md`.

Recovery assessment: `VERIFIED FOR FUNCTIONAL RECREATION`.

The GA4 Create Event workflow can recreate both custom events with the same
event name, stream, source event, URL condition and parameter behavior.

Limitations:

- Public component tag IDs were generated as `18` and `19`.
- Priorities were generated as `5` and `6`.
- GA4 does not expose a user-editable visible rule ID for exact restoration.
- Recreated rules may receive different generated IDs or priority ordering.

### Deletion Result

Deletion method:

- GA4 Admin custom-event delete action.
- Permanent deletion, not a synthetic status change.
- Both configured rules were deleted individually.
- No other custom event was selected or changed.

Deletion was confirmed by the empty custom-event list at:

`2026-11-26T08:39:14+08:00`

Final verification completed at:

`2026-11-26T08:42:27+08:00`

### Post-Deletion Configuration Verification

| Check | Result |
|---|---|
| GA4 event list for `generate_lead` URL rules | VERIFIED ABSENT |
| GA4 custom-event list | VERIFIED EMPTY |
| Unrelated custom-event deletion | VERIFIED — none existed |
| `generate_lead` key event | VERIFIED still enabled |
| Stream `G-B48RJMJ2W6` | VERIFIED unchanged |
| Public Google tag `GT-M38DWB5D` | VERIFIED present |
| Public GA4 destination | VERIFIED present |
| Public `__ogt_event_create` components | VERIFIED absent |
| Public `thank-you` condition | VERIFIED absent |
| Public `generate_lead` key-event marking | VERIFIED present |
| Automatic history, scroll, outbound, form, video and download logic | VERIFIED present |
| Page-view measurement | VERIFIED in GA4 stream UI as enabled |
| Google Signals | VERIFIED still configured as `ENABLED` |
| Automatic user-provided data | VERIFIED still enabled |
| Consent-specific settings | NOT INDEPENDENTLY VERIFIED; no consent UI was changed |
| Google Ads | NOT REQUIRED; unchanged |
| Formspree | NOT REQUIRED; unchanged |
| AJAX success callback | VERIFIED unchanged |

Public tag IDs `18` and `19` are reused by unrelated generated components after
deletion. They cannot be used as post-deletion presence tests. The public
configuration instead confirms:

- `__ogt_event_create`: 0 occurrences after deletion, versus 6 before.
- `thank-you`: absent after deletion, versus present before.
- `generate_lead` conversion/key-event marking: still present.
- Google tag and GA4 destination: still present.

### Source Callback

`layouts/shortcodes/formspree.html`, line 99-100, still runs
`gtag('event','generate_lead', ...)` only after:

```text
res && (res.ok || res.success)
```

The file remains unchanged from its pre-deletion state. Its diff check passed.
The existing Korean form callback also remained unchanged.

### Formspree Capability Status

| Capability | Status |
|---|---|
| Formspree current plan is Free | VERIFIED |
| Email notification action enabled | VERIFIED |
| One recipient mailbox configured | VERIFIED; address redacted |
| Approximate Free quota shown | VERIFIED; `6 / 50` |
| Submission archive enabled | VERIFIED; Free plan displays 30-day limit |
| Existing custom fields persisted in submission records | PARTIALLY VERIFIED using `annual_volume` precedent |
| Future `rfq_id` appears in Formspree submission record | PARTIALLY VERIFIED; direct receipt test not performed |
| Default notification body includes arbitrary custom fields | NOT VERIFIED |
| Future `rfq_id` appears in default notification body | NOT VERIFIED |
| Documented hidden `subject` can contain `{{ rfq_id }}` | VERIFIED as product documentation |
| Existing `_subject` acts as the email Subject header | NOT VERIFIED |
| Custom email template on current plan | NOT AVAILABLE ON FREE |
| Formspree server-side deduplication by custom `rfq_id` | NOT AVAILABLE / NOT CLAIMED |

The default email body must not be described as supporting `rfq_id` until a
separately authorized receipt/email test or a redacted real notification example
is provided.

### Remaining Verification Boundary

No RFQ was submitted and no email was sent. Therefore:

- Exactly one GA4 lead event per successful RFQ is `NOT VERIFIED`.
- GA4/Formspree/email correlation by `rfq_id` is `NOT VERIFIED`.
- Elimination of observed runtime duplicate events is `NOT VERIFIED`.
- The controlled AJAX callback is configured in source but production runtime
  receipt remains unverified.

An approved synthetic test is still required.

## 2026-11-26 Amendment — Production Implementation and Controlled Verification

Final status:

`PRODUCTION RELEASE COMPLETE — SOME MEASUREMENT GAPS REMAIN`

### Five Delivery States

| Delivery layer | Status | Evidence boundary |
|---|---|---|
| Source implementation | `VERIFIED COMPLETE` | Commit `209ec0f` changes only `layouts/partials/analytics.html` and `layouts/shortcodes/formspree.html` |
| Production deployment | `VERIFIED COMPLETE` | Merge commit `9f141e1`; Actions run `45760487830`; GitHub Pages deployment `8077162596` |
| Formspree receipt | `VERIFIED` | Submission `174445634`; same `rfq_id`; dynamic subject stored |
| Notification email inbox receipt | `NOT VERIFIED` | Plugin status is `processed`; an authorized inbox was not opened |
| GA4 actual event receipt | `VERIFIED` | Exactly one Realtime `generate_lead` and one matching collect request |

### Release Identity

| Item | Verified value |
|---|---|
| Baseline main | `0bdb005af33ecb2f996bd3db0b1618943e825d35` |
| Release branch | `codex/rfq-unique-id-20261126` |
| Release source commit | `209ec0fd386dedf2ec424fc517a3895dfc96cbee` |
| Production merge commit | `9f141e1380d5985826bf3468ea29c223d55d602b` |
| Pull request | `#2` |
| GitHub Actions run | `45760487830` |
| GitHub Pages deployment | `8077162596` |
| Deployment timestamp | `2026-11-26T05:16:25Z` |
| Deployment conclusion | `success` |

A normal fast-forward push was attempted without force. Git rejected it because
remote `main` had advanced. Fetching showed that it had advanced to the expected
merge commit `9f141e1`, which directly contains the already validated release
commit `209ec0f`. No retry, replacement commit or force push was performed.

### Controlled RFQ

Only one controlled production RFQ was submitted:

| Item | Verified value |
|---|---|
| Test marker | `INTERNAL SYSTEM TEST — NOT A CUSTOMER RFQ — DO NOT PROCESS AS A SALES INQUIRY` |
| RFQ ID | `RFQ-20261126-BF29E3AD3BA6` |
| Formspree submission | `174445634` |
| Formspree timestamp | `2026-11-26T05:19:00Z` |
| Stored `rfq_id` | Exact match |
| Stored `subject` | `AlumCasting RFQ RFQ-20261126-BF29E3AD3BA6 — New Inquiry` |
| Existing business fields | Preserved |
| Notification plugin | `processed` |
| Actual inbox receipt | `NOT VERIFIED` |

The Formspree receipt verifies that the hidden `rfq_id` and dynamic `subject`
were accepted and stored. It does not prove that the configured recipient opened
or received the email. The default email body’s rendering of `rfq_id` also
remains `NOT VERIFIED`.

### GA4 Runtime Evidence

| Check | Result |
|---|---|
| Property | `527174041` |
| Web stream | `G-B48RJMJ2W6` |
| Realtime `generate_lead` count | `1` |
| Matching `analytics.google.com/g/collect` requests | `1` |
| Event name | `generate_lead` |
| `ep.rfq_id` | `RFQ-20261126-BF29E3AD3BA6` |
| Same ID as Formspree | `VERIFIED` |
| Personal information in event parameters | `NONE OBSERVED` |
| Old `thank-you` URL-based lead request | `NOT OBSERVED` |
| Duplicate client-side lead signal | `NOT OBSERVED` |
| Request timestamp | `2026-11-26T05:19:17.023Z` |

This is actual GA4 event-receipt evidence, not a browser simulation. It does not
establish long-term reporting because `rfq_id` is intentionally not registered
as a GA4 custom dimension.

### Production Verification

| Check | Result |
|---|---|
| Homepage and Contact page | HTTP 200 |
| Sitemap URL count | 55 |
| Sitemap pages returning HTTP 200 | 55 / 55 |
| Sitemap pages with incorrect Canonical | 0 |
| Sitemap pages marked noindex | 0 |
| `robots.txt` Sitemap directive | Present |
| Alias response | HTTP 200 with meta refresh |
| Contact internal references | 21 checked; 0 non-200 |
| Hidden `rfq_id` | Present in production HTML |
| Dynamic `subject` template | Present in production HTML |
| Cryptographically secure ID generation | Present |
| Formspree endpoint | Unchanged |
| Production Google tag | Present |
| Staging-host leakage | Not observed |

### Remaining Boundary

No second production RFQ is needed or authorized to close the measurement gap.
The only unresolved delivery evidence is actual inbox receipt and default email
body rendering. Those require an authorized mailbox check or a redacted
notification example from the form owner.

Rollback is prepared but not executed: revert the isolated source commit in a new
branch, validate Production and Staging, and use the standard pull-request
deployment path. No force push or production history rewrite is required.
