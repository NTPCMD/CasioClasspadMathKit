# FX-CP400 ClassPad Syntax Reference (Strict Pass)

This project now targets **ClassPad II FX-CP400 parser-safe syntax** with conservative rules.

## Strict compatibility profile used in this repository

- Program names: **max 8 characters**
- Variable names: keep **8 characters or fewer** for safety
- Assignment: `→`
- Flow blocks: `If ... Then`, `Else`, `IfEnd`, `For ... Next`, `While ... WhileEnd`
- Menu loop pattern: `Lbl 1` + `Menu` + branch labels + `Goto 1`
- Program calls: `Prog "NAME"`
- Exit to caller/menu: `Return`
- Comparators: use `=`, `≠`, `<`, `<=`, `>`, `>=`
- Comments in source files: `'` at line start

## Syntax verification matrix

| Feature | Repository syntax | Status |
|---|---|---|
| Menu | `Menu "TITLE", "ITEM",A, ...` | Compatible pattern used |
| Locate | `Locate col,row,value` | Compatible pattern used |
| Input | `Input "PROMPT",VAR` | Compatible pattern used |
| Prog call | `Prog "NAME"` | Compatible pattern used |
| Return | `Return` | Compatible pattern used |
| Labels/Goto | `Lbl A`, `Goto 1` | Compatible pattern used |
| String handling | `If STR="" Then`, `If STR≠"" Then` | Compatible pattern used |
| Pause | `Pause` | Compatible pattern used |
| ClrText | `ClrText` | Compatible pattern used |
| Comments | `' comment` | Source-level convention |

## Known hardware-check items (must test on device/emulator)

- Unicode token transfer (`π`, `√`) through the final import path.
- Scientific-notation rendering behavior in the current `CRESULT` flow.
- Exact behavior of top-level `Return` from directly launched programs.

## Core examples

```text
Menu "MATHKIT",
"ALGEBRA",A,
"EXIT",Z

Lbl A
Prog "MN_ALG"
Goto 1

Lbl Z
Return
```

```text
If ERRMSG≠"" Then
 Locate 1,7,ERRMSG
 Pause
 Return
IfEnd
```
