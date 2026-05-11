# FX-CP400 Canonical Minimal Reference Programs

These are the canonical minimal syntax references for deployment preparation.
They intentionally use only commands currently classified as confirmed or explicitly tracked in the command support matrix.

## Source files

- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFHELLO.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFMENU.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFINPUT.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFRES.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFVALID.txt`

## Purpose map

- `RFHELLO` = minimal `ClrText` + `Locate` + `Pause` + `Return`
- `RFMENU` = minimal `Menu` + `Lbl` + `Goto` + `Prog` + `Return`
- `RFINPUT` = minimal `Input` + `Locate` echo flow
- `RFRES` = minimal assignment + calculation + result display
- `RFVALID` = minimal input validation branch with clear success/failure output

## Deployment note

These reference programs are prepared for emulator/hardware validation, but this repository environment still cannot execute FX-CP400 firmware directly.
Use the runtime test suite before marking them hardware-approved.
