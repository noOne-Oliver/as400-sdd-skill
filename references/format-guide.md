# Format Guide

Use this guide when the user does not clearly specify fixed, free, or mixed format.

## Prefer Fixed Format When

- Editing an existing legacy member that already uses fixed calculations.
- The shop enforces column-based RPG style.
- The change is small and a large conversion would raise regression risk.

## Prefer Free Format When

- Building a new module, service program, or procedure.
- Converting legacy logic for readability or maintainability.
- Introducing modern declarations, procedures, built-in functions, or embedded SQL.

## Prefer Mixed Format When

- Wrapping legacy calculations with modern interfaces during phased migration.
- The codebase already combines fixed and free sections and consistency matters more than purity.

## Decision Rule

If no constraint is known, default to free-format RPGLE and state that assumption explicitly.
