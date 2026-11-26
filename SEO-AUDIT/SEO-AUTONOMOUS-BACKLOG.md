# AlumCasting Autonomous SEO Backlog

Last updated: 2026-11-26

> Current operating mode: continuous production operations. The sections below
> are the live prioritized backlog. Batch sections retained farther down are
> historical records and must not be used as the future task sequence.

## Website Health

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P0 | Production availability | HEALTHY | GitHub Pages run `45600329646` succeeded for Release D; production pages verified |
| P1 | Internal-link health | HEALTHY | 0 missing targets in final release build |
| P1 | Image health | HEALTHY | 88 images; 0 missing; 0 missing Alt |
| P1 | Canonical/noindex health | HEALTHY | 0 Production canonical mismatches; 0 unexpected noindex |
| P2 | Sitemap/robots monitoring | MONITOR | 55 Sitemap URLs; Production robots contains Sitemap |

## Technical SEO

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P0 | Homepage public-template claims | APPROVAL REQUIRED | Older unapproved claims remain on production Homepage |
| P0 | Korean route policy | APPROVAL REQUIRED | `/ko/` indexed; child routes unknown/discovered |
| P1 | Alias long-term policy | HOLD | 31 HTTP-200 Meta Refresh aliases |
| P1 | Legacy 404 policy | HOLD | Three known 404 URLs |
| P2 | Contextual internal links | DEPLOYED / VERIFIED | Release C added 8 links; Release D added 3 Hot/Cold Chamber entries; production targets HTTP 200 |

## Search Performance

| Priority | Page | Status | Evidence / next action |
|---|---|---|---|
| P1 | Zamak | MONITOR | 574 impressions; position 11.04 |
| P1 | A380 | MONITOR | 141 impressions; position 16.30 |
| P1 | Motor Housing | MONITOR | 611 impressions; position 21.06 |
| P1 | Automotive | MONITOR | 1,327 impressions; position 50.02 |
| P2 | Explosion-proof | MONITOR | 447 impressions; position 12.52 |
| P2 | Aluminum HPDC | MONITOR | 530 impressions; position 59.02 |

## GEO / AI Discoverability

| Priority | Item | Status | Evidence |
|---|---|---|---|
| P1 | Real AI citations | NOT CONNECTED | No connected platform data |
| P1 | AI-originated RFQ attribution | NOT CONNECTED | No approved tracking/CRM data |
| P2 | Material question coverage | DEPLOYED | A380, A356 and Zamak answers live |
| P2 | Commercial selection answers | DEPLOYED | Motor Housing, EPE and conversion pages live |

## Content Quality

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P0 | Remaining high-risk claims | MONITOR | Hot-chamber 400T–5000T claim corrected and verified in production |
| P1 | Deployed content package | VERIFIED | Release A: 47 pages |
| P2 | A380/Zamak numeric property tables | HOLD | No approved primary source |
| P2 | Post-release content changes | WAIT FOR GSC | Avoid optimization before recrawl evidence |

## Business Evidence

| Priority | Item | Status |
|---|---|---|
| P0 | Brand/entity and asset-rights authorization | PENDING |
| P0 | Certificate holder/scope/public use | PENDING |
| P0 | 5000T ownership and operating state | PENDING |
| P1 | Inspection calibration and coverage | PENDING |
| P1 | Facility, history, image and customer rights | PENDING |

## RFQ Conversion

| Priority | Item | Status |
|---|---|---|
| P0 | CTA/form-start/source events | APPROVAL REQUIRED |
| P1 | GA4/Formspree/CRM attribution | NOT CONNECTED |
| P2 | Contact-page RFQ guidance | DEPLOYED |

## Release Readiness

| Priority | Release | Status | Next action |
|---|---|---|---|
| P1 | Release A+B | DEPLOYED / VERIFIED | Monitor recrawl |
| P1 | Release C: hot-chamber claim + contextual links | DEPLOYED / VERIFIED | Commit `2dddb0c`; Actions `45598869785` succeeded; production checks passed |
| P1 | Release D: Hot/Cold Chamber discovery links | DEPLOYED / VERIFIED | Commit `0bdb005`; Actions `45600329646` succeeded; 4/4 related production pages HTTP 200 |
| Approval | Homepage/shared templates | BLOCKED | Need evidence and business approval |
| Approval | URL/robots/Alias policy | BLOCKED | Need route/redirect decisions |
| Approval | RFQ tracking | BLOCKED | Need privacy/analytics/CRM approval |

## Production Monitoring

| Priority | Check | Status | Next action |
|---|---|---|---|
| P1 | GSC performance baseline | CONNECTED | Compare equal periods after recrawl |
| P1 | Indexing/canonical | VERIFIED | Recheck after Release C |
| P1 | Sitemap/robots | VERIFIED | Verify each production release |
| P2 | Bing Webmaster | NOT CONNECTED | Connect data if authorized |
| P2 | RFQ/qualified lead data | NOT CONNECTED | Connect after approved instrumentation |
| P2 | AI citation data | NOT CONNECTED | Use only real platform evidence |

## Post-Release-D Commercial Growth Audit

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P1 | 90-day search baseline | COMPLETE / ANALYZED | 94 clicks, 59,617 impressions, CTR 0.1577%, position 25.85 |
| P1 | Top 10 commercial opportunities | ANALYZED | Actual GSC rows and risk-ranked actions in `POST-RELEASE-D-COMMERCIAL-GROWTH-OPERATIONS-2026-11-25.md` |
| P1 | Opportunity-page internal discovery | ANALYZED | All ranked targets indexed; low-link pages are not isolated; no source change made |
| P1 | Mobile RFQ usability | PASS | 390 × 844: no horizontal overflow, form/submit visible, no console errors |
| P0 | GA4 / Formspree / qualified RFQ reconciliation | APPROVAL REQUIRED | Read-only GA4 and Formspree export, privacy and CRM definitions required |
| P1 | CTA/form-start/source attribution | APPROVAL REQUIRED | Shared template and form changes remain blocked |
| P1 | AI/search crawler access | PASS | 6 crawler identities × 3 URLs returned HTTP 200 |
| P1 | Recrawl-dependent content decisions | HOLD | Wait for new crawl before editing ISO 14001, A380, Motor, Automotive and other leading pages |
| P2 | Bing Webmaster | NOT CONNECTED | Add connector/permission if authorized |
| P2 | AI citation reporting | NOT CONNECTED | No platform reporting connection |

