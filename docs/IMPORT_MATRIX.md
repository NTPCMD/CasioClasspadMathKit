# Import Method Matrix & Recommendations

**Status**: RESEARCH COMPLETE  
**Date**: 2026-05-13  
**Target**: CASIO ClassPad II FX-CP400

---

## Executive Summary

| Method | Recommended | Purpose | Status |
|--------|-------------|---------|--------|
| Raw paste (text) | ❌ NO | Testing only | Simple but unreliable |
| Program(Text) | ⚠ MAYBE | Modular testing | Not yet verified |
| txt2xcp | ⏳ LATER | Automated pipeline | Promising, not yet tested |
| XCP import | ✓ YES | First real test | RECOMMENDED NOW |
| VCP AutoImport | ✓ YES | Classroom deploy | Ready after XCP success |
| Emulator-only | ✓ YES | Development/testing | Always available |

---

## Detailed Comparison

### 1. Raw Text Paste (Drag-and-Drop)

```
Workflow:
  editor (program.txt)
    ↓ copy-paste
  Calculator app → Paste → Run
```

**Pros**:
- ✓ Immediate feedback
- ✓ No tools needed
- ✓ Direct control

**Cons**:
- ✗ ASCII only (Unicode corruption likely)
- ✗ Limited program size (~1000 chars typical)
- ✗ Parser hits maximum buffer
- ✗ No token preservation
- ✗ Requires manual entry for each line

**Use Case**: 
- Quick testing of single 10-line function
- NOT suitable for real programs

**Recommendation**: ❌ SKIP
- Too unreliable for MathKit (programs 30-50 lines each)

---

### 2. Program(Text) Mode

```
Claimed Workflow (from CASIO docs):
  Calculator app:
    1. Open program in "Text" mode
    2. Import structured text object
    3. Auto-convert to program
    4. Switch to "Normal" execution mode
```

**Pros**:
- ✓ Official CASIO documentation mentions it
- ✓ Would preserve structure if it works
- ✓ No external tool needed

**Cons**:
- ⚠ Method not yet verified on real hardware
- ⚠ Exact menu path unclear
- ⚠ May fail silently

**Use Case**:
- Backup plan if XCP fails
- Modular import of individual programs

**Recommended**: ⚠ MAYBE (for investigation post-XCP)

**Follow-up Action**: 
1. If XCP succeeds: Good fallback method
2. If XCP fails: Try this immediately

---

### 3. txt2xcp Conversion

```
Workflow:
  program.txt (source)
    ↓ txt2xcp tool
  program.xcp (binary container)
    ↓ FX-CP400 import
  Calculator app → Run
```

**Pros**:
- ✓ Automated batch processing possible
- ✓ Saves manual ClassPad Manager entry time
- ✓ Supports all syntax forms properly
- ✓ One-command deployment potential

**Cons**:
- ⚠ Requires txt2xcp tool (may not be installed)
- ⚠ First hardware test not yet done
- ⚠ Potential Unicode issues (same as txt source)
- ⚠ Error handling if conversion fails

**Use Case**:
- Batch conversion of multiple programs
- Automated release builds (v0.4.0+)
- Developer workflow after verification

**Recommended**: ⏳ LATER (investigate after first XCP test)

**Implementation Ready**: ✓ See build/txt2xcp_pipeline/

---

### 4. XCP Direct Import

```
Workflow:
  ClassPad Manager (or txt2xcp)
    ↓ creates program.xcp
  FX-CP400 USB transfer
    ↓ ClassPad transfer utility
  Calculator Program app
    ↓ Run program normally
```

**Pros**:
- ✓ Official CASIO import format
- ✓ Full syntax support (no corruption risk)
- ✓ Per-program modularity
- ✓ Easy to test individual programs
- ✓ Flexible: add/remove programs without risk

**Cons**:
- ✗ Requires all referenced programs imported first
  (e.g., LIN_GRAD needs CFORMAT, CVALID, CRESULT imported)
- ✗ Must follow import order
- ✗ If one program missing, call fails

