# FX-CP400 Parser Test Suite

These checks validate that repository source stays inside the conservative FX-CP400 syntax profile.

## Canonical minimal reference programs

Use these files as parser baselines:

- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFHELLO.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFMENU.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFINPUT.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFRES.txt`
- `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/RFVALID.txt`

## Parser checks

### 1. Program names

- Program file stem must be 8 characters or fewer.
- Every `Prog "NAME"` target must exist as a sibling program object before export.
- Labels must stay local to a single file.

### 2. Allowed command baseline

The minimal references intentionally cover these commands:

- `Menu`
- `Locate`
- `Input`
- `Prog`
- `Pause`
- `Return`
- `Lbl`
- `Goto`
- `ClrText`

### 3. Source formatting checks

- One statement per line.
- Use `→` for assignment.
- Use `If ... Then` / `IfEnd` blocks exactly.
- Use `=` or `≠` for string comparison.
- Keep prompts and titles in quoted strings.
- Keep variables and shared identifiers at 8 characters or fewer.

### 4. Manual parser import checks in ClassPad Manager

For each minimal source file:

1. Create a program with the exact same name.
2. Paste the source into the program editor.
3. Confirm the editor accepts every line without syntax error.
4. Save the program object.
5. Re-open it and confirm token rendering is preserved.

### 5. Failure conditions

A parser test fails if any of the following occur:

- Manager rejects a line during paste or save.
- A `Prog` call target name is truncated or altered.
- `Menu` entries do not tokenize cleanly.
- `Locate` coordinates or quoted strings are rewritten unexpectedly.
- UTF symbols are replaced with placeholder glyphs.