## RFQ Measurement Readiness

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P0 | GA4 runtime/configuration verification | PARTIALLY VERIFIED | Stream `13725654878` and ID `G-B48RJMJ2W6` inspected; DebugView/BigQuery and event-level correlation still gated |
| P0 | Staging analytics isolation | RISK / APPROVAL REQUIRED | Staging build loads Production GT and Ads tags |
| P0 | Consent mode and privacy notice | APPROVAL REQUIRED | No CMP/consent mode or standalone privacy policy found |
| P0 | Formspree receipt reconciliation | PARTIALLY VERIFIED / TEST HOLD | Form settings and custom-field precedent inspected; no PII read or test submission |
| P0 | Qualified RFQ definition and CRM linkage | APPROVAL REQUIRED | Sales/business/privacy must define qualification and outcomes |
| P1 | CTA/form-start/submit-attempt events | APPROVAL REQUIRED | Package designed; no event code changed |
| P1 | Native fallback success measurement | APPROVAL REQUIRED | Requires receipt/webhook/thank-you architecture |
| P1 | PII-safe attribution and deduplication | APPROVAL REQUIRED | Random attempt ID plus non-PII context; no form values in GA4 |
| P1 | Recrawl monitoring | ACTIVE / WAIT | Recheck high-value pages on 2026-11-30 |
| P2 | File upload | HOLD | Requires Formspree plan, privacy, malware and retention review |

### RFQ Management Approval Queue

Decision pack:
`SEO-AUDIT/RFQ-MEASUREMENT-APPROVAL-DECISION-PACK.md`

| Decision ID | Priority | Status | Approval scope |
|---|---|---|---|
| `RFQ-DEC-A1` | P0 | APPROVED / PARTIALLY VERIFIED | Google tag, GA4 stream and Formspree inspected read-only; duplicate lead-rule and email-rendering gaps remain |
| `RFQ-DEC-B1` | P0 | IMPLEMENTED LOCALLY / DEPLOYMENT HOLD | Remove Production analytics tags from Staging |
| `RFQ-DEC-C1` | P0 | LEGAL REVIEW REQUIRED | Consent behavior, legal basis and firing order |
| `RFQ-DEC-C2` | P0 | LEGAL REVIEW REQUIRED | Privacy/cookie notice and data-use disclosure |
| `RFQ-DEC-D1` | P1 | PENDING APPROVAL | Anonymous CTA/form events with allowlisted non-PII fields |
| `RFQ-DEC-E1` | P1 | PENDING APPROVAL | Random attempt ID and event deduplication |
| `RFQ-DEC-E2` | P2 | PENDING APPROVAL | Formspree hidden attribution fields |
| `RFQ-DEC-E3` | P1 | PENDING APPROVAL | Read-only Formspree receipt reconciliation |
| `RFQ-DEC-F1` | P1 | PENDING APPROVAL | Native fallback, confirmation and duplicate-submission controls |
| `RFQ-DEC-G1` | P1 | PENDING BUSINESS DECISION | Qualified-RFQ definition, owner and reason codes |
| `RFQ-DEC-G2` | P1 | BLOCKED BY G1 | CRM fields, access, retention and opportunity lifecycle |
| `RFQ-DEC-H1` | P2 | NOT REQUIRED — BUSINESS DECISION | No Google Ads campaigns planned |
| `RFQ-DEC-I1` | P2 | BLOCKED | Secure file upload and personal-data handling |

### RFQ Foundation Phase 1 Result

| Item | Status | Evidence / next action |
|---|---|---|
| A1 public configuration inventory | PARTIALLY VERIFIED | Public Google tag, rendered pages and account settings inspected read-only; specific gaps tracked below |
| A1 Google tag version/environment | VERIFIED NON-BLOCKING FOR RFQ | GA4 property custom events and public generated config are directly observable; the empty GTM-Lite version is a separate configuration layer |
| A1 GA4 Admin/events/DebugView | PARTIALLY VERIFIED | Stream and Google tag settings inspected; DebugView/export/approved test still required |
| A1 Formspree receipts/spam/duplicates | PARTIALLY VERIFIED / TEST HOLD | Form settings and custom-field storage inspected; no PII read or test submission |
| B1 Production/Staging isolation | IMPLEMENTED / LOCALLY VALIDATED / DEPLOYMENT NOT AUTHORIZED | Source-level Production-only gate; Staging tags absent |
| C1/C2 consent/privacy design | DESIGN PREPARED / LEGAL REVIEW REQUIRED | No CMP, policy or consent code implemented |
| Public container PII/duplicate risk | CONFIGURATION RISK OBSERVED / RUNTIME UNVERIFIED | Review user-data settings and two `generate_lead` URL rules in Admin |
| Next phase | WAITING FOR ACCESS / LEGAL DECISIONS | Complete A1 admin evidence and C1/C2 review |

### B1 Local Implementation Result

| Item | Status | Evidence |
|---|---|---|
| `RFQ-DEC-B1` source isolation | IMPLEMENTED / LOCALLY VALIDATED | `analytics.html` gated by `hugo.IsProduction` |
| Production non-regression | PASS | 121 generated files unchanged by SHA-256 |
| Staging tag removal | PASS | GT, GA4, Ads and Google tag hosts absent from generated output |
| Staging form | PASS | Form, POST method, endpoint, required fields and submit button preserved |
| SEO regression checks | PASS | Canonical 0 mismatch; aliases 31/31; missing links/images 0 |
| Deployment | NOT AUTHORIZED | Await separate management release approval |
| A1 admin verification | ACCESS REQUIRED | Checklist prepared in B1 implementation report |
| C1/C2 | LEGAL REVIEW REQUIRED | No consent/CMP/policy implementation |