**Use Case**:
- ✓ RECOMMENDED for first real test (NOW)
- ✓ Best for modular development
- ✓ Testing individual modules

**Recommended**: ✓ YES (DO THIS FIRST)

**Status**: Ready to deploy via DEPLOYMENT.md

---

### 5. VCP Full Import (AutoImport)

```
Workflow:
  ClassPad Manager (emulator state)
    ↓ export complete calculator
  calculator.vcp (full state)
    ↓ FX-CP400 USB transfer  
  AutoImport process
    ↓ Bulk install entire system
  Calculator ready to use
```

**Pros**:
- ✓ One-file deployment (single VCP)
- ✓ Entire system in known state
- ✓ Perfect for classroom rollout
- ✓ No "missing program" risk
- ✓ Fastest deployment (no per-program wait)

**Cons**:
- ✗ Overwrites entire calculator memory
- ✗ Loses any existing user programs
- ✗ Backup needed before import
- ✗ "Golden" state must be verified first
- ✗ Cannot easily add/remove programs after
- ✗ Large file (~100-500 KB)

**Use Case**:
- ✓ Classroom bulk deployment
- ✓ "Fresh start" distribution
- ✓ Reference golden builds

**Recommended**: ✓ YES (after XCP test succeeds)

**Status**: Documentation ready (GOLDEN_VCP_PLAN.md)  
Execution: After first real import confirmed working

---

### 6. Emulator-Only (No Hardware)

```
Workflow:
  ClassPad Manager emulator
    ↓ create programs
    ↓ run/test
  No hardware required
```

**Pros**:
- ✓ Works immediately
- ✓ No USB/hardware hassles
- ✓ Quick iteration (good for dev)
- ✓ Perfect for "first test"

**Cons**:
- ✗ NOT real hardware
- ✗ Potential emulator quirks
- ✗ May not catch hardware-specific issues
- ✗ Not suitable for final validation

**Use Case**:
- ✓ Development and testing
- ✓ Parser verification
- ✓ Pre-flight check before hardware

**Recommended**: ✓ YES (as precursor to hardware test)

**Status**: Emulator testing already done

---

## Decision Tree: Which Method to Use?

```
                        FIRST REAL IMPORT?
                               ↓
                    ┌─────────────────────┐
                    │  YES (NOW)          │
                    └─────────────────────┘
                               ↓
                    Do you have hardware?
                         /          \
                       YES          NO
                        ↓            ↓
                   Use XCP    Use Emulator
                     +         Test cycle
                  DEPLOY.md
                     ↓
            (After success)
                     ↓
          Create VCP for classroom?
               /            \
             YES            NO
              ↓              ↓
         GOLDEN_VCP   Continue XCP
           PLAN.md     for per-program
                      modular updates
```

---

## Recommendation Timeline

### Phase 1: NOW (Prove End-to-End)

**Goal**: First successful import to FX-CP400

**Method**: XCP Import  
**Steps**: calculator_import/test_build/docs/DEPLOYMENT.md  
**Validation**: tests/real_hardware_test.md  
**Duration**: 1-2 hours

```
1. Import 11 programs to FX-CP400 via ClassPad Manager
2. Run MN_MAIN successfully
3. Navigate menus and run LIN_GRAD, TRI_PYTH
4. Document any issues
5. If all pass: Proceed to Phase 2
```

---

### Phase 2: WEEK 1 (Prove Reproducibility)

**Goal**: Verify golden state is repeatable

**Method**: VCP Full Import  
**Steps**: release/GOLDEN_VCP_PLAN.md  
**Status**: Manual execution, documented

```
1. Export working calculator as VCP
2. Create RELEASE_NOTES.md
3. Test import to fresh calculator
4. Document import procedure
```

---

### Phase 3: MONTH 1 (Automate Pipeline)

**Goal**: Batch deployment ready

**Method**: txt2xcp Conversion  
**Steps**: build/txt2xcp_pipeline/README.md  
**Status**: Documentation ready, tool verification pending

