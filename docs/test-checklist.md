# Test checklist — sandbox only

Record date, payment ID (redacted), execution link/screenshot, result, and any issue for each case.

| Test | Expected | Status from observed work |
|---|---|---|
| New Stripe test payment succeeds | Trigger receives `payment_intent.succeeded` | PASS |
| USD amount maps correctly | `10000` Stripe minor units stored as `100.00` | PASS after correction |
| Airtable bulk upsert | Correct payment row created/matched | PASS |
| Airtable search formula | One record, `rec_...` ID | PASS after removing invalid Sort item |
| QuickBooks receipt | One sandbox sales receipt at $100 | PASS (receipt #1039 observed) |
| Airtable final update | `Synced` and internal receipt ID | PASS (ID observed) |
| Already-synced payment | Filter passes zero bundles, no QuickBooks action | PASS on tested record; confirm receipt count unchanged |
| Invalid QuickBooks reference | Gmail alert with actual validation error | PASS |
| Gmail standalone sending | Email send succeeds | PASS |
| Empty customer email | No failure and blank/nullable handling | NOT VERIFIED |
| Missing/empty lookup | Safe stop and alert/manual review | NOT VERIFIED |
| API timeout after receipt created | No blind duplicate retry; reconciliation | NOT VERIFIED; documented limitation |
| Concurrent identical events | At most one receipt | NOT VERIFIED; requires hardening |
| Exported blueprint imports and reconnects | Scenario re-creates in new account | NOT VERIFIED |
| Secrets/screenshots sanitized | No personal or secret data committed | NOT VERIFIED |

## Procedure: controlled error test

Use *sandbox only*. Preserve original QuickBooks settings. Temporarily map an invalid item ID to induce a validation rejection; do not enable automatic retry. Confirm Gmail receives the error and that no sales receipt is created for that failed test ID. Restore `Services (Service)` and save. Mark the test record `Needs Review` / `Failed — Test` as appropriate; do not claim it was synced.

## Before final submission

- [ ] Confirm QuickBooks item restored to `Services (Service)`; normal customer/amount mapping restored
- [ ] Review incomplete executions safely; do not rerun an uncertain accounting request
- [ ] Check previously failed test rows for accurate status; correct historical amount only with clear audit note
- [ ] Verify sanitized screenshots and final blueprint contain no credentials or personal data
- [ ] Attach 60–90-second Loom and recorded run
- [ ] Work on feature branch and open PR for reviewer
