# First Golden VCP Deployment Plan

**Status**: PLANNING  
**Date**: 2026-05-13  
**Target**: CASIO ClassPad II FX-CP400  
**Format**: VCP (full calculator memory package)

---

## Overview

**VCP** (full release package) contains entire calculator state and is used for classroom/bulk deployment via ClassPad Manager AutoImport.

This document prepares the **FIRST GOLDEN VCP** workflow without full automation.

---

## What is VCP?

| Property | VCP | XCP |
|----------|-----|-----|
| Format | Full calculator state | Single program |
| Size | ~50-500 KB | ~1-5 KB per program |
| Use Case | Bulk deployment | Testing/modular import |
| Safety | Higher (complete state) | Lower (missing programs risk) |
| Import Tool | ClassPad Manager (easy) | Direct or txt2xcp |
| Lock-in | Yes (full calculator state) | No (additive) |

---

## Design Principles (Do NOT Automate Yet)

### Principle 1: Manual Verification
```
Goal: Ensure first VCP is 100% verified before release
Why: Cannot easily rollback VCP errors
Approach: Each step manual, documented, verified
```

### Principle 2: Golden Reference
```
Goal: Create known-good reference snapshot
Why: Future VCP builds can compare against this
Approach: Keep original files, document exact process
```

### Principle 3: Classroom Ready
```
Goal: VCP ready for immediate distribution
Why: Teachers need install-and-go experience
Approach: Include example data, test results, documentation
```

---

## Prerequisites

Before creating first golden VCP:

1. ✓ Successful XCP import test (see tests/real_hardware_test.md)
2. ✓ All 5 core programs working: MN_MAIN, LIN_GRAD, TRI_PYTH, CFORMAT, CVALID (+ 4 support)
3. ✓ No known parser issues
4. ✓ Hardware (or emulator) running stable
5. ✓ ClassPad Manager installed with export capability

---

## Manual Workflow: Create First Golden VCP

### Phase 1: Verify Test Build Success

**Step 1.1: Confirm Hardware State**
```
On FX-CP400 (or emulator):
  [ ] Open Program app
  [ ] Verify all 9 programs present
  [ ] Run MN_MAIN successfully
  [ ] Run at least one computation
  [ ] Return cleanly to Program app menu
  [ ] Close program app, return to home
```

### Phase 2: Prepare Calculator State for Export

**Step 2.1: Clear Non-essential Data**
```
Optional - only if you want "clean" reference:
  [ ] Delete any previous test programs
  [ ] Delete any temporary system files
  [ ] Create clean system state
  
Note: Purpose is to have known reference point
```

**Step 2.2: Verify Program List Final**
```
On FX-CP400, in Program app:
  [ ] Count programs: Should have exactly 9-11 programs
  [ ] List them:
      - MN_MAIN
      - MN_LIN
      - MN_TRI
      - LIN_GRAD
      - TRI_PYTH
      - CFORMAT
      - CVALID
      - CRESULT (computed form of RESVAL distribution,optional for first)
      - CCONST
      - CERROR
  
  [ ] No syntax indicator warnings
  [ ] All programs execute without immediate errors
```

### Phase 3: Export Calculator State to VCP

**Step 3.1: Open ClassPad Manager**
```
On computer:
  [ ] Launch ClassPad Manager application
  [ ] Connect FX-CP400 via USB (if hardware)
      OR verify emulator is running (if emulator)
```

**Step 3.2: Navigate Export Menu**
```
In ClassPad Manager:
  [ ] File → Device Setup OR
  [ ] Device Management - select FX-CP400
  [ ] Select "Export" or "Backup"
  [ ] Choose Export Type:
      - Select "Full Memory" or "Complete Calculator"
      - NOT "Program only"
      - NOT "Variables only"
```

**Step 3.3: Select Output Format**
```
Export Options:
  [ ] Choose format: VCP
      OR .g1m (varies by manager version)
  
Note: VCP is preferred for FX-CP400
      g1m is older/may work too
      
Ask: Which format does your Manager version support?
```

**Step 3.4: Set Export Filename**
```
Filename: MathKit_Test_v0_2_0_Golden.vcp
Location: release/mathkit_test_v0_2_0_golden.vcp

Convention:
  - Name: Project + Version + Purpose
  - Version: Match your current version (v0.2.0)
  - Purpose: "Golden" = reference/baseline build
```

**Step 3.5: Annotate Export**
```
Optional - if Manager allows comments:
  [ ] Date: 2026-05-13
  [ ] Programs: 9 core + 2 test
  [ ] Hardware: FX-CP400 emulator (if applicable)
  [ ] Firmware: [version if known]
  [ ] Verified: Yes - all tests passed
```