```
1. Verify txt2xcp availability
2. Test batch conversion
3. Compare with ClassPad Manager results
4. Document differences
5. Plan integration into build system
```

---

### Phase 4: MONTH 3 (Release Pipeline)

**Goal**: CI/CD integration

**Method**: Hybrid (txt2xcp + VCP export)  
**Steps**: Automate previous manual steps

```
1. Trigger: make build-release
2. Step 1: txt2xcp all *.txt → *.xcp
3. Step 2: Import all XCP to emulator
4. Step 3: Run test suite
5. Step 4: Export emulator state as VCP
6. Step 5: Package release artifacts
```

---

## Contingency Plans

### If XCP Import Fails

```
Fallback 1: Try Program(Text) mode
- Use DEPLOYMENT.md "Option B" instructions
- May require different menu path per firmware

Fallback 2: Try raw text paste
- Not recommended, likely to fail
- Good for single 10-line functions only

Fallback 3: Manual ClassPad Manager creation
- Create each program manually by typing
- Last resort (very time-consuming)
```

### If VCP Export Fails

```
Fallback 1: Continue with XCP modules
- Can deploy each module individually
- Takes longer but works
- Still classroom-ready

Fallback 2: Wait for txt2xcp
- May automate what VCP export does
- Alternative deployment method
```

### If txt2xcp Unavailable

```
Fallback 1: Use ClassPad Manager for all builds
- Manual but reliable
- Already documented
- No external tools needed

Fallback 2: Seek source/alternative
- Check CASIO GitHub
- Community forums
- Build alternative if needed (low priority)
```

---

## Summary Table: Choose Your Path

```
┌──────────────────────────────────────────────────────────────┐
│                  IMPORT METHOD SELECTION                     │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  "I want to test TODAY"                                      │
│  → Use XCP Import (DEPLOYMENT.md Option A)                   │
│  → Ready NOW ✓                                               │
│                                                               │
│  "I want emulator testing first"                             │
│  → Use Emulator (ClassPad Manager built-in)                  │
│  → Then proceed to XCP                                       │
│                                                               │
│  "I want to deploy to classrooms"                            │
│  → Use VCP AutoImport (after XCP verified)                   │
│  → See GOLDEN_VCP_PLAN.md                                    │
│                                                               │
│  "I want automated batch builds"                             │
│  → Plan txt2xcp (see txt2xcp_pipeline/)                      │
│  → Ready to test NEXT MONTH                                  │
│                                                               │
│  "I want simple one-off testing"                             │
│  → Use raw text paste for <1000 char snippets               │
│  → NOT for MathKit programs (too large)                      │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Verification Checklist

After choosing your method:

- [ ] Method selected from table above
- [ ] Documentation for method reviewed
- [ ] Prerequisites checked
- [ ] Hardware/emulator ready
- [ ] Programs imported successfully
- [ ] MN_MAIN runs without errors
- [ ] Test programs compute correctly (LIN_GRAD, TRI_PYTH)
- [ ] Returns and exits work cleanly
- [ ] Next phase planned

---

## References

| Document | Purpose |
|----------|---------|
| [DEPLOYMENT.md](../calculator_import/test_build/docs/DEPLOYMENT.md) | XCP import steps |
| [GOLDEN_VCP_PLAN.md](./GOLDEN_VCP_PLAN.md) | VCP creation & export |
| [real_hardware_test.md](../tests/real_hardware_test.md) | Verification tests |
| [txt2xcp_pipeline/](../build/txt2xcp_pipeline/) | Automated conversion |
| [classpad_syntax.md](../docs/classpad_syntax.md) | Parser reference |

---

## Status

**This Matrix**: Documentation complete ✓  
**Recommended Path**: XCP → VCP → txt2xcp  
**First Step**: DEPLOYMENT.md Option A (XCP import NOW)

---

**Ready to begin? Start with [DEPLOYMENT.md](../calculator_import/test_build/docs/DEPLOYMENT.md)**
