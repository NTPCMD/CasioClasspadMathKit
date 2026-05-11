# ClassPad Syntax Reference

## Project Conventions

- Use arrow-assignment style in loops and assignments where shown in this guide.

## Feature Syntax Table

| Feature | Syntax |
|---|---|
| If | `If X>0 Then` |
| Else | `Else` |
| Loop | `While` |
| For | `For 1→I To 10` |

| Input | `Input "?",A` |
| Output | `Locate 1,1,"TEXT"` |
| Menu | `Menu "TITLE"` |
| Stop | `Stop` |
| Labels | `Lbl` |
| Goto | `Goto` |

## Core Commands You Need

### Variables
```
5→A
A+1→A
```

### Input
```
Input "Radius?",R
```

### Output
```
Locate 1,1,"HELLO"
```

### If Statements
```
If A>0 Then
 Locate 1,1,"POSITIVE"
Else
 Locate 1,1,"NEGATIVE"
IfEnd
```

### While Loops
```
While A<10
 A+1→A
WhileEnd
```

### For Loops

```
For 1→I To 10
 Locate 1,I,I
Next
```

### Menus
```
Menu "MATHKIT",
"ALGEBRA",A,
"TRIG",B,
"EXIT",Z
```

### Labels/Goto
```
Lbl A
Goto A
```

Use `Goto` mainly for menu-loop control; avoid it in calculation logic where structured flow is clearer.


## ClassPad compatibility review notes

The following items should be verified on real Casio ClassPad hardware/software before final export:

- `Prog "NAME"` invocation syntax can vary by model/OS; confirm exact call syntax.
- String comparison support (e.g., `If IN_STR="" Then`) should be validated for the chosen ClassPad runtime.
- `Return` behavior from top-level programs vs subprograms may differ across models.
- Unicode symbols such as `π` and `√` are used intentionally; verify UTF-safe transfer path during import/export.
- Label token support (`Lbl`) should be confirmed against actual parser expectations.