**Step 3.6: Execute Export**
```
In ClassPad Manager:
  [ ] Click "Export" button
  [ ] Wait for completion (~10-30 seconds typical)
  [ ] Verify success message shown
  [ ] Confirm file created: release/mathkit_test_v0_2_0_golden.vcp
  
File size should be ~100-200 KB (typical)
If <50 KB: May be incomplete
If >500 KB: May include extra system data (ok but large)
```

### Phase 4: Verify Exported VCP

**Step 4.1: Check File Properties**
```
Windows Command Prompt / Linux Terminal:
  dir release/mathkit_test_v0_2_0_golden.vcp           (Windows)
  ls -lh release/mathkit_test_v0_2_0_golden.vcp        (Linux/Mac)
  
Expected Output:
  Size: ~100-300 KB
  Date: Today's date
  
If size <50 KB: Likely incomplete export, retry
If size >500 KB: Unusual but may include extras, ok for golden
```

**Step 4.2: Verify VCP Format**
```
Check file type:
  file release/mathkit_test_v0_2_0_golden.vcp
  
Expected: Binary file, .vcp format indicator

Note: VCP is binary, not viewable as text
```

**Step 4.3: Create Metadata File**
```
Filename: release/mathkit_test_v0_2_0_golden.vcp.meta

Content:
---
VCP Name: MathKit_Test_v0_2_0_Golden
Date Created: 2026-05-13
Source:
  - Hardware: FX-CP400 (emulator)
  - Programs: 9 core programs from calculator_import/test_build/
  - Firmware: [version if known]

Contents:
  - MN_MAIN (master menu)
  - MN_LIN (linear menu)
  - MN_TRI (trig menu)
  - LIN_GRAD (gradient computation)
  - TRI_PYTH (pythagorean computation)
  - CFORMAT (shared formatting)
  - CVALID (shared validation)
  - CRESULT (shared result display)
  - CCONST (shared constants)
  - CERROR (shared error handling)

Tests Passed: real_hardware_test.md (6/7 categories)
Verified By: [Your Name]
Status: GOLDEN VERSION - REFERENCE BUILD

Import Instructions:
1. Connect FX-CP400 via USB
2. Open ClassPad Manager
3. File → Import → Select this VCP
4. Confirm overwrite when prompted
5. Run MN_MAIN to test success

Notes:
- This is the FIRST VERIFIED build
- Use as baseline for future releases
- Document any issues for v0.3.0 planning

---
```

### Phase 5: Create Release Documentation

**Step 5.1: Create Release Notes**
```
File: release/mathkit_test_v0_2_0_RELEASE_NOTES.md

Content:
---
# MathKit Test Build v0.2.0 - Golden Release

**Date**: 2026-05-13  
**Format**: VCP (Full calculator memory package)  
**Hardware**: CASIO ClassPad II FX-CP400

## What's Included

### Programs (9 total)

**Navigation Menus**:
- MN_MAIN: Master menu with Linear, Trig, Exit options
- MN_LIN: Linear algebra menu
- MN_TRI: Trigonometry menu

**Computations**:
- LIN_GRAD: Linear gradient (slope) calculator
- TRI_PYTH: Pythagorean theorem (hypotenuse) calculator

**Shared Services**:
- CFORMAT: Display formatting and title handling
- CVALID: Input validation dispatcher
- CRESULT: Result rendering and output formatting
- CCONST: Global constants (π as 3.14159265359)
- CERROR: Error message display

### Test Results

Real hardware testing completed:
- ✓ Parser validation (Prog, Menu, Lbl, Goto all work)
- ✓ Navigation and control flow (clean returns)
- ✓ Computation and variables (correct math results)
- ✓ Error handling (validation and recovery)
- ✓ Multiple executions (no state corruption)

### Known Limitations

1. **Unicode Symbols**: 
   - π (pi) imported as numeric constant 3.14159265359
   - √ (sqrt) implemented as ^0.5 exponent
   - Status: Works but display shows alternate notation

2. **Program Calls**:
   - Uses Prog "NAME" syntax (NEWLY CONFIRMED ✓)
   - Subroutine nesting 3+ levels verified working

3. **Feature Set**:
   - Minimal test build (2 modules only)
   - Full 8-module release planned for v0.3.0

## Installation

### Via ClassPad Manager (Recommended)

1. Connect FX-CP400 to computer via USB
2. Open ClassPad Manager
3. File → Import
4. Select: mathkit_test_v0_2_0_golden.vcp
5. Confirm: "Replace existing programs? Yes"
6. Wait for import to complete
7. Disconnect USB

### On Calculator

1. Press MENU to go to home
2. Open Program app
3. Scroll to MN_MAIN
4. Press EXE to run
5. Navigate with arrow keys
6. Try LINEAR → GRADIENT
7. Try TRIG → PYTH

## Support

Issue? Check:
- real_hardware_test.md: Step-by-step testing guide
- calculator_import/test_build/docs/DEPLOYMENT.md: Detailed import help

## Next Release (v0.3.0)

Planned additions:
- Full 8-module support (Algebra, Finance, Stats, Geometry, Measurement, Utility, Settings)
- Enhanced graphics rendering (if hardware permits)
- Performance optimization
- Extended variable system

---
```

