# Real Hardware Test Checklist

**Target**: CASIO ClassPad II FX-CP400  
**Test Build**: MathKit First Real Import  
**Date**: 2026-05-13

---

## PRE-TEST REQUIREMENTS

Before running this checklist, you must have:

1. ✓ Imported test_build/ package to FX-CP400
2. ✓ All 11 programs visible in Program app
3. ✓ MN_MAIN launches without "Prog not found" errors
4. ✓ Menu structure displays correctly
5. ✓ Computer + calculator remain connected (optional, for logging)

---

## TEST MATRIX

### CATEGORY A: IMPORT & PARSER VALIDATION

#### Test A1: Program List Verification
**Objective**: Verify all programs imported successfully

```
Hardware: FX-CP400
Step 1: Open Program app from main menu
Step 2: Scroll through program list

VERIFY - List contains (in any order):
  [ ] CERROR
  [ ] CFORMAT
  [ ] CRESULT
  [ ] CONST
  [ ] CVALID
  [ ] LIN_GRAD
  [ ] MN_MAIN
  [ ] MN_LIN
  [ ] MN_TRI
  [ ] TRI_PYTH

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

#### Test A2: MN_MAIN Syntax Parsing
**Objective**: Verify parser recognizes Menu and Lbl syntax

```
Hardware: FX-CP400
Step 1: Scroll to MN_MAIN in program list
Step 2: Press EXE to run

VERIFY - Display shows:
  [ ] "================" header line
  [ ] "MATHKIT" or title text
  [ ] "================" footer line
  [ ] "EXE=OK  EXIT=BACK" instruction line
  [ ] Menu appears with options:
       [ ] LINEAR
       [ ] TRIG
       [ ] EXIT

VERIFY - No errors:
  [ ] No "Syntax Error" message
  [ ] No "Prog not found" message
  [ ] No crash/freeze on menu display

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

---

### CATEGORY B: NAVIGATION & CONTROL FLOW

#### Test B1: Menu Navigation (LINEAR Branch)
**Objective**: Verify Lbl/Goto and Prog calls work

```
Hardware: FX-CP400
Step 1: Run MN_MAIN (should show menu from A2)
Step 2: Press EXE on LINEAR option
Step 3: Wait 0.5 seconds

VERIFY - Display shows:
  [ ] LINEAR submenu appears
  [ ] Shows option: "GRADIENT"
  [ ] Shows option: "BACK"
  [ ] No "Prog not found" error
  [ ] No syntax errors

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

#### Test B2: Menu Navigation (TRIG Branch)
**Objective**: Verify alternate Prog call works

```
Hardware: FX-CP400
Step 1: Run MN_MAIN
Step 2: Press EXE on TRIG option
Step 3: Wait 0.5 seconds

VERIFY - Display shows:
  [ ] TRIG submenu appears
  [ ] Shows option: "PYTH"
  [ ] Shows option: "BACK"

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

#### Test B3: Return Navigation
**Objective**: Verify Return and Goto maintain control flow

```
Hardware: FX-CP400
Step 1: From TRIG menu, press EXE on BACK
Step 2: Should return to MN_MAIN menu

VERIFY:
  [ ] MN_MAIN menu re-displays
  [ ] Same format as original (clean redisplay)
  [ ] Menu is navigable again
  [ ] No infinite loops or hangs

Step 3: From MN_MAIN, press EXIT key (or EXE on EXIT option)
Step 4: Program should terminate

VERIFY:
  [ ] Returns to Program app list
  [ ] MN_MAIN no longer running
  [ ] Calculator responsive to further commands

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

---

### CATEGORY C: COMPUTATION & VARIABLE HANDLING

#### Test C1: Input Prompts (LINEAR Module)
**Objective**: Verify Input statement and variable assignment work

```
Hardware: FX-CP400
Step 1: Run MN_MAIN
Step 2: LINEAR → GRADIENT

VERIFY - First input prompt appears:
  [ ] Prompt shows: "X1?"
  [ ] Cursor responds to keypresses
  [ ] Can type numeric values

Step 3: Enter value: 1
Step 4: Press EXE

VERIFY:
  [ ] Moves to next prompt: "Y1?"
  [ ] Previous value 1 not lost (stored in X1)

Step 5: Enter value: 2, press EXE (for Y1)
Step 6: Enter value: 4, press EXE (for X2)
Step 7: Enter value: 5, press EXE (for Y2)

VERIFY:
  [ ] All four inputs accepted
  [ ] No input buffer overflow
  [ ] Values stored correctly
  [ ] Computation completes without error

Expected output display:
  [ ] Shows: "GRAD=" label
  [ ] Shows numeric result: (5-2)/(4-1) = 1 (or 1.0)
  [ ] No "DIVIDE BY ZERO" error
  [ ] No variable corruption

