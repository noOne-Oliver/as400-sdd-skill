# Generation Playbook

Use this playbook before writing code. Pick the nearest template first, then adapt business rules inside that shape.

## Template Selection

### Query Or Lookup Program

Choose `Main Program Query Reader` when the request:

- reads one file or a small set of records
- returns fields or a single status
- does not update data

Template invariants:

- Keep a single `chain`-based lookup flow unless sequential read is explicitly required.
- Keep a visible found/not-found branch.
- Keep default fallback output behavior explicit.

### Batch Processing Program

Choose `Batch Status Updater` when the request:

- scans many records
- applies repeated business checks
- updates a status, flag, or summary field
- needs counts for reads, updates, or errors

Template invariants:

- Keep the `setll -> read -> dow -> read` loop shape.
- Keep explicit read, update, and error counters.
- Keep update logic inline unless the prompt explicitly requires decomposition.

### Validated Update Program

Choose `Validation Before Update` when the request:

- chains to a single object
- checks multiple conditions
- updates only when all validations pass

Template invariants:

- Keep a single-object `chain` flow.
- Keep visible validation branches before the update.
- Keep a flag and optional reason field that explain why update did or did not occur.

### Report Or Extract Program

Choose `Report Preparation Skeleton` when the request:

- reads and filters records
- writes staged output or report records
- is output-focused rather than transaction-focused

Template invariants:

- Keep the read/filter/write cadence visible in the mainline.
- Keep output record population adjacent to `write`.
- Avoid update-style counters unless the request explicitly asks for them.

## Decision Rules

- Prefer the example with the closest control flow, not just the closest file names.
- If two examples are close, choose the simpler one.
- If no example is close, say so explicitly and reuse the least surprising skeleton.

## Adaptation Rules

- Keep declaration order from the chosen example.
- Keep the same style of flags, counters, and validation branches.
- Replace only files, fields, and business literals.
- Avoid adding helper procedures unless the chosen example already suggests them.
- Keep the chosen template's control-flow invariant even when business rules become more complex; add conditions inside the existing skeleton before creating a new shape.

## Required Output Notes

When reporting the chosen template, always state:

- the selected template name
- why it matches the request
- which assumptions remain unverified