### A1 Administrator Verification Coordination

Reference:
`SEO-AUDIT/RFQ-A1-ADMIN-VERIFICATION-REPORT.md`.

| Evidence item | Status | Next action |
|---|---|---|
| Google tag version/workspace | NON-BLOCKING FOR RFQ | GA4 custom events and public generated configuration inspected; no longer a source-patch dependency |
| Automatic user data | CONFIGURED / RUNTIME NOT VERIFIED | Account settings inspected; approved test trace required |
| Email/phone/address mapping | CONFIGURED / MAPPING NOT VERIFIED | Confirm detected fields and payload |
| Actual PII transmission | NOT VERIFIED | Approved synthetic/redacted runtime test |
| Enhanced Conversions | CONFIGURED / TRANSMISSION NOT VERIFIED | Out of business scope; privacy review frozen |
| Google Signals | CONFIGURED ENABLED / RUNTIME NOT VERIFIED | Legal review remains frozen |
| Two `generate_lead` URL rules | CONFIGURED IN PUBLIC TAGS 18/19 | Approve disable/consolidation before unique-ID implementation |
| Duplicate `generate_lead` | NOT VERIFIED | DebugView/Realtime/export |
| Consent settings/order | C1/C2 FROZEN | Legal/privacy decision required |
| GA4 stream/configuration | PARTIALLY VERIFIED | Stream `13725654878`, ID `G-B48RJMJ2W6`, enhanced measurement observed |

GA4 Realtime receipt evidence has been supplied. Google tag, GA4 stream and
Formspree settings have now been inspected read-only. Remaining gaps are the
duplicate lead rules, default email rendering and approved event-level
correlation.

### External A1 Read-Only Verification

Reference:
`SEO-AUDIT/RFQ-EXTERNAL-ADMIN-VERIFICATION-REPORT.md`.

| Item | Status | Next action |
|---|---|---|
| Production Google tag identity | VERIFIED | Preserve `GT-M38DWB5D`; do not conflate with `GTM-NH3766HW` |
| GA4 destination | VERIFIED | Preserve `G-B48RJMJ2W6` |
| Existing `rfq_id` | VERIFIED ABSENT | Approve source implementation separately |
| Custom Formspree field storage | PARTIALLY VERIFIED | Approved synthetic test required for `rfq_id` receipt |
| Default email displays `rfq_id` | NOT PROVEN | Obtain redacted example or approve test |
| Subject syntax | DOCUMENTED / CHANGE REQUIRED | Approve `_subject` → `subject` |
| Custom email template | BUSINESS PLAN REQUIRED | Do not purchase; default-email test first |
| Duplicate lead rules | CONFIGURED RISK | Approve disable/consolidation of tags 18 and 19 |
| Duplicate Formspree request control | DESIGN READY / IMPLEMENTATION HOLD | Add in-flight button guard in approved source release |

### 2026-11-26 Final RFQ Readiness Amendment

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P0 | Duplicate GA4 lead-rule diagnosis | VERIFIED CONFIGURATION / RUNTIME NOT VERIFIED | Two independent GA4 custom events have identical `page_view` + `page_location contains thank-you` conditions |
| P0 | Minimum GA4 lead correction | APPROVAL REQUIRED | Analytics administrator must disable both URL-based custom events; retain one client success signal |
| P0 | Formspree plan | VERIFIED FREE | Plans page marks Free as Current; account quota is 6/50 |
| P0 | Default email contains future `rfq_id` | NOT VERIFIED | No read-only rendered email evidence; test or redacted example required |
| P1 | `subject` / `_subject` behavior | PARTIALLY VERIFIED | `subject` is documented; `_subject` is stored but Subject-header behavior is not proven |
| P1 | RFQ data contract | READY | `rfq_id` shared by form, receipt, GA4; email conditional; CRM future only |
| P1 | Duplicate-click protection | IMPLEMENTATION HOLD | Current form has no in-flight guard |
| P1 | RFQ readiness decision | READY AFTER EXTERNAL CONFIGURATION CORRECTION | Correct lead rules and verify notification behavior before release |
| P1 | Human-qualified RFQ definition | APPROVAL REQUIRED | Keep separate from submission attempt, accepted receipt and GA4 event |
| P2 | `rfq_id` GA4 custom dimension | NOT REQUIRED | High-cardinality identifier; do not register without separate reporting need |

### 2026-11-26 External Correction Approval Queue

| Priority | Item | Status | Owner / exact action |
|---|---|---|---|
| P0 | Disable duplicate GA4 URL rules | APPROVAL REQUIRED | Business owner approves; authorized GA4 administrator disables both exact rows |
| P0 | GA4 rules after correction | NOT VERIFIED | Confirm custom-event count and public tag components |
| P0 | Protected Google tag settings | MUST REMAIN UNCHANGED | Destination, key-event marking, automatic events, Signals, user data, consent |
| P1 | AJAX success signal | VERIFIED IN SOURCE | Preserve one client `generate_lead` after Formspree success |
| P1 | Approved runtime correction test | NOT AUTHORIZED | Test controlled thank-you URL, successful submission, failure and duplicate click |
| P1 | `_subject` to `subject` migration | SEPARATE APPROVAL REQUIRED | Formspree owner + business release owner |
| P1 | Free-plan email displays `rfq_id` | NOT VERIFIED | Redacted rendered email or approved synthetic submission |
| P1 | Source implementation | NOT AUTHORIZED | Begin only after external correction and business approval |
| P1 | Deployment | NOT AUTHORIZED | Separate release gate |
| P2 | Formspree plan upgrade | NOT REQUIRED NOW | Do not purchase; only reconsider if verified default behavior is insufficient |

Current release-control status:

