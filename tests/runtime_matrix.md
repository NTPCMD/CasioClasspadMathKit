# Runtime Matrix

| Area | Test | Expected | Status |
|---|---|---|---|
| Parser | minimal HELLO parse | no syntax error | pending |
| Parser | Menu token parse | menu appears | pending |
| Runtime | `Prog` call/return | return to caller menu | pending |
| Runtime | `Return` at top-level | exits program cleanly | pending |
| Menus | repeat loop | no ghost redraw | pending |
| Variables | `A` persists in session | retained until cleared | pending |
| Variables | `Str 1` write/read | exact round-trip | pending |
| UTF | `π` tokenized | no garbling | pending |
| UTF | `√` tokenized | no garbling | pending |
| Imports | txt->program object | appears in Program list | pending |
| Imports | g1m bundle import | all modules created | pending |
| Memory | large module set | import succeeds under limit | pending |
