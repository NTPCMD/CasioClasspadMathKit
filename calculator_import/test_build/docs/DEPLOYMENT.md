# DEPLOYMENT.md - MathKit First Real Test Build

**Status**: FIRST REAL HARDWARE-IMPORTABLE BUILD  
**Target**: CASIO ClassPad II FX-CP400  
**Format**: XCP (modular per-program)  
**Date**: 2026-05-13

---

## DEPLOYMENT WORKFLOW

This document provides EXACT step-by-step instructions to deploy the test build to FX-CP400.

### OPTION A: Via ClassPad Manager (RECOMMENDED FOR FIRST TEST)

**Prerequisite**: ClassPad Manager installed with FX-CP400 emulator support

#### Step 1: Launch ClassPad Manager
```
1. Open ClassPad Manager application
2. Create NEW project or open existing
3. Locate Program application/editor
```

#### Step 2: Import First Program (MN_MAIN)
```
1. In Program editor: Create NEW program
2. Name: MN_MAIN (exactly 8 chars)
3. Open source/MN_MAIN.txt in text editor
4. Copy entire content (all lines)
5. Paste into Manager program editor
6. Save program object
7. Click EXE to test syntax - should show menu
8. Confirm no errors before continuing
```

#### Step 3: Import Menu Modules (MN_LIN, MN_TRI)
```
Repeat Step 2 for each:
- source/MN_LIN.txt → create program MN_LIN
- source/MN_TRI.txt → create program MN_TRI
```

#### Step 4: Import Computation Program (LIN_GRAD)
```
1. Create NEW program: LIN_GRAD
2. Paste source/LIN_GRAD.txt
3. Save
4. Note: Do NOT test-run yet (requires sub-programs loaded)
```

#### Step 5: Import Computation Program (TRI_PYTH)
```
1. Create NEW program: TRI_PYTH
2. Paste source/TRI_PYTH.txt
3. Save
4. Note: Do NOT test-run yet (requires sub-programs loaded)
```

#### Step 6: Import Core Support Programs (8 files total)
```
Import these WITH EXACT NAMES (no shortcuts):

1. CCONST   ← source/CCONST.txt
2. CFORMAT  ← source/CFORMAT.txt
3. CVALID   ← source/CVALID.txt
4. CRESULT  ← source/CRESULT.txt
5. CERROR   ← source/CERROR.txt
```

#### Step 7: Verify All Programs Loaded
```
In Program editor list:
- Confirm ALL 11 programs appear:
  MN_MAIN, MN_LIN, MN_TRI,
  LIN_GRAD, TRI_PYTH,
  CCONST, CFORMAT, CVALID, CRESULT, CERROR

Do NOT proceed if any missing.
```

#### Step 8: Test MN_MAIN in Manager Emulator
```
1. Double-click MN_MAIN to run
2. Should see:
   ================
   MATHKIT
   ================
   
   EXE=OK  EXIT=BACK
3. Press EXE to open menu
4. Should show:
   - LINEAR
   - TRIG
   - EXIT
5. Press EXE on LINEAR → should show GRADIENT option
6. Press EXE on TRIG → should show PYTH option
7. Return to main menu (EXIT)
8. Test complete - if you see menus, parser works!
```

#### Step 9: Export to XCP Format
```
1. Select ALL 11 programs (Ctrl+A in program list)
2. Right-click → Export/Save
3. File type: Select "XCP" format
4. Name: mathkit_test_v0.2.0.xcp
5. Location: calculator_import/test_build/converted/
6. Save
```

#### Step 10: Hardware Import (USB Connection)
```
1. Connect FX-CP400 to computer via USB cable
2. Put calculator in USB storage mode:
   - On calculator: Press MENU
   - Navigate to System settings
   - Select USB communication
   - Select "Mass Storage" mode
3. On computer: Locate FX-CP400 as USB drive
4. Open ClassPad transfer utility (part of Manager)
5. File → Import/Send → Select converted/mathkit_test_v0.2.0.xcp
6. Send to device
7. Wait for transfer complete (should show ✓)
8. Safely eject USB device
```

#### Step 11: Launch on Calculator
```
1. Disconnect USB
2. On calculator: Press MENU
3. Open Program app
4. Scroll to MN_MAIN
5. Press EXE
6. Should see MathKit menu
7. Navigate: LINEAR → GRADIENT (test for crashes)
8. If menu appears and you return safely, FIRST TEST PASSES!
```

---

### OPTION B: Direct txt2xcp Conversion (EXPERIMENTAL)

**Prerequisite**: txt2xcp utility available on PATH

#### Step 1: Locate txt2xcp
```bash
# Verify txt2xcp is available
which txt2xcp
```

#### Step 2: Convert Source Files
```bash
cd /workspaces/CasioClasspadMathKit/calculator_import/test_build/source

# Convert all .txt to .xcp
for file in *.txt; do
  txt2xcp "$file" "${file%.txt}.xcp"
done

# Result: *.xcp files created
```

#### Step 3: Verify Conversions
```bash
# Check output
ls -lh *.xcp

# Should see 11 .xcp files, ~1-2KB each
```

