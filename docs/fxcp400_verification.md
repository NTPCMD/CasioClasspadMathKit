# FX-CP400 Strict Syntax Verification Report

This pass verifies repository source against conservative ClassPad II FX-CP400 compatibility rules.

## 1) Command verification status

| Command area | Status | Notes |
|---|---|---|
| Menu | PASS (syntax normalized) | Uses `Menu ...` + `Lbl` + `Goto` loop pattern |
| Locate | PASS | Uses `Locate col,row,value` consistently |
| Input | PASS | Standardized to `Input "...",VAR` form |
| Prog call | PASS | All calls use `Prog "NAME"` |
| Return | PASS | Used for safe exits from subprograms and menus |
| Lbl/Goto | PASS | Used primarily in menu loops |
| String compare | PASS | Standardized on `=` and `≠` |
| Pause | PASS | Used for user-visible stop points only |
| ClrText | PASS | Used in shared format program |
| Comments | PASS (source convention) | `'` line comments in `.txt` source files |

## 2) Parser-compatibility conversions completed

- Renamed programs to <=8 characters:
  - `CORE_CONST`→`CCONST`
  - `CORE_ERROR`→`CERROR`
  - `CORE_FORMAT`→`CFORMAT`
  - `CORE_INPUT`→`CINPUT`
  - `CORE_RESULT`→`CRESULT`
  - `CORE_VALID`→`CVALID`
  - `MES_CAREA`→`MES_CAR`
- Renamed long shared identifiers to <=8 chars:
  - `RESULT_MODE`→`RESMODE`
  - `DISP_PREC`→`DPREC`
  - `ROUND_MODE`→`RMODE`
  - `ANGLE_MODE`→`AMODE`
  - `IN_PROMPT`→`INPRMPT`
- Normalized inequality operator from `<>` to `≠`.
- Updated rounding call in result formatter to `fRound(...)`.

## 3) Remaining hardware-runtime checks required

These are syntax-safe in source, but must still be confirmed in manager/emulator/hardware runtime:

- UTF symbol transfer (`π`, `√`) through export/import.
- Scientific notation mode behavior in current `CRESULT` branch.
- Final top-level `Return` behavior when launching programs directly from calculator app list.

## 4) Architecture preservation check

- Core systems preserved (`CCONST`, `CFORMAT`, `CVALID`, `CRESULT`, `CERROR`, `CINPUT`).
- Menu hierarchy preserved and expanded modularly.
- No monolithic merge introduced.
