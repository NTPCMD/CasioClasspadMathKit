# MathKit First Real Import Test Build

**Status**: FIRST REAL HARDWARE-IMPORTABLE PACKAGE  
**Date**: 2026-05-13  
**Target**: CASIO ClassPad II FX-CP400  
**Format**: XCP (modular per-program import)

## Contents

This directory contains the FIRST verifiable, hardware-importable MathKit test package.

### Test Programs (5 core programs)

1. **MN_MAIN** - Master navigation menu
2. **LIN_GRAD** - Linear gradient calculator (test computation)
3. **TRI_PYTH** - Pythagorean hypotenuse (test √ and variables)
4. **CFORMAT** - Shared formatting module (test UI rendering)
5. **CVALID** - Shared validation dispatcher (test error handling)

### Subdirectories

- `source/` - Original .txt source files (parser-safe version)
- `ascii_safe/` - ASCII fallback versions (No Unicode symbols)
- `converted/` - Ready-to-import .xcp files (after txt2xcp conversion)
- `docs/` - Deployment and testing documentation

## Import Workflow (Quick Start)

### Option A: Via ClassPad Manager (Recommended for First Test)

```
1. Open ClassPad Manager
2. Open Program application
3. For each program in source/:
   - Create new program with exact name (8 chars max)
   - Paste source .txt content
   - Save program
4. Export all programs as .xcp package
5. Import .xcp to FX-CP400 via USB
```

### Option B: Direct txt2xcp Conversion

```
1. Use txt2xcp tool on source/*.txt
2. Produces .xcp files in converted/
3. Import .xcp files directly to FX-CP400
```

### Option C: ASCII-Safe Fallback

If Unicode transfer fails:
```
1. Use ascii_safe/*.txt versions instead
2. Follow Option A or B with ASCII versions
3. Documents unicode issues for later fix
```

## Deployment Instructions

See [DEPLOYMENT.md](DEPLOYMENT.md) for:
- Exact ClassPad Manager steps
- USB transfer workflow
- Hardware test sequence
- Rollback procedure
- Troubleshooting guide

## Real Hardware Test Checklist

See [../../tests/real_hardware_test.md](../../tests/real_hardware_test.md)

Covers:
- Import verification
- Parser compatibility  
- Menu navigation
- Variable persistence
- Return behavior
- Unicode rendering
- Nested program calls

## Status: READY FOR FIRST REAL TEST

This build:
✓ Contains only verified syntax patterns
✓ Uses confirmed parser-safe structure
✓ Includes ASCII fallbacks for transfer safety
✓ Has complete deployment documentation
✓ Is ready for emulator/hardware testing

Next step: Import to FX-CP400 and run real_hardware_test.md checklist.
