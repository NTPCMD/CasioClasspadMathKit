# FX-CP400 Command Support Matrix

Status meanings:

- **CONFIRMED**: syntax is documented for ClassPad II FX-CP400 and used in repository source.
- **UNCONFIRMED**: not yet backed by enough evidence for this project.
- **MODEL-DEPENDENT**: supported, but important behavior can vary by context/firmware.

| Command | Status | Notes |
|---|---|---|
| `Menu` | CONFIRMED | Canonical menu syntax used in `RFMENU` and `MN_MAIN`. |
| `Locate` | CONFIRMED | Use `Locate col,row,value` with text-screen coordinates. |
| `Input` | CONFIRMED | Use `Input "PROMPT",VAR` numeric form. |
| `Prog` | CONFIRMED | Subprogram call syntax is `Prog "NAME"`. |
| `Pause` | CONFIRMED | Safe blocking command for user acknowledgement. |
| `Return` | MODEL-DEPENDENT | Confirmed for subprogram return; top-level exit behavior must still be tested on hardware. |
| `Lbl` | CONFIRMED | Local program labels only. |
| `Goto` | CONFIRMED | Local branch only; use mainly for menu loops. |
| `fRound` | MODEL-DEPENDENT | Supported, but precision semantics must be validated on target hardware. |
| `ClrText` | CONFIRMED | Clears the text/Locate output area. |

## Canonical references

- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFMENU.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFINPUT.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFRES.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFVALID.txt`