`EXTERNAL CORRECTION REQUIRED — IMPLEMENTATION NOT AUTHORIZED`

### 2026-11-26 Authorized Correction Attempt Result

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P0 | GA4 property and stream identity | VERIFIED | `SKSME Alumcasting`, property `527174041`, stream `G-B48RJMJ2W6` |
| P0 | Rule A | VERIFIED / UNCHANGED | `generate_lead`, page-view plus `thank-you`, public tag `19`, priority `6` |
| P0 | Rule B | VERIFIED IN LIST / UNCHANGED | Same conditions, public tag `18`, priority `5` |
| P0 | Reversible disable/pause control | NOT AVAILABLE | Detail menu exposes only Copy and Delete |
| P0 | Authorized configuration action | BLOCKED — NO CHANGE | Deletion prohibited by task |
| P1 | AJAX success callback | VERIFIED / UNCHANGED | Source still emits `generate_lead` only after Formspree success |
| P1 | Protected settings | UNCHANGED | No GA4, Google tag, Formspree, source or consent action occurred |
| P1 | Runtime duplicate verification | NOT VERIFIED | No test was authorized or executed |
| P1 | Next required decision | DELETE AUTHORIZATION + ROLLBACK | Obtain separate explicit approval before any destructive retry |

Execution timestamp:

`2026-11-26T08:12:01+08:00`.

Final operation status:

`BLOCKED — NO CONFIGURATION CHANGES MADE`

### 2026-11-26 GA4 Correction Complete

| Priority | Item | Status | Evidence / next action |
|---|---|---|---|
| P0 | Duplicate URL-based GA4 rules | DELETED | Both matching `generate_lead` rules permanently deleted |
| P0 | GA4 custom-event inventory | VERIFIED EMPTY | No custom event remains for the selected stream |
| P0 | Unrelated custom events | VERIFIED NONE EXISTED | No unrelated rule could have been deleted |
| P0 | `generate_lead` key event | VERIFIED PRESENT | Checkbox remains enabled |
| P0 | GA4 destination | VERIFIED | `G-B48RJMJ2W6` remains |
| P0 | Public Google tag | VERIFIED | `GT-M38DWB5D` remains |
| P0 | URL-based event creation in public tag | VERIFIED ABSENT | `__ogt_event_create` count changed from 6 to 0 |
| P0 | `thank-you` condition in public tag | VERIFIED ABSENT | No matching string remains |
| P0 | Other Google tag measurement | VERIFIED PRESENT | Page view, history, scroll, outbound, form, video, download |
| P0 | Google Signals | VERIFIED ENABLED | No change |
| P0 | Automatic user data | VERIFIED ENABLED | No change |
| P1 | Consent-specific settings | NOT INDEPENDENTLY VERIFIED | No consent UI was changed |
| P1 | Ajax success callback | VERIFIED UNCHANGED | `formspree.html` line 100 |
| P1 | Runtime one-lead behavior | NOT VERIFIED | Requires separately authorized RFQ test |
| P1 | Formspree Free email includes `rfq_id` | NOT VERIFIED | No test receipt |
| P1 | `_subject` Subject behavior | NOT VERIFIED | Use documented `subject` after approval |
| P1 | Source `rfq_id` implementation | READY BUT NOT AUTHORIZED | Specification complete |
| P1 | Synthetic RFQ/GA4 correlation test | NOT AUTHORIZED | Separate approval required |
| P2 | GA4 custom dimension for `rfq_id` | NOT REQUIRED | Avoid high-cardinality identifier |

Final configuration status:

`TWO RULES DELETED — CONFIGURATION AND RECOVERY EVIDENCE VERIFIED`

Evidence folder:

`SEO-AUDIT/AUTONOMOUS/EVIDENCE/2026-11-26-RFQ-GA4-DUPLICATE-RULES/`

### RFQ End-to-End Verification Result

Reference:
`SEO-AUDIT/RFQ-END-TO-END-VERIFICATION-REPORT.md`.

| Check | Status | Evidence |
|---|---|---|
| GA4 Page View triggered | PASS | One browser request with `tid=G-B48RJMJ2W6` and `en=page_view` |
| GA4 Page View transmitted | PASS | Runtime collect request contained expected ID, event and test URL/title |
| GA4 Page View received | PASS | Administrator Realtime summary: 3-page_view |
| Duplicate GA4 page view on first load | PASS — none observed | One GA4 page-view request |
| GA4 `generate_lead` configured | PASS | Client JS plus public tags 18/19 |
| GA4 `generate_lead` received | PASS | Administrator Realtime summary: one generate_lead |
| Google Ads conversion | NOT REQUIRED — BUSINESS DECISION | Campaigns not planned |
| Ads remarketing/page view | PASS — request observed | Ads config, remarketing and CCM page-view resources observed |
| Enhanced Conversions | NOT REQUIRED — BUSINESS DECISION | Existing related configuration remains under privacy review |
| PII URL parameters | PASS — none observed on page load | No email/phone/address/hash patterns in runtime tracking URLs |
| Consent behavior | NOT VERIFIED | No site-level consent code; consent values require Admin interpretation |
| Formspree test RFQ email | PASS | Business confirmed receipt; no matching ID/timestamp |
| Deployed Staging isolation | NOT EXECUTED | No independent Staging deployment |

### GA4 Realtime Receipt Update

| Item | Status | Management-supplied evidence |
|---|---|---|
| GA4 Page View received | PASS | Realtime `page_view`: 3; Homepage 2; Contact 1 |
| GA4 `generate_lead` received | PASS | Realtime count: 1 |
| GA4 `form_start` received | PASS | Realtime count: 1 |
| Other Realtime events | OBSERVED | `scroll` 2, `session_start` 2, `first_visit` 1 |
| Formspree test email | PASS | Business confirms the email arrived |
| Event-to-submission correlation | NOT VERIFIED | No timestamp, Formspree receipt ID or GA4 event ID |
| Duplicate `generate_lead` test | INCOMPLETE | Aggregate count cannot prove uniqueness |

