# FX-CP400 Program Template (ClassPad-safe)

All future programs should follow this exact structure.

```text
'================================
' PROGRAM: AAA_BBBB
' PURPOSE: Describe purpose
'================================

"PROGRAM"→TITLE
Prog "CFORMAT"

' INPUT SECTION
Input "VALUE?",A

' VALIDATION SECTION
' set VALMODE and call shared validator
1→VALMODE
Prog "CVALID"
If VALID=0 Then
 Return
IfEnd

' CALCULATION SECTION
A^2→RESVAL

' OUTPUT SECTION
"RESULT="→RESLBL
1→RESMODE
Prog "CRESULT"

Pause
Return
```

## Naming safety rules

- Program name max 8 chars
- Variable names max 8 chars
- No spaces in program names
