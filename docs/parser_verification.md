# FX-CP400 Parser Verification (ClassPad II)

Date: 2026-05-13
Scope: Commands and syntax used by this repository, validated for real FX-CP400 deployment planning.

## Status Legend
- **CONFIRMED**: syntax/behavior confirmed via existing repo tests + ClassPad-oriented command references.
- **UNCONFIRMED**: not yet hardware-proven on physical FX-CP400 in this repo workflow.
- **MODEL-DEPENDENT**: behavior differs across ClassPad firmware/emulator/editor context.

## Command Matrix

| Command / Element | Status | FX-CP400-safe syntax baseline | Notes |
|---|---|---|---|
| `Menu` | MODEL-DEPENDENT | `Menu "T",1,"A",2` | Exists on ClassPad BASIC, but visual rendering and nesting differ in app contexts. |
| `Locate` | CONFIRMED | `Locate 1,1,"TXT"` | Core text placement command for Program app. |
| `Input` | CONFIRMED | `Input "A?",A` | Prompt+variable form accepted in ClassPad program syntax. |
| `Prog` | CONFIRMED | `Prog "MN_MAIN"` | String target program call. Name resolution depends on exact program names. |
| `Pause` | MODEL-DEPENDENT | `Pause` or `Pause "MSG"` | Prompted pause support can vary; use bare `Pause` for highest safety. |
| `Return` | CONFIRMED | `Return` | Valid for subprogram exit. |
| `Lbl` | CONFIRMED | `Lbl 1` | Numeric labels are safest cross-model form. |
| `Goto` | CONFIRMED | `Goto 1` | Must target existing label in same program. |
| `fRound` | MODEL-DEPENDENT | `fRound(expr,n)` | Function availability/behavior tied to mode/settings; test per firmware. |
| `ClrText` | CONFIRMED | `ClrText` | Valid text screen clear in Program context. |
| Unicode (`π`,`√`) | MODEL-DEPENDENT | Prefer tokenized entry on-device | UTF-8 source import can corrupt unless editor converts to tokens. |
| Comments | MODEL-DEPENDENT | `' comment` | Single-quote comments supported in BASIC, but some import paths strip/alter. |
| Variable names | CONFIRMED | `A`..`Z`, `Str 1`, list/mat vars | Prefer canonical single-letter numeric vars for portability. |
| String behavior | MODEL-DEPENDENT | `"ABC"`, `Str 1` | String length limits and control char behavior must be hardware-tested. |

## Line-by-line review guidance
Use this checklist when reviewing each source line before release:
1. First token is valid ClassPad BASIC token (not pseudocode).
2. Program names in `Prog` are quoted and within name constraints.
3. `Locate` coordinates are integer and display-safe.
4. `Input` targets simple scalar vars unless explicit string test.
5. `Goto`/`Lbl` pairs exist and are unique per routine.
6. Unicode symbols replaced with token-safe alternatives where import path is uncertain.
7. Comments removed from release bundle if import path has comment-loss risk.

## Repository policy for parser stability
- Release bundles should use **ASCII-safe sources** unless UTF tokenization has been hardware-verified.
- Minimal programs in `tests/minimal/` are canonical parser references.
- Any new command must be added to this matrix with status and hardware evidence.
