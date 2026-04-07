# Anti-Patterns

Use this file to reject code that is technically valid but unlikely to be adopted by the team.

## Structural Anti-Patterns

- Do not invent a new program structure when a golden example already matches.
- Do not turn a simple one-pass program into a layered design with extra procedures, service wrappers, or helper modules.
- Do not hide business validation inside generic utility routines.
- Do not replace a visible mainline with an abstract control flow that is harder to diff against existing members.

## Style Anti-Patterns

- Do not introduce a new naming convention inside the same member.
- Do not use long descriptive variable names if the sample set uses short business-facing names.
- Do not switch between team-style working variables and a modern framework-like naming scheme.
- Do not add decorative comments, tutorial comments, or narration that existing team code would not contain.

## Control-Flow Anti-Patterns

- Do not replace a `chain`-based lookup with a loop unless the requirement demands sequential processing.
- Do not replace a `setll/read/dow/read` batch loop with random-access logic unless the requirement demands keyed access.
- Do not remove visible flags, counters, or rejection reasons just because they appear redundant.
- Do not collapse explicit not-found or invalid-status branches into hidden defaults.

## Technology Anti-Patterns

- Do not modernize syntax only because it is newer.
- Do not introduce SQL, service programs, data structures, or APIs that were not requested and are not present in the sample set.
- Do not add reusable framework-style helpers unless the chosen sample already depends on them.

## Validation Anti-Patterns

- Do not present guessed objects as verified facts.
- Do not omit `unverified` markers when DDS, PF/LF, field lists, or status codes are missing.
- Do not claim compile safety when runtime or compile settings are unknown.

## Use In Review

When reviewing generated code, treat any anti-pattern as an adoption risk even if the code is otherwise correct.
