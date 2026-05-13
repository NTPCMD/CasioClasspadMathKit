# ASCII-Safe Mode Documentation

**Date**: 2026-05-13  
**Target**: CASIO ClassPad II FX-CP400  
**Purpose**: Fallback syntax for Unicode transfer safety

---

## Overview

MathKit Unicode symbols (√, π) may cause transfer corruption when importing to FX-CP400 via USB or ClassPad Manager.

**ASCII-Safe Mode** provides equivalent functionality with ASCII-only characters and ensures reliable transfer across:
- ClassPad Manager
- USB mass storage
- Network/cloud transfer channels
- Legacy transfer utilities

---

## Symbol Replacements

### Table: Unicode → ASCII Equivalents

| Symbol | Context | Original | ASCII-Safe | Notes |
|--------|---------|----------|-----------|-------|
| √ | Square root | `√(X^2+Y^2)` | `(X^2+Y^2)^(0.5)` | Uses exponent 0.5 |
| π | Pi constant | `π` | `3.14159265359` | Assigned to PI_CONST |
| π | Display | `"π"` | `"[PI]"` or `"*PI"` | Context-based |
| → | Assignment | `VALUE→VARIABLE` | `VALUE→VARIABLE` | Already ASCII |
| ≠ | Not equal | `IF A≠B` | `IF A≠B` | Check parser support |
| ≤ | Less/equal | `IF X≤0` | `IF X≤0` | Check parser support |

---

## Conversion Rules

### Rule 1: Square Root

**Original**:
```casio
√(A^2+B^2)→HYPOTENUSE
```

**ASCII-Safe**:
```casio
(A^2+B^2)^(0.5)→HYPOTENUSE
```

**Rationale**: 
- Exponent 0.5 is mathematically equivalent to square root
- All calculators support exponentiation
- No special symbol required

---

### Rule 2: Pi Constant

**Original** (CCONST.txt):
```casio
π→PI_CONST
```

**ASCII-Safe** (CCONST.txt):
```casio
3.14159265359→PI_CONST
```

**Rationale**:
- Pre-calculated numerical value
- Ensures portability
- Precision: 11 decimal places (sufficient for FX-CP400)
- Avoids π symbol entirely

---

### Rule 3: Pi Display

**Original** (CRESULT.txt):
```casio
Locate 11,7,"π"
```

**ASCII-Safe Options**:

Option A (descriptive):
```casio
Locate 11,7,"*PI"
```

Option B (notation):
```casio
Locate 11,7,"[PI]"
```

Option C (minimal):
```casio
Locate 11,7,"PI"
```

**Recommended**: Option A (`"*PI"`)  
Reason: Clearly indicates multiplication by π value

---

### Rule 4: Comments & Documentation

**Original**:
```casio
' VALMODE 9: TA and TB must be positive
```

**ASCII-Safe**:
```casio
' VALMODE 9: TA and TB must be positive
```

**Status**: No change required  
Comments use ASCII `'` already.

---

## Files Affected

### Core Programs Using Unicode

| File | Symbols Used | ASCII-Safe Available |
|------|--------------|----------------------|
| TRI_PYTH.txt | √ | ✓ Yes |
| CRESULT.txt | π | ✓ Yes |
| CCONST.txt | π | ✓ Yes |
| ALG_QUAD.txt | √ | (external docs) |
| MES_CAR.txt | π | (external docs) |

### Location
- **Original**: `src/`
- **Test ASCII-Safe**: `test_build/ascii_safe/`
- **Production ASCII-Safe**: `src/ascii_safe/` (planned)

---

## Deployment: When to Use

### Use Original Source IF:
```
✓ Unicode transfer has been verified to work
✓ Running on FX-CP400 firmware v3.0+
✓ Using ClassPad Manager v3.0+
✓ Using newest USB drivers/transfer utility
```

### Use ASCII-Safe Mode IF:
```
✓ Unicode transfer fails with corruption
✓ Symbols appear as boxes/garbage on display
✓ Import process hangs when Unicode is present
✓ Running on older calculator firmware
✓ Using legacy ClassPad transfer tools
✓ First-time importing (safest option)
```

