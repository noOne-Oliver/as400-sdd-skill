# Golden Examples

Use these anonymized examples to choose structure before generating code. Prefer the closest example over inventing a new layout.

## Example 1: Main Program Query Reader

Best for new read-only programs that load one business object, validate state, and return a compact result.

Why this is recommended:

- Simple linear flow that teams accept quickly.
- Declaration block is short and familiar.
- Error handling stays close to the mainline instead of being abstracted away.

Do not change these invariants:

- Single keyed lookup with one visible found/not-found decision.
- Explicit fallback output behavior when the record is absent.
- No extra procedures or helper wrappers.

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
dcl-s wsStatus char(1) inz(*blanks);

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

## Example 2: Batch Status Updater

Best for new batch-style programs that scan records, apply validation, update a status field, and count outcomes.

Why this is recommended:

- Matches classic IBM i batch layout.
- Keeps counters, validation flags, and update logic visible in one pass.
- Easy for reviewers to diff against existing operational programs.

Do not change these invariants:

- `setll/read/dow/read` loop shape remains visible.
- Read, update, and error counters stay explicit.
- Validation and update happen in the same mainline pass.

```rpgle
**FREE

ctl-opt dftactgrp(*no) actgrp(*caller);

dcl-f INVHDR usage(*update) keyed extfile('INVHDR');

dcl-s wsReadCnt packed(9:0) inz(0);
dcl-s wsUpdCnt packed(9:0) inz(0);
dcl-s wsErrCnt packed(9:0) inz(0);

setll *loval INVHDR;
read INVHDR;
dow not %eof(INVHDR);
  wsReadCnt += 1;

  if IHSTAT = 'A' and IHBAL > 0;
    IHFLAG = 'R';
    update INVHDRR;
    wsUpdCnt += 1;
  else;
    wsErrCnt += 1;
  endif;

  read INVHDR;
enddo;

return;
```

## Example 3: Validation Before Update

Best for update programs that must verify multiple conditions before writing back to the file.

Why this is recommended:

- Keeps validation rules explicit and near the update.
- Avoids over-engineered subprocedures for small business flows.
- Aligns with teams that prefer visible status branching.

Do not change these invariants:

- Single `chain` against the target object.
- Validation branches stay above the update.
- Keep both a can-update flag and a human-readable rejection reason.

```rpgle
**FREE

ctl-opt dftactgrp(*no) actgrp(*caller);

dcl-f CUSHDR usage(*update) keyed extfile('CUSHDR');

dcl-pi *n;
  inCustNo char(10);
  inNewLevel char(2);
end-pi;

dcl-s wsCanUpdate ind inz(*off);
dcl-s wsReason char(20) inz(*blanks);

chain inCustNo CUSHDR;
if not %found(CUSHDR);
  wsReason = 'NOT FOUND';
elseif CUSTAT <> 'A';
  wsReason = 'STATUS INVALID';
elseif inNewLevel = *blanks;
  wsReason = 'LEVEL REQUIRED';
else;
  wsCanUpdate = *on;
endif;

if wsCanUpdate;
  CULEVL = inNewLevel;
  update CUSHDRR;
endif;

return;
```

## Example 4: Report Preparation Skeleton

Best for report or extract programs that read, filter, and stage output records.

Why this is recommended:

- The control flow is predictable for operations teams.
- Read, filter, write sections are easy to patch independently.
- Useful when the request is output-oriented rather than interactive.

Do not change these invariants:

- Read/filter/write flow stays in one visible loop.
- Output population stays adjacent to `write`.
- No update-style branching unless the request explicitly adds it.

```rpgle
**FREE

ctl-opt dftactgrp(*no) actgrp(*caller);

dcl-f SALHDR usage(*input) keyed extfile('SALHDR');
dcl-f RPTOUT usage(*output) extfile('RPTOUT');

setll *loval SALHDR;
read SALHDR;
dow not %eof(SALHDR);
  if SHSTAT = 'C';
    clear RPTOUTR;
    RPTCUST = SHCUST;
    RPTAMT = SHAMT;
    write RPTOUTR;
  endif;

  read SALHDR;
enddo;

return;
```
