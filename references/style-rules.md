# Style Rules

Use these rules after selecting a golden example. They are derived from the example set and are meant to raise adoption, not to maximize abstraction.

## Naming

- Prefer `ws` prefix for working variables.
- Keep variable names short, business-facing, and stable across the member.
- Reuse file-based field naming where possible instead of inventing alternate business labels.
- Use indicator variables only when the sample pattern uses them for readability.

## Layout

- Keep `ctl-opt` first.
- Group file declarations before parameter and working-storage declarations.
- Keep the declaration block compact.
- Keep the main business flow visually linear.

## Procedures And Structure

- Default to a single mainline for simple new programs.
- Introduce extra procedures only when the sample type already uses them or the business logic is clearly multi-stage.
- Keep validation close to the update or write path.

## Error Handling

- Prefer explicit visible branches over hidden helper wrappers.
- Keep not-found, invalid-status, and blank-input paths close to the main logic.
- Use counters and flags directly in batch-style programs.

## Modern Syntax Discipline

- Prefer free-format RPGLE for new programs unless the user or environment requires otherwise.
- Do not add modern syntax that materially changes the team's usual shape.
- Do not replace a simple direct flow with service-style architecture unless the prompt explicitly requires it.

## Comments

- Keep comments sparse.
- Comment business intent only when the structure alone is not obvious.
- Avoid decorative or tutorial comments.