#### Step 4: Transfer to Hardware
```
1. Connect FX-CP400 via USB
2. Put in Mass Storage mode
3. Copy all .xcp files to calculator
4. (Exact menu path TBD per ClassPad manager version)
5. Import each .xcp
```

#### Step 5: Test on Calculator
See Section "Option A, Step 11" for testing procedure.

---

### OPTION C: ASCII-Safe Fallback (If Unicode Transfer Fails)

If Option A fails with Unicode errors:

#### Step 1: Use ASCII Versions
```
1. Instead of source/*.txt
2. Use ascii_safe/*.txt
3. These have NO Unicode symbols (no √, π)
```

#### Step 2: Repeat Import
```
Follow Option A (Steps 2-11)
BUT use files from ascii_safe/ directory
```

#### Step 3: Expected Behavior
```
- Menu display: identical
- Math display: Output shows "(A^2+B^2)^0.5" instead of "√"
- Pi display: Shows "*PI" instead of "π"
- Functionality: Unchanged (same results)
```

#### Step 4: Document Issue
```
If ASCII fallback works but Unicode fails:
1. Note in docs/unicode_transfer_issues.md
2. Record calculator model/firmware version
3. Plan Unicode fix for next release
```

---

## ROLLBACK PROCEDURE

If import breaks calculator state:

### Immediate Steps
```
1. Disconnect FX-CP400 from computer
2. On calculator: Press MENU → Program
3. Scroll to MN_MAIN
4. Press DEL to remove
5. Repeat for all imported programs
```

### Via Backup
```
1. Before import, export calculator state:
   - Calculator: Memory → Backup → Export (creates .g1m)
   - Save as: calculator_import/backups/mathkit_pretest_backup.g1m
   
2. If import fails:
   - Connect FX-CP400
   - Open ClassPad transfer utility
   - Import saved backup .g1m
   - Transfers entire calculator state back
```

### Full Reset (Last Resort)
```
1. Connect FX-CP400
2. Run factory reset from System menu
3. This erases ALL programs and data
4. Restores calculator to factory state only
```

---

## TROUBLESHOOTING

### Problem: "Prog not Found" Error When Running MN_MAIN

**Cause**: Sub-program not loaded to calculator  
**Solution**:
```
1. Verify ALL 11 programs imported (check Program list)
2. If any missing, import from source/ directory
3. Try running MN_MAIN again
```

### Problem: Menu Doesn't Display Correctly

**Cause**: Unicode symbols not transferred properly  
**Solution**:
```
1. Delete all programs from calculator
2. Re-import using ascii_safe/ versions instead
3. Retry import workflow
```

### Problem: "Invalid Syntax" Error During Import

**Cause**: Parser doesn't recognize certain syntax  
**Solution**:
```
1. Note which program caused error
2. Document error in tests/parser_issues.md
3. Try alternative from ascii_safe/ if available
4. Report to development team
```

### Problem: Calculator Freezes After Running Program

**Cause**: Likely infinite loop or missing Return statement  
**Solution**:
```
1. Press [ESC] multiple times to stop execution
2. Return to calculator main menu
3. Open Program and delete the offending program
4. Check source file for logic errors
5. Report to development team
```

---

## VERIFICATION CHECKLIST

After import to hardware, verify:

- [ ] All 11 programs appear in Program list
- [ ] MN_MAIN runs without "Prog not found" errors
- [ ] Menu structure displays (LINEAR, TRIG, EXIT visible)
- [ ] Can navigate: MN_MAIN → LINEAR → GRADIENT
- [ ] Can navigate: MN_MAIN → TRIG → PYTH
- [ ] EXIT returns without freezing
- [ ] Multiple menu traversals don't corrupt state
- [ ] Input prompts appear correctly
- [ ] Pause displays and responds to EXE key
- [ ] Program exits cleanly with Return

If ALL checks pass: **FIRST REAL IMPORT TEST SUCCESSFUL** ✓

---

## NEXT STEPS

After successful first import:

1. Run full hardware test checklist:
   - See [../../tests/real_hardware_test.md](../../tests/real_hardware_test.md)

2. Document findings:
   - Unicode transfer behavior
   - Prog "NAME" syntax confirmation
   - Menu parsing status
   - Variable persistence

3. Plan next release:
   - Full MathKit module build (all 8 topics)
   - Hardware optimization
   - Performance profiling

---

## FILE LOCATIONS

| Item | Path |
|------|------|
| Source programs | `source/` (9 files) |
| ASCII fallback | `ascii_safe/` (2 files) |
| Converted XCP | `converted/` (after export) |
| Backups | `../../backups/` |
| Test results | `../../tests/` |

---

## SUPPORT

For questions:
1. Check [../../docs/import_pipeline.md](../../docs/import_pipeline.md) for general info
2. Check [../../docs/classpad_syntax.md](../../docs/classpad_syntax.md) for syntax reference
3. Document issues in `./docs/` for team review
4. Escalate persistent problems to development team

---

**Status: READY FOR FIRST HARDWARE TEST**
