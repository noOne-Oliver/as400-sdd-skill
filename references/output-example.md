# Standard Output Example

Use this example to keep generation output stable. Match this section order and level of detail.

## Example

### Assumptions

- `ORDHDR` is the source PF and key `OHORD#` is verified.
- `OHCUST` and `OHSTAT` are available output fields and are verified.
- Return status `'N'` means record not found. This status code is `unverified` unless the user confirms it.

### Selected Template

- Template: `Main Program Query Reader`
- Why it matches: the request is a new read-only lookup program that reads one object by key and returns a compact set of fields.
- Style anchor: use the query reader sample's single `chain` flow, visible found flag, and explicit not-found fallback.

### Spec Summary

- Purpose: read one order header by order number and return customer number plus approval status.
- Inputs: order number.
- Outputs: customer number, status code.
- Files: `ORDHDR` as input.
- Business rules:
  - chain by order number
  - if found, return customer number and order status
  - if not found, return fallback status
- Open questions:
  - confirm whether not-found status should be `'N'` or another project code

### Generated Code

```rpgle
**FREE

ctl-opt dftactgrp(*no) actgrp(*caller) option(*srcstmt:*nodebugio);

dcl-f ORDHDR keyed usage(*input) extfile('ORDHDR');

dcl-pi *n;
  inOrderNo char(10);
  outCustNo char(10);
  outStatus char(1);
end-pi;

dcl-s wsFound ind inz(*off);

chain inOrderNo ORDHDR;
if %found(ORDHDR);
  wsFound = *on;
  outCustNo = OHCUST;
  outStatus = OHSTAT;
else;
  wsFound = *off;
endif;

if not wsFound;
  outStatus = 'N';
endif;

return;
```

### Self-Check

- Style fit: matches the query reader template with a single `chain` and visible found/not-found path.
- Spec alignment: all generated branches map to stated business rules.
- Verified objects: `ORDHDR`, `OHORD#`, `OHCUST`, `OHSTAT`.
- Unverified objects: fallback status `'N'` until user confirms the project status convention.
- User validation needed: confirm actual DDS field names and not-found status semantics before compile.
