# Verified Subroutine Call System

**Status**: RESEARCH FINDINGS IMPLEMENTED  
**Date**: 2026-05-13  
**Target**: CASIO ClassPad II FX-CP400

---

## Problem Statement

**Original Architecture** uses unconfirmed syntax:
```
Prog "PROGRAM_NAME"
```

**Research Findings**:
- ✓ Prog "NAME" appears in official CASIO manuals (promising)
- ✗ Not yet verified on real FX-CP400 hardware
- ⚠ May need alternative bare program call syntax

**Solution**: Maintain Prog "NAME" for now, document as unconfirmed, prepare alternatives.

---

## Current Implementation (Test Build)

### Architecture 1: Prog "NAME" (Current - Unconfirmed)

**File**: All test build programs  
**Pattern**:
```casio
Prog "CFORMAT"
```

**Status**: Used in current test build  
**Risk**: May not be parser-supported syntax  
**Fallback**: Will document and test alternatives if hardware fails

---

## Alternative Syntaxes (If Prog "NAME" Fails)

### Alternative 1: Bare Program Call

**Pattern** (if supported):
```casio
' Direct program call without Prog keyword
CFORMAT
```

**Hypothesis**: FX-CP400 may auto-resolve program names  
**Risk**: Unlikely to work, but documented for exploration  
**Test**: First hardware test will reveal if needed

---

### Alternative 2: Gosub Label Refactoring

**Pattern** (if subroutines must be inline):
```casio
' Instead of: Prog "CFORMAT"
' Use shared subroutine section at end of main program

Lbl CFORMAT_SUB
  ' ...shared code here...
Goto BACK_FROM_SUB

Lbl BACK_FROM_SUB
' Resume main program
```

**Complexity**: High - requires code merging  
**Risk**: Program size grows, maintenance harder  
**Status**: Plan B if Prog "NAME" fails completely

---

### Alternative 3: Manual Code Duplication

**Pattern** (if modular calls impossible):
```casio
' Instead of: Prog "CFORMAT"
' Copy CFORMAT code inline into each calling program

' CFORMAT inline:
Prog "CCONST"
ClrText
Locate 1,1,"================"
' ...etc...
```

**Complexity**: Very High  
**Risk**: Code duplication, maintenance nightmare  
**Status**: Last resort only

---

### Alternative 4: Array-Based Dispatch

**Pattern** (if Prog "NAME" fails):
```casio
' Use dispatcher with numeric codes
' This would be FX-CP400 specific

1→PROG_ID    ' Request CFORMAT
Prog "DISPATCHER"
```

**Complexity**: Extreme - requires runtime dispatcher  
**Risk**: Slow, hard to debug  
**Status**: Theoretical fallback only

---

## Decision Tree: Which to Use

```
                   First hardware test
                          ↓
              Does Prog "NAME" work?
                   /             \
                YES              NO
                 ↓                ↓
            Use current      Investigate
            implementation    alternatives
                ↓                ↓
            Continue         Try: Bare call
            development      If fails:
                ↓            Try: Gosub
            Document       If all fail:
            as CONFIRMED    Use duplication
```

---

## Implementation: Test Build

The test build is set up to use **Prog "NAME"** syntax:

### Programs Using Prog Calls

| Caller | Called | Type | Purpose |
|--------|--------|------|---------|
| MN_MAIN | MN_LIN, MN_TRI | Navigation | Menu module dispatch |
| MN_LIN | LIN_GRAD | Navigation | Linear gradient program |
| MN_TRI | TRI_PYTH | Navigation | Pythagorean program |
| LIN_GRAD | CFORMAT | Core | Output formatting |
| LIN_GRAD | CVALID | Core | Input validation |
| LIN_GRAD | CRESULT | Core | Result display |
| TRI_PYTH | CFORMAT | Core | Output formatting |
| TRI_PYTH | CVALID | Core | Input validation |
| TRI_PYTH | CRESULT | Core | Result display |
| CFORMAT | CCONST | Core | Constants initialization |
| CRESULT | CCONST | Core | Constants initialization |
| CVALID | CERROR | Core | Error display |

### Example: LIN_GRAD Using Multiple Progs

```casio
'================================
' PROGRAM: LIN_GRAD
' PURPOSE: Gradient calculation with shared services
'================================

"LINEAR GRAD"→TITLE
Prog "CFORMAT"              ' Request formatting

Input "X1?",X1
Input "Y1?",Y1
Input "X2?",X2
Input "Y2?",Y2

X2-X1→DX
DX→DEN
1→VALMODE
Prog "CVALID"               ' Request validation
If VALID=0 Then
 Return
IfEnd

(Y2-Y1)/DEN→RESVAL
"GRAD="→RESLBL
1→RESMODE
Prog "CRESULT"              ' Request result display

Pause
Return
```

---

## File Structure (Modular)