### Ads and Enhanced Conversions Scope Retirement

- Google Ads Conversion implementation: `NOT REQUIRED — BUSINESS DECISION`.
- Enhanced Conversions implementation: `NOT REQUIRED — BUSINESS DECISION`.
- Ads conversion diagnostics: `DEFERRED / NOT REQUIRED`.
- Google Ads campaign measurement: `OUT OF SCOPE`.
- Existing Ads loader, remarketing, CCM and 1P user-list resources: `ACTIVE / READ-ONLY REVIEW`.
- Recommendation:
  `SEO-AUDIT/GOOGLE-ADS-RESOURCE-DECOMMISSION-RECOMMENDATION.md`.

## Priority Rules

1. P0: indexing, canonical, accessibility, or false high-risk enterprise claims.
2. P1: high-impression commercial pages with weak CTR, ranking, trust, or RFQ path.
3. P2: metadata, internal links, image Alt, FAQ, content depth, conversion copy.
4. P3: lower-traffic content and non-critical experience improvements.

## P0

| Task | Status | Evidence | Next action |
|---|---|---|---|
| Canonical and production robots verification | DONE | Production 55/55 sitemap pages have self-canonicals; robots has one production Sitemap declaration. | Preserve; recheck after each build. |
| Staging alias noindex | DONE | 31/31 staging aliases now emit `noindex, nofollow`; production alias output is unchanged. | Preserve. |
| Legacy redirect implementation | BLOCKED | 31 aliases are currently HTTP-200 soft redirects; current internal crawler has 0 missing targets and no reproducible current 404 list. | Approve 301 implementation in `BATCH-2-LEGACY-URL-REVIEW.md`; remediate high-risk targets first. |
| English-only index conflict | HOLD / APPROVAL REQUIRED | `/ko/` is indexed; `/ko/aluminum-die-casting/` is unknown; `/ko/contact/` is discovered-not-indexed. All three remain in the sitemap. | Approve 301 or noindex/sitemap-exclusion fallback; no robots.txt-only solution. |
| Remaining unverified enterprise claims | IN PROGRESS / HOLD | 5000T machine evidence is partial and the business topic is retained; ownership, site, operating status and aluminum production scope remain pending. Homepage templates still hard-code other unsupported claims. | Continue safe content cleanup and collect the 5000T asset package. |

## P1

| Task | Status | GSC evidence | Scope |
|---|---|---|---|
| Automotive page safe content remediation | IMPLEMENTED / VERIFIED | 2026-10-24 to 2026-11-20: 1,507 impressions, 0 clicks, position 48.12. | Remove unsupported claims, improve commercial clarity and RFQ requirements; preserve URL and conversion path. |
| A380 page remediation | IMPLEMENTED / VERIFIED | 176 impressions, 0 clicks, position 12.68. Queries at positions 2–8 include `a380 aluminum die casting`, `a380 aluminum die castings`, and `a380 material`. | Unsupported CNC, IATF, zero-porosity, volume, cost-saving, and tolerance claims removed; metadata, FAQ, Alt, and RFQ copy updated. |
| ADC12 page remediation | IMPLEMENTED / VERIFIED | 316 impressions, 0 clicks, position 16.67. `adc12 aluminum die casting` at 3.0, `adc12 cnc machining` at 2.1. | Unsupported Tier-1, IATF, 100% quality, precision, vacuum, MAGMA, and response-time claims replaced with conservative alloy, DFM, CNC, and project-inspection content. |
| Motor housing page remediation | IMPLEMENTED / VERIFIED | 686 impressions, 0 clicks, position 22.03; previous window had 2,360 impressions. | Removed unsupported equipment, CMM, X-ray, leak, IATF, ISO, NDA, and response-time claims; rebuilt around motor-housing engineering and RFQ inputs. |
| Aluminum die casting hub review | VERIFIED | 2026-10-26 to 2026-11-22: 590 impressions, 0 clicks, position 59.33. | Metadata, content, FAQ/schema and unsupported claims remediated locally; monitor recrawl and query mix. |
| Magnesium page remediation | VERIFIED | 175 impressions, 2 clicks, position 35.09. | Metadata, alloy guidance, process, inspection, FAQ/schema and unsupported claims remediated locally. |
| Precision CNC machining page review | VERIFIED | 324 impressions, 0 clicks, position 51.23; 33 query rows focus on cast-and-machined aluminum components. | Metadata, DFM/fixture planning, tolerance review, FAQ/schema and unsupported claims remediated locally. |
| A356 content and claim review | VERIFIED | Material cluster includes `a356 aluminum` (44 impressions) and `a356 t6 heat treatment process` (position 4.5). | Three A356 pages remediated for factual porosity, semi-solid, heat-treatment and alloy-selection intent. |
| Medical component pages | VERIFIED | Medical CNC: 135 impressions, 0 clicks, position 52.43. Medical die casting: 39 impressions, 0 clicks, position 27.92. | Regulatory boundary, claims, FAQ/schema and RFQ content remediated locally. |
| Manufacturing Capabilities vs Services | VERIFIED | 74 and 102 impressions respectively; both at 0 clicks. | Capabilities now covers process/qualification scope; Services covers RFQ/DFM/project workflow. Keep separate URLs. |
| IATF automotive CNC page | VERIFIED | 2 impressions, position 9.00. | Removed certified/Tier-1, 400+ CNC, ±0.01 mm, capacity and automatic PPAP claims; retained supplier-qualification intent. |
| 5000T large die-casting page | VERIFIED | 1 impression, position 3.00; page retained and indexed. | Preserved all 5000T/large-casting keywords and qualified equipment ownership/availability instead of deleting the topic. |
| Vacuum-assisted HPDC comparison | VERIFIED | 43 impressions, 0 clicks, position 15.98. | Removed zero-porosity/zero-leak guarantees and clarified inspection methods and acceptance limits. |
| Explosion-proof enclosure casting page | DONE / VERIFIED | 462 impressions, 1 click, position 13.04; 9 visible query rows include enclosure, sand-casting and oil-and-gas intent. | Removed 20-year-author, pore-free, 5000T, X-ray coverage, certification and fixed-response claims; rebuilt around casting route, flamepath machining, testing and certification boundaries. |
| Gravity die casting manufacturer page | DONE / VERIFIED | 415 impressions, 0 clicks, position 28.21; 25 visible query rows include manufacturer, foundry, service and production intent. | Removed unverified experience, in-house, equipment, tolerance, documentation and absolute-quality claims; rebuilt around process selection, alloy, T6, tooling, machining and project evidence. |
| Zamak 3 vs Zamak 5 selection page | DONE / VERIFIED | 621 impressions, 3 clicks, position 10.63; material comparison intent with strong current ranking. | Replaced unsourced numeric table and unsupported certification/quality/response claims with practical selection and RFQ guidance. |
| Minimum draft-angle DFM page | DONE / VERIFIED | 225 impressions, 2 clicks, position 22.16. | Removed universal minimums and absolute claims; rebuilt around release direction, feature depth, texture, tooling and validation. |
| Die casting tooling page | DONE / VERIFIED | 164 impressions, 0 clicks, position 42.65; procurement/tooling intent. | Rebuilt around mold procurement, quotation inputs, trials, maintenance and supplier scope. |
| Thin-wall die casting tooling page | DONE / VERIFIED | 50 impressions, 0 clicks, position 32.00; thin-wall feasibility intent. | Removed universal wall and equipment claims; rebuilt around flow length, geometry, alloy, draft, thermal balance, ejection and validation. |
| EV powertrain casting page | DONE / VERIFIED | 78 impressions, 0 clicks, position 49.45. | Retained EV and 5000T project review; removed unsupported certification, equipment, leak, tolerance and production claims. |