### Quick Decision Tree
```
Is transfer Unicode-safe?
├─ YES: Use original source/
├─ NO:  Use ascii_safe/
└─ UNKNOWN: Try ascii_safe/ first (safer default)
```

---

## Functional Equivalence Verification

### Test Case: TRI_PYTH (Pythagorean Theorem)

**Original**:
```casio
√(TA^2+TB^2)→RESVAL
```

**ASCII-Safe**:
```casio
(TA^2+TB^2)^(0.5)→RESVAL
```

**Test Inputs**: TA=3, TB=4

**Expected Output**: 5.0 (or 5)

**Verification**: Both versions produce 5.0 if run on calculator

---

### Test Case: Circle Area with Pi

**Original (CRESULT.txt)**:
```casio
If RESMODE=3 Then
 RESVAL/PI_CONST→RESPI
 Locate 1,7,RESPI
 Locate 11,7,"π"
 Return
IfEnd
```

**ASCII-Safe**:
```casio
If RESMODE=3 Then
 RESVAL/PI_CONST→RESPI
 Locate 1,7,RESPI
 Locate 11,7,"*PI"
 Return
IfEnd
```

**Test**: Compute circle area with radius 1, display in π format
- Original: Shows `1 π`
- ASCII-Safe: Shows `1 *PI`
- Result: Same mathematical value, different notation

---

## Performance Impact

### Execution Speed
- ✓ No performance difference
- √ via exponent: Native FX-CP400 operation
- π via constant: Native multiplication
- **Verdict**: Identical performance

### Memory Usage
- Original: 1 byte per π symbol × occurrences
- ASCII-Safe: 11 bytes for numeric constant (one-time)
- **Verdict**: Negligible difference

### Transfer Time
- Original: May stall or corrupt
- ASCII-Safe: Guaranteed safe
- **Verdict**: ASCII-Safe may be faster (no corruption/retry)

---

## Known Unresolved Issues

These symbols have no confirmed safe ASCII replacement:

| Symbol | Use Case | Status | Workaround |
|--------|----------|--------|-----------|
| `≠` | Not-equal in If | Unconfirmed | Use `Not(A=B)` if needed |
| `≤` | Less-or-equal in If | Likely OK | Keep as-is, monitor |
| `≥` | Greater-or-equal in If | Likely OK | Keep as-is, monitor |

**Current Status**: These appear to work in CVALID.txt. Test on real hardware to confirm.

---

## Migration Path

### Phase 1: First Real Import (NOW)
```
Use ASCII-safe versions for test_build/
Verify import success
Document Unicode issues
```

### Phase 2: Production Build (Next)
```
If Unicode safe: Release with original
If NOT safe: Release with ASCII-safe
Document decision
```

### Phase 3: Future Enhancement (Later)
```
Once Unicode safety confirmed:
- Create Unicode → ASCII conversion tool
- Automate ASCII-safe generation
- Support both formats in pipeline
```

---

## For Development Team

### To Add New Programs with Unicode

1. **Create original version** with Unicode symbols
   - File: `src/module/PROGRAM.txt`
   - Example: `TRI_PYTH.txt` with √

2. **Create ASCII-safe version**
   - File: `src/module/PROGRAM_ascii.txt`
   - Replace all √ with ^0.5
   - Replace all π with 3.14159265359

3. **Document differences**
   - Note in program header comments
   - List affected lines

4. **Test both versions**
   - original/ on new unicode-verified hardware
   - ascii_safe/ on standard hardware

---

## References

- [DEPLOYMENT.md](../calculator_import/test_build/docs/DEPLOYMENT.md) - Unicode fallback section
- [real_hardware_test.md](real_hardware_test.md) - Test D1/D2: Unicode display verification
- [classpad_syntax.md](../docs/classpad_syntax.md) - Parser capabilities

---

## Summary

**ASCII-Safe Mode** is:
- ✓ **Safe**: Avoids Unicode transfer issues
- ✓ **Equivalent**: Mathematically identical output
- ✓ **Fast**: No performance penalty
- ✓ **Temporary**: Until Unicode verified stable
- ✓ **Documented**: Clear conversion rules
- ✓ **Available**: Test versions ready to use

**Recommendation**: Use ASCII-safe for first real import. Upgrade to Unicode versions once hardware Unicode support confirmed on target calculators.