```
calculator_import/test_build/source/
├── MN_MAIN.txt          ← Entry point (does Prog "MN_LIN" / Prog "MN_TRI")
├── MN_LIN.txt           ← Menu module (does Prog "LIN_GRAD")
├── MN_TRI.txt           ← Menu module (does Prog "TRI_PYTH")
├── LIN_GRAD.txt         ← Computation (does Prog "CFORMAT", Prog "CVALID", etc.)
├── TRI_PYTH.txt         ← Computation (does Prog "CFORMAT", Prog "CVALID", etc.)
├── CFORMAT.txt          ← Shared (does Prog "CCONST")
├── CVALID.txt           ← Shared (does Prog "CERROR")
├── CRESULT.txt          ← Shared (does Prog "CCONST")
├── CCONST.txt           ← Constants (no Prog calls)
└── CERROR.txt           ← Error handler (no Prog calls)
```

**Modularity Benefits**:
- ✓ Programs ~20-40 lines each (manageable)
- ✓ Shared code in CFORMAT/CRESULT/CVALID (DRY principle)
- ✓ Easy to test individual modules
- ✓ Easy to extend with new modules

---

## Hardware Test Strategy

### Test Phase 1: Confirm Prog "NAME" Works

**Objective**: Verify subroutine call system is functional

**Test Case 1.1**: MN_MAIN execution
```
Run MN_MAIN on FX-CP400
Expected: Should execute Prog "MN_LIN" and Prog "MN_TRI" successfully
Result: ✓ PASS = Prog "NAME" works
Result: ✗ FAIL = Need alternative syntax
```

**Test Case 1.2**: Nested Prog calls
```
Run LIN_GRAD (which calls CFORMAT → which calls CCONST)
Expected: Three levels of Prog calls execute successfully
Result: ✓ PASS = Nesting works
Result: ✗ FAIL = May need depth limit or alternatives
```

**Test Case 1.3**: Conditional Prog success
```
Run TRI_PYTH with invalid input
- Should call Prog "CVALID"
- CVALID should detect error
- CVALID should call Prog "CERROR"
Expected: Error message displays, program terminates gracefully
Result: ✓ PASS = Error path works
Result: ✗ FAIL = Error dispatch broken
```

---

### Test Phase 2: If Prog "NAME" Fails

**Fallback Testing** (execute in this order):

#### Test 2.1: Bare Program Call
```
Edit MN_MAIN to use bare syntax:
  MN_LIN
  MN_TRI
(without Prog keyword)

Run on FX-CP400
Result: ✓ PASS = Use bare syntax in production
Result: ✗ FAIL = Proceed to Test 2.2
```

#### Test 2.2: Inline Gosub Refactoring
```
Merge CFORMAT code directly into LIN_GRAD
Run LIN_GRAD on FX-CP400
Result: ✓ PASS = Feasible but high-maintenance
Result: ✗ FAIL = Proceed to Test 2.3
```

#### Test 2.3: Accept Limitation
```
If all subroutine methods fail:
- Document FX-CP400 does NOT support program calls
- Plan architecture for flat (non-modular) structure
- Reconsider product design
```

---

## Documentation Updates Needed

When hardware test confirms subroutine behavior, update:

1. **docs/classpad_syntax.md**
   - Add: "Confirmed: Prog "NAME" works" OR "Not supported"
   - Document exact syntax

2. **calculator_import/test_build/source/README.md**
   - Add: Subroutine calling pattern used
   - Document any limitations discovered

3. **Program headers**
   - Update: "NOTE: Uses Prog "NAME" which is [CONFIRMED/UNCONFIRMED]"

---

## Production Build Implications

### If Prog "NAME" Confirmed ✓
```
Architecture Choice: KEEP CURRENT
Benefits:
  - Modular design sustainable
  - Easy to maintain and extend
  - Clean separation of concerns
  - Matches documentation
  
Next Steps:
  - Document as "CONFIRMED"
  - Expand to full module set (8 topics)
  - Plan release distribution
```

### If Prog "NAME" Not Supported ✗
```
Architecture Choice: REFACTOR FOR INLINE CODE
Benefits:
  - Works within hardware limitations
  - No runtime dispatch overhead
  - Match actual calculator capabilities
  
Challenges:
  - Code duplication
  - Harder maintenance
  - Larger programs
  
Next Steps:
  - Inline shared code into each program
  - Increase program sizes (~100-200 lines each)
  - Plan new modular structure with Gosub
```

---

## Current Status

### For Test Build (now)
- ✓ Using Prog "NAME" syntax
- ✓ Modular 9-program structure
- ✓ Ready for first hardware test
- ⏳ Awaiting confirmation

### For Next Phase
- ⏳ Hardware test Phase 1 & 2
- ⏳ Document results
- ⏳ Decide architecture
- ⏳ Plan production build

---

## References

- Test build programs: calculator_import/test_build/source/
- Real hardware test: tests/real_hardware_test.md (Category B)
- ClassPad syntax: docs/classpad_syntax.md
- Deployment: calculator_import/test_build/docs/DEPLOYMENT.md

---

## Summary

**Current Subroutine System**:
- Modular design using `Prog "NAME"`
- Clear call hierarchy
- manageable program sizes
- Awaiting hardware confirmation

**If Confirmed**: Continue as designed for full release  
**If Failed**: Have documented alternatives ready

**Next Step**: Import test build to FX-CP400 and verify subroutine calls work.