## P2

| Task | Status | Evidence / opportunity | Action |
|---|---|---|
| Homepage public-template remediation | BLOCKED / APPROVAL REQUIRED | 6,663 impressions, 5 clicks, position 37.01; core metrics and certification claims are hard-coded in protected templates. | Await protected-template approval. |
| High-tolerance automotive CNC page remediation | DONE / VERIFIED | Rebuilt around feature-level tolerance, casting/machining interaction, fixture planning and project-specific evidence; unsupported CNC count, tolerance, Cpk, Tier-1 and capacity claims were removed. | Monitor recrawl. |
| Automotive CNC equipment list remediation | DONE / VERIFIED | Rebuilt as an equipment and capacity verification guide; unsupported CNC count, facility, tolerance, Cpk and certificate claims were removed; URL and RFQ path were preserved. | Monitor recrawl. |
| Homepage public-template patch review | NEXT / APPROVAL REQUIRED | Replacement matrix is ready in `BATCH-2-HOMEPAGE-FACT-MATRIX.md`. | Obtain approval before editing protected templates. |
| ISO 14001 HPDC supplier-evaluation page | DONE / VERIFIED | 44 impressions, 0 clicks, position 2.82; certificate and quality claims were conflated. | Rebuilt around certificate verification, site-specific environmental controls and quality-evidence boundaries. |
| HPDC process and quality-control page | DONE / VERIFIED | 100 impressions, 0 clicks, position 63.23; zero-defect, fixed-tolerance, 100% test and certification claims. | Rebuilt around process controls, defect causes, inspection limits, pressure/leak methods and project acceptance. |
| About AlumCasting page | DONE / VERIFIED | 524 impressions, 1 click, position 31.88; 91 visible China manufacturer/supplier query rows. | Rebuilt as an engineering and supplier-evidence introduction; removed unapproved legal, facility, certificate, image and capacity claims. |
| EV battery housing page | DONE / VERIFIED | 101 impressions, 0 clicks, position 31.94; high-risk 5000T/400+ CNC/100% leak/certification claims. | Rebuilt with evidence-safe housing DFM, 5000T project qualification, CNC, inspection and RFQ content; URL preserved. |
| Thixocasting vs Rheocasting page | DONE / VERIFIED | 59 impressions, position 9.68; process-comparison technical intent. | Removed universal winner, zero-porosity, automatic T6 and fixed-cost claims; added fair process-selection guidance and RFQ inputs. |
| Sand casting services page | DONE / VERIFIED | 46 impressions, position 44.48; process/diagram and prototype intent. | Removed author, zero-porosity, absolute finish and universal small-batch claims; rebuilt around patterns, variants, machining and inspection. |
| Metallurgy and alloy pages | DONE / VERIFIED | A383, AZ91D, AM60B, zinc, aluminum-to-magnesium and LPDC/HPDC pages. | Removed unsupported material, customer, equipment, certificate, quality and performance claims; added project-specific selection and validation guidance. |
| EV cooling and finishing pages | DONE / VERIFIED | Cooling plates: 5 impressions, position 25.80; finishing: 36 impressions, position 36.28. | Clarified pressure/leak validation, masking, substrate-specific finishing and project test requirements. |
| Cold chamber and semi-solid pages | DONE / VERIFIED | Cold chamber: position 4.00; semi-solid: position 5.67. | Removed fleet, IATF, zero-defect, forge-density and fixed-response claims; retained commercial process intent. |
| Remaining quality-control and certification pages | NEXT | Additional routes still carry zero-defect, 100% inspection, Cpk, CNC-count or certificate claims. | Select by GSC exposure and fact-risk severity. |
| Internal-link and image-Alt audit after content batches | RECURRING | Rebuild and validate each batch. | Keep generated output outside the repository. |

## Batch 8 Growth Work

