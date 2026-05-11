# Standard Program Template

All future programs should use this exact structure.

```text
'================================
' PROGRAM: XXX_NAME
' PURPOSE: Describe purpose
'================================

"PROGRAM TITLE"→TITLE
Prog "CORE_FORMAT"

' INPUT SECTION
Input "Prompt?",VAR

' VALIDATION SECTION
' set VALMODE or ERRMSG/VALID then call CORE_VALID/CORE_ERROR

' CALCULATION SECTION
' compute RESVAL or local outputs

' OUTPUT SECTION
"RESULT="→RESLBL
1→RESULT_MODE
Prog "CORE_RESULT"

Pause
Return
```
