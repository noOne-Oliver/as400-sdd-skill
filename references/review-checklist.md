# AS400 SDD Review Checklist

Use this checklist for generated code and edits to existing members. Optimize for team adoption, not just technical correctness.

## Adoption Fit

- Confirm the selected template matches the requested program type.
- Confirm the program header, declaration block, and main flow resemble the closest golden example.
- Confirm variable names, prefixes, and routine names match the team pattern.
- Flag any structure that is technically valid but visually foreign to the sample set.
- Confirm the chosen template's control-flow invariant was preserved.
- Confirm expected readability variables such as found flags, counters, and rejection reasons were not removed.

## Spec Alignment

- Confirm each branch and file operation exists for a stated business rule.
- Confirm parameter list, returned values, and called programs match the spec.
- Flag any logic present in code but absent from the spec.

## Field and Object Validation

- Verify every file, record format, field, data structure, and indicator against provided definitions.
- Flag any identifier that appears plausible but cannot be verified.
- Confirm keyed access uses the correct keys and lengths.

## Format Correctness

### Fixed Format

- Check opcode placement and column discipline.
- Check continuation handling and line width limits.
- Check indicator positions and legacy result-field usage.

### Free Format

- Check block structure, declarations, procedure interfaces, and terminators.
- Check `%trim`, `%subst`, `%found`, `%error`, and similar built-ins for semantic correctness.
- Check that free-format syntax does not silently change legacy behavior.

## Runtime Safety

- Check file open/update/write/delete paths for missing status handling.
- Check record locking, commit control, and rollback expectations.
- Check null, blank, zero, and not-found paths.
- Check date, decimal, and character conversions.

## Maintainability

- Preserve established naming and module boundaries unless modernization is the goal.
- Prefer explicit declarations over implicit assumptions.
- Separate modernization improvements from required behavioral fixes.

## Prohibited Pattern Check

- Flag helper abstractions that do not appear in the sample set.
- Flag unnecessary extra procedures in otherwise simple programs.
- Flag modernized syntax choices that were introduced without sample support.
- Flag any guessed field, record format, API, or indicator convention.
- Flag any mixed naming system inside the same member.
- Flag any output that does not follow the standard response shape from `output-example.md`.
- Flag any structure or wording that matches a listed anti-pattern.

## Review Output

Report findings in this order:

1. Incorrect or unverifiable data references
2. Behavioral mismatches with the spec
3. Style or adoption risks relative to the chosen sample
4. Missing error handling or transaction safety
5. Format and maintainability issues