| Task | Status | Evidence / opportunity | Action |
|---|---|---|---|
| Contact RFQ conversion hub | DONE / VERIFIED | 61 impressions, 0 clicks, position 39.39 | Removed unverified age claim; added RFQ inputs, review scope, material/process links and four FAQs. |
| 5000T wall/projected-area page | DONE / VERIFIED | 5 impressions, 1 click, position 6.20 | Preserved 5000T topic; removed fixed limits, author and equipment claims; added project calculation and qualification guidance. |
| Aluminum-to-magnesium intent split | DONE / VERIFIED | Commercial service page had 2 impressions; technical guide had 81 impressions, position 16.12 | Commercial page now covers conversion service and RFQ; guide retains selection and redesign intent. |
| Stainless steel machining risk cleanup | DONE / VERIFIED | Focused residual-claim scan | Removed fabricated experience, sub-micron, medical/aerospace and IATF claims. |
| ISO 14001 CTR optimization | DONE / VERIFIED | Exact query: 41 impressions, position 2.76 | Title now targets manufacturer and ISO 14001 review intent. |
| Oil-and-gas enclosure CTR optimization | DONE / VERIFIED | 447 impressions, 1 click, position 12.52; visible oil-and-gas/EPE queries | Title and description now include oil and gas, enclosure casting and certification boundary. |
| Motor Housing CTR optimization | DONE / VERIFIED | 611 impressions, 0 clicks, position 21.06 | Title targets manufacturer; added tooling, CNC and inspection links. |
| Gravity die cast internal linking | DONE / VERIFIED | 411 impressions, 0 clicks, position 27.72 | Added LPDC/HPDC comparison, finishing and contact links. |
| Content cluster map | DONE / VERIFIED | Current English content and GSC inventory | Created `SEO-AUDIT/CONTENT-CLUSTER-MAP.md`; no new URL or redirect. |
| Aerospace and industrial landing pages | HOLD | Aerospace queries exist, but evidence and program scope are absent. | Improve existing pages; do not create a new claim-led page yet. |
| Live/local legacy URL reconciliation | HOLD / APPROVAL REQUIRED | GSC shows URLs with no matching local content source. | Audit deployed routes before any deployment, redirect or deletion. |

## Batch 9 Search-Intent Work

| Task | Status | Evidence | Action |
|---|---|---|---|
| A380 property/service intent | DONE / VERIFIED | 141 impressions, position 16.30; property queries at positions 12–21 | Added documented property-verification guidance and aligned title/description. |
| Zamak property and aluminum comparison | DONE / VERIFIED | 574 impressions, position 11.04; `zamak 3` 44 impressions at 9.18 and `zamak 5 material properties` 20 at 8.80 | Added property-source and zinc-versus-aluminum sections without numeric claims. |
| Motor Housing manufacturer intent | DONE / VERIFIED | 611 impressions, position 21.06; manufacturer/EV queries in positions 5–32 | Added drawing-specific manufacturer qualification and aligned title to EV intent. |
| EPE enclosure intent | DONE / VERIFIED | 447 impressions, position 12.52; EPE query at 7.56 | Added EPE and protection-concept clarification. |
| Gravity alloy-supplier intent | DONE / VERIFIED | 411 impressions, position 27.72; alloy-supplier query at 26.31 | Aligned title to manufacturer and aluminum-alloy GDC. |
| A356 material/T6 intent | DONE / VERIFIED | 155 impressions, position 63.39; T6 query at 4.50 | Added material-record verification and process-scope differentiation. |
| A380 sourced numeric property table | HOLD | Property queries seek composition/density/mechanical values, but no approved primary data pack is in the repo | Obtain an approved material source before publishing numbers. |
| A356 investment-casting intent | KEEP SEPARATE | GSC has 24 impressions for A356 investment casting | Page now states its die-casting/semi-solid scope; do not imply investment-casting production. |

## Batch 10 Measurement Work

| Task | Status | Evidence | Action |
|---|---|---|---|
| Batch 9 content quality review | DONE / VERIFIED | Six content pages plus fifteen tracking files | No corrective rewrite required |
| Growth priority model | DONE / VERIFIED | Current GSC page/query data and transparent scoring | Created `SEO-AUDIT/GROWTH-PRIORITY-MATRIX.md` |
| RFQ attribution architecture audit | DONE / VERIFIED | Google tag, GA4 config, Formspree code and generated HTML | Created `SEO-AUDIT/RFQ-ATTRIBUTION-PROPOSAL.md` |
| CTA and form-start measurement | APPROVAL REQUIRED | No dedicated events exist | Implement only through an approved shared-template patch |
| Source-page capture with RFQ | APPROVAL REQUIRED | No hidden source/landing fields exist | Add normalized non-PII fields after approval |
| GA4 event validation | NOT CONNECTED | Public tag exists; no read-only GA4 export | Connect read-only GA4 data before reporting conversions |
| Formspree submission reconciliation | NOT CONNECTED | No Formspree submission export/API available | Connect a read-only export or approved internal register |
| CRM qualification/outcome | NOT CONNECTED | No CRM data | Define human review fields and sales stages |
| AI citation and AI-RFQ measurement | NOT CONNECTED | No platform or AI-referral data | Keep as design only |
| Numerical A380/Zamak property table | HOLD | No approved primary source in the repository | Do not publish unsourced values |

## Batch 11 Evidence and Tracking Readiness

| Task | Status | Evidence | Next action |
|---|---|---|---|
| Claim-to-evidence mapping | DONE / VERIFIED INTERNAL | 39 evidence files; no explicit public authorization | Use public claims only after written approval |
| Evidence-driven commercial page changes | HOLD | No eligible public claim | Keep existing conservative content |
| RFQ tracking implementation package | DONE / APPROVAL REQUIRED | Current Google tag, Formspree and generated form audited | Obtain privacy/analytics/CRM/release approval |
| Deployment blocker register | DONE / VERIFIED | Protected templates, live/GSC URL checks and evidence gaps | Resolve approvals before any deployment |
| IATF/ISO public wording | HOLD | Holder is Kingship; current status/public use pending | Obtain issuer verification and brand authorization |
| 5000T ownership/operation | HOLD | No canonical asset/state package | Obtain owner, site, installation and operating evidence |
| Inspection scope | HOLD | Equipment classes only | Obtain calibration, uncertainty and coverage records |
| Facility/history | HOLD | Conflicting internal statements | Obtain registry, lease/title, area and HR evidence |
| Image publication | HOLD | 30/30 public use pending | Complete rights register |
| RFQ outcome reporting | NOT CONNECTED | No GA4/Formspree/CRM export | Connect data after approved implementation |

