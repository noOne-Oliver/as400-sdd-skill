# AS400 SDD Spec Template

Use this template to build the shortest spec that still supports high-adoption code generation.

## Minimum Input Contract

Do not generate a new program until these fields are confirmed or explicitly marked `unverified`.

- Program type:
- Input objects:
- Output objects:
- Key business rules:
- File or field sources:
- Target format: `fixed` | `free` | `mixed`
- Preferred sample or style anchor:

## Spec Summary

- Feature:
- Objective:
- Request type: `new` | `change` | `bugfix` | `modernization`
- IBM i release:
- Language: `RPG` | `RPGLE` | `SQLRPGLE` | `CLLE` | other

## Data Contract

| Object | Type | Purpose | Key fields | Verified source | Status |
| --- | --- | --- | --- | --- | --- |
| ORDHDR | PF | Example | OHORD#, OHCUST, OHSTAT | DDS or schema path | verified |

Use `unverified` when the source object or field list has not been confirmed.

## Program Shape

- Chosen template:
- Mainline or procedure layout:
- Error handling pattern:
- Indicator usage policy:
- Naming pattern to match:

## Business Rules

1. Main processing flow.
2. Validation rules.
3. Status or state transitions.
4. Error conditions and recovery behavior.
5. Commit/rollback or locking expectations.

## Open Questions

- Missing file definitions:
- Missing field semantics:
- Unknown status codes:
- Unknown compile or runtime constraints:
