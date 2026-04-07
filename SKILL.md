---
name: as400-sdd-skill
description: Strongly-templated IBM i / AS400 code generation skill for creating new RPG or RPGLE programs that match an existing team style. Use when Codex needs to generate a first draft for new AS400 development, select a project skeleton, follow real RPGLE examples, write a short spec before code, validate PF/LF and field usage, or review output for adoption risk, style fit, and hallucination risk.
---

# AS400 SDD

Use this skill to maximize first-draft adoption for new IBM i programs. Optimize for code that looks like the team already wrote it, not for the most modern or abstract RPG style.

Keep `spec -> generate -> self-check`, but treat `generate` as the main path. Use the lightest spec that still prevents guessing.

## Primary Goal

Produce a new program draft that requires minimal style cleanup before a team accepts it.

Prioritize in this order:

1. Match the closest approved team pattern.
2. Preserve familiar naming, structure, and error handling.
3. Avoid hallucinated files, fields, indicators, and helper abstractions.
4. Only then optimize for elegance or modernization.

## Required Workflow For New Program Generation

Follow this order every time:

1. Extract the requirement.
2. Select the closest template using [references/generation-playbook.md](./references/generation-playbook.md).
3. Load the matching example from [references/golden-examples.md](./references/golden-examples.md).
4. Confirm the minimum required context using [references/spec-template.md](./references/spec-template.md).
5. Output a short spec summary.
6. Generate code by reusing the chosen example's structure.
7. Run a self-check with [references/review-checklist.md](./references/review-checklist.md).

Do not skip template selection. Do not invent a fresh architecture when a close sample exists.

## Minimum Input Contract

Before generating a new program, confirm or infer these fields:

- program type
- input and output objects
- key business rules
- file or field sources
- target format: `fixed`, `free`, or `mixed`
- whether the user wants alignment to a known sample

If one of these is missing, ask only the blocking question. Do not ask generic discovery questions.

If file or field definitions are still missing:

- mark them as `unverified`
- say validation is partial
- avoid inventing uncertain objects
- keep the code conservative and easy to patch

## Style Selection Rules

Use real examples as the primary style source. Read [references/style-rules.md](./references/style-rules.md) after selecting the template.

- Prefer the closest sample over a cleaner but unfamiliar structure.
- Reuse the sample's declaration layout, naming rhythm, and error-handling flow.
- Copy structure, not business literals. Replace only the task-specific logic.
- If the request conflicts with the sample, preserve the sample's shape and adapt the logic inside it.
- Preserve the chosen template's control-flow cadence. Do not change a `chain -> validate -> return` shape into a loop, and do not change a `setll/read/dow/read` batch shape into keyed random access unless the request explicitly requires it.
- Preserve the chosen template's visible state variables such as `wsFound`, `wsCanUpdate`, counters, and reason fields when they are part of the sample's readability pattern.

## Output Contract For New Program Generation

Use this exact section order:

### Assumptions

- List only assumptions that affect generated code.
- Mark each uncertain file, field, status code, or indicator as `unverified`.

### Selected Template

- Name the chosen pattern.
- State why it is the closest fit.
- Mention which example or style rule drove the structure.

### Spec Summary

- Summarize the program purpose.
- List inputs, outputs, files, and core business rules.
- Call out open questions if partial information remains.

### Generated Code

- Produce code in the requested format.
- Reuse the selected template's shape.
- Do not add optional helpers, frameworks, or extra procedures unless the chosen sample already uses them.

### Self-Check

- Confirm style fit.
- Confirm spec alignment.
- Confirm verified versus unverified objects.
- Flag any place the user should validate against real DDS, PF/LF, or compile settings.
- Confirm the generated control flow still matches the selected template's invariants.

## Review Rules

Use [references/review-checklist.md](./references/review-checklist.md) for every generated result.
Use [references/anti-patterns.md](./references/anti-patterns.md) to reject technically-correct but low-adoption shapes.

- Treat low adoption risk as a first-class requirement.
- Flag code that is technically valid but stylistically foreign.
- Prefer conservative edits over broad refactors when closing review gaps.

## Prohibited Behaviors

- Do not introduce helper patterns the team samples do not use.
- Do not split a simple program into many procedures unless the sample does so.
- Do not modernize syntax just because it is newer.
- Do not invent fields, record formats, APIs, or indicator conventions.
- Do not switch naming systems mid-file.
- Do not drop counters, status flags, or rejection-reason variables when the selected sample uses them to make the flow reviewable.

When in doubt, choose the more conservative and more familiar structure.

## Output Example

Use [references/output-example.md](./references/output-example.md) as the default response shape for new program generation.

- Match the section order exactly.
- Keep the same level of detail unless the user explicitly asks for more or less.
- Prefer this stable output shape over ad hoc prose.

## Reference Usage

- Read [references/generation-playbook.md](./references/generation-playbook.md) first for new program generation.
- Read [references/golden-examples.md](./references/golden-examples.md) for concrete skeletons.
- Read [references/style-rules.md](./references/style-rules.md) to align naming and layout.
- Read [references/anti-patterns.md](./references/anti-patterns.md) before finalizing generated code.
- Read [references/forward-test-notes.md](./references/forward-test-notes.md) for known failure modes and template invariants.
- Read [references/spec-template.md](./references/spec-template.md) to confirm the minimum input contract.
- Read [references/review-checklist.md](./references/review-checklist.md) before final output.
- Read [references/output-example.md](./references/output-example.md) when producing the final response structure.
- Read [references/format-guide.md](./references/format-guide.md) only when the target format is unclear.

Load only the references needed for the active task.
