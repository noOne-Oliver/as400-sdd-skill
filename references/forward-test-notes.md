# Forward Test Notes

These notes summarize three manual forward tests against the current skill. Use them to prevent common adoption regressions.

## Scenario 1: Query Reader

Request shape:

- New lookup program
- Read one object by key
- Return a few fields plus a status
- No file update

Expected template:

- `Main Program Query Reader`

Observed drift risk:

- The generator may return fields without an explicit not-found fallback.
- The generator may remove the visible found flag because it looks redundant.

Tightening rule:

- Keep a visible found/not-found branch and explicit fallback behavior even if the logic seems simple.

## Scenario 2: Validated Update

Request shape:

- New update program
- Chain to one object
- Apply several validations
- Update only if all conditions pass

Expected template:

- `Validation Before Update`

Observed drift risk:

- The generator may inline all validation without keeping a readable rejection reason.
- The generator may split validation into extra procedures even when the sample is flat.

Tightening rule:

- Keep a flat validation ladder with a can-update flag and a rejection-reason variable when business rejection matters.

## Scenario 3: Batch Status Update

Request shape:

- New batch job
- Scan many records
- Check conditions
- Update a flag or status
- Report counts

Expected template:

- `Batch Status Updater`

Observed drift risk:

- The generator may omit explicit counters.
- The generator may replace the classic sequential loop with a different structure that feels less native to the team.

Tightening rule:

- Keep explicit read, update, and error counters plus the visible `setll/read/dow/read` loop when the request is batch-like.