## Batch 12 Release Readiness

| Task | Status | Finding | Decision |
|---|---|---|---|
| Sitemap inventory reconciliation | DONE / VALIDATED | Local and production both list 55 paths; production content is older | Do not deploy mixed worktree |
| Alias inventory | DONE / VALIDATED | 31 meta-refresh aliases; 30 GSC redirects, 1 crawled-not-indexed | Preserve; 301 policy requires approval |
| Korean routes | DONE / BUSINESS DECISION | `/ko/` indexed; child routes unknown/discovered | No deletion or noindex |
| `casting-industry-applications` | DONE / VALIDATED | HTTP 200 meta refresh; GSC page with redirect | Keep temporarily; 301 requires approval |
| Legacy 404s | DONE / NEEDS BUSINESS DECISION | Three known URLs return 404 | Do not recreate or loosely redirect |
| Robots comparison | DONE / VALIDATED | Live lacks Sitemap; local Production includes one | Deploy only with approved release and verify |
| Homepage/template release | APPROVAL REQUIRED | Protected claims remain | Stage 5 only |
| RFQ tracking release | APPROVAL REQUIRED | Privacy/analytics/CRM dependencies | Stage 4 only |
| Business evidence | HOLD | P0/P1 approvals pending | No new enterprise claims |
| Content-only release | DONE / DEPLOYED | Release A+B deployed at `11cf753` | Monitor and continue held items |

## Release A+B Candidate

| Task | Status | Evidence | Next action |
|---|---|---|---|
| Release A content commit | DONE / DEPLOYED | Commit `a7058f4`; 47 English content files | Preserve commit |
| Release B technical commit | DONE / DEPLOYED | Commit `11cf753`; six approved technical files | Preserve commit |
| Combined production build | DONE / VALIDATED | 0 missing links, 0 missing images, 31/31 aliases guarded | Monitor |
| Remote push | DONE | `11cf753` pushed to production main | Preserve |
| CI / merge / deploy | DONE / VERIFIED | GitHub Actions run `45593154825` succeeded | Preserve deployment record |
| Production verification | PARTIAL / VERIFIED | Key content, robots, Sitemap and aliases verified | Monitor recrawl |

## Business Evidence Queue

1. AlumCasting/KingShip legal-brand citation authorization.
2. Current certificate holder, scope, validity, and permitted public wording.
3. Die-casting and CNC asset ledger with ownership, availability, and capacity.
4. X-ray, CMM, and leak-test calibration, coverage, methods, and records.
5. Feature-level tolerance capability study.
6. PPM, lot yield, and inspection acceptance data.
7. Approved RFQ SLA and achieved T0 records.
8. Verified legal source for company age and factory area.

Do not block unrelated pages while these items remain open.

## 2026-11-26 RFQ Unique-ID Production Completion

Final status:

`PRODUCTION RELEASE COMPLETE — SOME MEASUREMENT GAPS REMAIN`

| Workstream | Status | Evidence | Remaining action |
|---|---|---|---|
| RFQ unique-ID source | DONE / VERIFIED | Commit `209ec0f`; two source files | Preserve |
| Production deployment | DONE / VERIFIED | Merge `9f141e1`; Actions `45760487830`; Pages `8077162596` | Monitor |
| Formspree custom-field receipt | DONE / VERIFIED | Submission `174445634` stores `rfq_id` and dynamic `subject` | Preserve |
| Notification inbox receipt | HOLD / NOT VERIFIED | Plugin reports `processed`; inbox not inspected | Authorized form owner/mailbox check |
| Email body `rfq_id` rendering | HOLD / NOT VERIFIED | Default body not inspected | Redacted rendered example if a future approved test occurs |
| GA4 `generate_lead` receipt | DONE / VERIFIED | One Realtime event and one collect request | Monitor |
| GA4 non-PII boundary | DONE / VERIFIED | No personal fields observed in event parameters | Monitor |
| Second production RFQ test | NOT REQUIRED | One controlled submission accepted | Do not repeat without a specific defect |
| Production smoke | DONE / VERIFIED | 55/55 Sitemap pages HTTP 200; 0 Canonical errors; 0 noindex | Monitor |
| Rollback package | READY / NOT EXECUTED | Isolated revert path and trigger conditions recorded | Execute only on approved trigger |

The release used merge commit `9f141e1` after a normal fast-forward push was
rejected because remote `main` had already advanced to the expected merge
containing `209ec0f`. No force push, duplicate branch or duplicate source commit
was created.

Controlled test:

| Item | Value |
|---|---|
| RFQ ID | `RFQ-20261126-BF29E3AD3BA6` |
| Formspree submission | `174445634` |
| GA4 Realtime lead count | 1 |
| GA4 collect request count | 1 |
| Notification email inbox receipt | `NOT VERIFIED` |

Next safe operations:

1. Monitor production RFQ submissions without initiating synthetic tests.
2. Obtain a redacted notification example from the form owner if inbox evidence
   becomes available.
3. Use Formspree submission and GA4 event timestamps for manual reconciliation;
   do not infer a qualified sales RFQ from the generated event.
4. Keep `rfq_id` out of GA4 custom dimensions unless a separate reporting
   requirement justifies the high-cardinality dimension.
5. Do not implement CRM, Consent Mode, CMP, Google Signals or Enhanced
   Conversions changes under this release record.

Machine-readable evidence:

`SEO-AUDIT/AUTONOMOUS/EVIDENCE/2026-11-26-RFQ-UNIQUE-ID-IMPLEMENTATION/post-deployment-smoke.json`