### Phase 6: Backup and Archive

**Step 6.1: Create Backup Copy**
```
In release/ directory:
  
  Copy:
    mathkit_test_v0_2_0_golden.vcp
  To:
    backups/mathkit_test_v0_2_0_golden_BACKUP.vcp
  
Purpose: Keep known-good reference
```

**Step 6.2: Archive Original Source**
```
In calculator_import/test_build/:

Create directory: archive/
Copy:
  source/ → archive/mathkit_test_v0_2_0_source/
  
Purpose: Preserve exact source used for golden VCP
```

---

## Validation: Test First Golden VCP

### Test 1: Import to Fresh Calculator

**Procedure**:
```
1. Reset FX-CP400 to factory state (if possible)
2. Connect to ClassPad Manager
3. Import release/mathkit_test_v0_2_0_golden.vcp
4. Run MN_MAIN on calculator
5. Navigate: MN_MAIN → LINEAR → GRADIENT
6. Input: 0, 0, 1, 1
7. Verify: Output shows GRAD=1
8. Return to menu
9. Test: MN_MAIN → TRIG → PYTH
10. Input: 3, 4
11. Verify: Output shows HYP=5
12. Return successfully
```

**Result**: ✓ PASS if all steps complete without errors

### Test 2: Import to Already-Loaded Calculator

**Procedure**:
```
1. Keep existing MathKit programs on calculator
2. Import golden VCP again (will overwrite)
3. Run MN_MAIN
4. Verify no issues, clean operation
```

**Result**: ✓ PASS if import doesn't corrupt or conflict

---

## Documentation: Golden VCP Process

### What NOT to Automate (Why)

**Do NOT automate export** because:
1. Manual verification catches issues before release
2. Human judgment needed for "ready to release"
3. Tool versions vary (need to document exact steps)
4. Failure recovery requires human intervention

### What COULD Automate (Later)

**In v0.4.0+**:
```
Automate:
  [ ] File size validation
  [ ] Format verification
  [ ] Backup copy creation
  [ ] Metadata generation
  [ ] Release notes creation
  
Manual steps preserved:
  [ ] Calculator state verification
  [ ] Export command
  [ ] Approval/sign-off
```

---

## File Locations & Naming

### Convention

```
release/mathkit_[type]_v[VERSION]_[status].vcp

Examples:
  - mathkit_test_v0_2_0_golden.vcp (first golden)
  - mathkit_test_v0_2_1_patch.vcp (bug fix)
  - mathkit_full_v1_0_0_release.vcp (production v1)
  
Metadata:
  - mathkit_test_v0_2_0_golden.vcp.meta (properties)
  - mathkit_test_v0_2_0_RELEASE_NOTES.md (details)
```

---

## Next Phase: After Golden VCP

### Immediate (This Week)
1. ✓ Create and document golden VCP
2. ✓ Test import and basic functionality
3. ✓ Archive source and VCP for reference

### Short-term (Next Month)
1. ⏳ Expand to full 8-module build
2. ⏳ Create v0.3.0 golden VCP
3. ⏳ Plan bulk classroom deployment

### Medium-term (Next Quarter)  
1. ⏳ Automate VCP creation
2. ⏳ Create CI/CD pipeline
3. ⏳ Plan v1.0.0 production release

---

## References

- DEPLOYMENT.md: Import procedures
- real_hardware_test.md: Verification checklist
- release/mathkit_test_v0_2_0_golden.vcp: The actual VCP file
- release/mathkit_test_v0_2_0_RELEASE_NOTES.md: Release documentation

---

## Status

**Current**: Documented and ready for manual execution  
**Next**: Execute when first real import confirmed successful

---

**WARNING**: Do NOT release VCP to users until real_hardware_test.md confirms ✓ PASS.

Golden VCP is for internal reference and classroom testing only.