Step 8: Press Pause/EXE to continue
Step 9: Should return to MN_MAIN menu

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

#### Test C2: Validation System (Pythagorean)
**Objective**: Verify validation dispatcher and error handling

```
Hardware: FX-CP400
Step 1: Run MN_MAIN
Step 2: TRIG → PYTH

VERIFY - Format display:
  [ ] Title shows "TRI PYTH"
  [ ] Border displays correctly
  [ ] Input prompt appears: "A?"

Step 3: Enter value: 3, press EXE
Step 4: Enter value: 4, press EXE

VERIFY:
  [ ] Computation occurs without error
  [ ] Output shows: "HYP=" label
  [ ] Output shows numeric result (should be ~5)
  [ ] No validation errors
  [ ] Returns to menu on EXIT

Step 5: Run PYTH again
Step 6: Try invalid input: -3, press EXE

VERIFY - Error handling:
  [ ] Shows error message (expected: format varies)
  [ ] Does NOT crash
  [ ] Allows graceful exit (EXIT or similar)
  [ ] Returns to MN_MAIN cleanly

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

---

### CATEGORY D: UNICODE & SPECIAL CHARACTERS

#### Test D1: Display of Special Characters
**Objective**: Verify Unicode symbols display or degrade gracefully

```
Hardware: FX-CP400
Step 1: Run TRIG → PYTH with inputs 3, 4
Step 2: Observe output display

VERIFY - Check for symbols:
  [ ] If using original source: √ may appear in calculation display
       OR
  [ ] If using ASCII-safe version: "^0.5" or "(A^2+B^2)^0.5" appears
  [ ] Output value displays correctly regardless

Step 3: Run LIN_GRAD
Step 4: Observe title and labels

VERIFY - Display quality:
  [ ] All text displays (no boxes/corruption)
  [ ] Readability acceptable
  [ ] No font/encoding errors

RESULT: PASS / FAIL / DEGRADED
NOTES: If Unicode displays poorly, this is expected - document for follow-up

_______________________________________________
```

#### Test D2: Menu Text Display
**Objective**: Verify all menu text renders without corruption

```
Hardware: FX-CP400
Step 1: Run MN_MAIN
Step 2: Cycle through menus: LINEAR, TRIG, BACK

VERIFY - All text displays:
  [ ] Menu headers appear correctly
  [ ] Option labels fully visible
  [ ] No missing/corrupted characters
  [ ] Layout looks professional

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

---

### CATEGORY E: REPETITION & STATE

#### Test E1: Multiple Executions
**Objective**: Verify no state corruption after repeated runs

```
Hardware: FX-CP400
Step 1: Run MN_MAIN twice in succession
Step 2: Navigate menus each time
Step 3: Run GRADIENT with different inputs (e.g., 0,0,1,1 then 10,20,30,40)

VERIFY:
  [ ] Both executions complete without error
  [ ] Different inputs produce different outputs
  [ ] No residual values from previous run
  [ ] Menu state resets properly

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

#### Test E2: Variable Persistence
**Objective**: Verify variables don't leak between programs

```
Hardware: FX-CP400
Step 1: Run GRADIENT with inputs: 1, 2, 3, 4
Step 2: Returns to menu
Step 3: Run PYTH with inputs: 5, 6

VERIFY:
  [ ] PYTH computation correct (hyp = ~7.81)
  [ ] No interference from previous GRADIENT values
  [ ] VALMODE and RESMODE properly set by each program

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

---

### CATEGORY F: ERROR RECOVERY

#### Test F1: Invalid Input Handling
**Objective**: Verify error messages display and allow recovery

```
Hardware: FX-CP400
Step 1: Run LIN_GRAD
Step 2: Inputs: X1=1, Y1=2, X2=1 (same as X1), Y2=5
          → This causes DEN = 0 (divide by zero)

VERIFY:
  [ ] Error message appears (e.g., "DIVIDE BY ZERO")
  [ ] Error message displays without crashing
  [ ] Can press EXE/EXIT to continue
  [ ] Returns to MN_MAIN cleanly (not stuck)

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

#### Test F2: Program Abort
**Objective**: Verify Pause and EXIT allow graceful termination

```
Hardware: FX-CP400
Step 1: Run any computation program (e.g., PYTH)
Step 2: At Pause screen after result, press EXIT key

VERIFY:
  [ ] Program terminates (doesn't freeze)
  [ ] Returns to MN_MAIN menu (if Goto implemented)
        OR
  [ ] Returns to Program app list (if Return used)
  [ ] No residual errors/messages

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

---

### CATEGORY G: HARDWARE INTEGRATION

#### Test G1: Calculator Responsiveness
**Objective**: Verify no permanent hangs or resource leaks

```
Hardware: FX-CP400
Step 1: Run test sequence:
  - MN_MAIN menu
  - LINEAR → GRADIENT (inputs: 0, 0, 1, 1)
  - Return to MN_MAIN
  - TRIG → PYTH (inputs: 3, 4)
  - Return to MN_MAIN
  - EXIT

Step 2: After EXIT, calculator should be fully responsive
Step 3: Return to home screen
Step 4: Open Calculator app (verify system works)

VERIFY:
  [ ] All programs run in <5 seconds each (typical)
  [ ] No delays or slowdowns
  [ ] No memory leaks (repeated runs don't slow down)
  [ ] Home button/menu navigation works normally

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

#### Test G2: Long-term Stability (Optional)
**Objective**: Verify system remains stable after extended use

```
Hardware: FX-CP400
Step 1: Repeat test sequence 10 times:
  - MN_MAIN
  - Navigate to each menu branch
  - Run one calculation
  - Return to home

Time limit: 10 minutes total

VERIFY:
  [ ] No performance degradation
  [ ] No error accumulation
  [ ] No memory warnings
  [ ] Calculator remains responsive

RESULT: PASS / FAIL
NOTES: _______________________________________________
```

---

## SUMMARY & INTERPRETATION

### PASS Criteria (All Green for First Real Success)

- [ ] Category A: 2/2 tests pass (Parser & imports work)
- [ ] Category B: 3/3 tests pass (Navigation/control flow works)
- [ ] Category C: 2/2 tests pass (Computation & validation work)
- [ ] Category D: 2/2 tests pass (Display works, Unicode handled)
- [ ] Category E: 2/2 tests pass (Repetition safe)
- [ ] Category F: 2/2 tests pass (Error handling works)
- [ ] Category G: At least G1 passes (System integration stable)

**Overall Result**: ✓ FIRST REAL IMPORT SUCCESSFUL

### Known Issues (Document for Future Fix)

If tests FAIL in specific areas:

1. **Parser failures** (Category A/B fails):
   - Indicates Prog "NAME" or Menu syntax not supported
   - ACTION: Report exact error, use alternative syntax
   
2. **Calculation failures** (Category C fails):
   - Indicates variable assignment or function support issue
   - ACTION: Check VALMODE/RESMODE dispatcher logic

3. **Unicode issues** (Category D fails):
   - Expected for √ and π symbols
   - ACTION: Use ascii_safe/ versions instead

4. **State corruption** (Category E fails):
   - Indicates global variable leak or Lbl/Goto issue
   - ACTION: Isolate by testing one module at a time
   - Check if variables properly scoped

5. **Error handling failures** (Category F fails):
   - Indicates If/Then/IfEnd or Prog "CERROR" not working
   - ACTION: Test validation paths separately

---

## LOGGING & DOCUMENTATION

After completing tests, record findings:

### Pass by Category
```
Category A (Parser):     PASS / FAIL
Category B (Navigation): PASS / FAIL
Category C (Compute):    PASS / FAIL
Category D (Unicode):    PASS / FAIL
Category E (Repetition): PASS / FAIL
Category F (Recovery):   PASS / FAIL
Category G (Hardware):   PASS / FAIL
```

### Overall Assessment
```
If 6/7 categories pass: FIRST REAL IMPORT SUCCESSFUL ✓
If 5/7 categories pass: LIKELY SUCCESSFUL (minor issues)
If 4/7 or fewer pass:   DEBUG REQUIRED (major issues)
```

### Document Issues
For each FAIL, create entry in `tests/parser_issues.md`:
```
- Test: [Category][Number]
- Expected: [what should happen]
- Actual: [what did happen]
- Program: [which program failed]
- Inputs: [test inputs if applicable]
- Error message: [exact error text]
- Action taken: [rollback / retry / switch to ASCII]
```

---

## NEXT STEPS AFTER TEST

### If PASS (6/7+ passing):
1. Document success with calculator model/firmware version
2. Proceed to full release build
3. Import remaining modules (algebra, stats, finance, etc.)
4. Plan VCP (full release package) workflow

### If Mixed Results:
1. Identify specific failing patterns
2. Check docs/classpad_syntax.md for workarounds
3. Try ASCII-safe versions for Unicode issues
4. Create isolated test case for other failures
5. Report to development team with exact reproduction steps

### If FAIL (multiple categories):
1. Check import procedure (see DEPLOYMENT.md)
2. Verify all 11 programs actually imported
3. Try manual reload from ascii_safe/ versions
4. Document exact errors
5. Contact team with reproduction steps

---

## REFERENCE

- [DEPLOYMENT.md](./DEPLOYMENT.md) - Import procedures
- [../../docs/classpad_syntax.md](../../docs/classpad_syntax.md) - Parser reference
- [../../tests/parser_issues.md](../../tests/parser_issues.md) - Known issues database
- [../../release/](../../release/) - Release artifacts

---

**Status: TEST CHECKLIST READY**

Begin testing once import is complete.
