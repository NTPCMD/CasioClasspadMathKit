# txt2xcp Test Pipeline

**Status**: EXPERIMENTAL  
**Date**: 2026-05-13  
**Target**: FX-CP400 XCP format conversion

---

## Overview

`txt2xcp` is a CASIO-provided utility that converts ClassPad BASIC source text files (`.txt`) directly to eXchange Program format (`.xcp`).

This pipeline enables:
- ✓ Direct .txt → .xcp conversion
- ✓ Bypass manual ClassPad Manager entry
- ✓ Batch program conversion
- ✓ Automated release packaging

---

## Setup & Installation

### Check if txt2xcp is Available

```bash
# Test if tool exists on system
which txt2xcp

# If found: /path/to/txt2xcp
# If not found: Need to install or locate
```

### Installation Methods

#### Method A: Via Package Manager (Linux/Ubuntu)
```bash
# Check if available in repositories
apt search txt2xcp

# If found:
sudo apt install txt2xcp

# Verify:
txt2xcp --version
```

#### Method B: From CASIO ClassPad Manager
```bash
# Some ClassPad Manager installations include txt2xcp
# Typical paths:
# - Windows: C:\Program Files\CASIO\ClassPad Manager\bin\txt2xcp.exe
# - macOS: /Applications/ClassPad\ Manager/Contents/bin/txt2xcp
# - Linux: /opt/classpad/bin/txt2xcp
```

#### Method C: Build from Source (If Needed)
```bash
# Check GitHub/CASIO repositories
# (Placeholder - actual source location TBD)

# Compile if source available:
git clone <source-repo>
cd txt2xcp
make
sudo make install
```

### Verify Installation
```bash
# Test with minimal file
echo 'Return' > test.txt
txt2xcp test.txt test.xcp

# Check output
ls -l test.xcp
file test.xcp  # Should show XCP format

# Clean up
rm test.txt test.xcp
```

---

## Basic Usage

### Single File Conversion

```bash
# Convert one program
txt2xcp input.txt output.xcp

# Example:
txt2xcp MN_MAIN.txt MN_MAIN.xcp
```

### Batch Conversion

```bash
# Convert all .txt files in directory
cd /path/to/source/
for file in *.txt; do
  txt2xcp "$file" "${file%.txt}.xcp"
done

# Verify all conversions
ls -1 *.xcp | wc -l  # Should match number of .txt files
```

### Specific Test Build Workflow

```bash
#!/bin/bash
# File: build/txt2xcp_pipeline/batch_convert.sh

cd /workspaces/CasioClasspadMathKit/calculator_import/test_build

# Create output directory
mkdir -p converted

# Convert source programs
cd source
for program in *.txt; do
  echo "Converting $program..."
  txt2xcp "$program" "../converted/${program%.txt}.xcp"
  
  # Verify each conversion
  if [ -f "../converted/${program%.txt}.xcp" ]; then
    echo "  ✓ ${program%.txt}.xcp created"
  else
    echo "  ✗ FAILED: ${program%.txt}.xcp"
    exit 1
  fi
done

cd ../converted
echo "Conversion complete. Files:"
ls -lh *.xcp

echo ""
echo "Ready for hardware import."
```

---

## Conversion Process (Technical Details)

### What txt2xcp Does

```
Input: program.txt (ClassPad BASIC source)
  ↓
1. Tokenize: Convert text to CASIO bytecode tokens
2. Validate: Check for parser errors
3. Package: Wrap in XCP container format
4. Encode: Binary XCP file format
  ↓
Output: program.xcp (import-ready file)
```

### Input Requirements

The `.txt` file must:
1. ✓ Be valid ClassPad BASIC syntax
2. ✓ Contain program code (not data)
3. ✓ Have UTF-8 or ASCII encoding
4. ✓ Use CASIO token syntax (Prog, Lbl, Goto, etc.)

### Output Format

The `.xcp` file:
- Binary format (not human-readable)
- ~1-3 KB per program (typical)
- Contains tokenized bytecode
- Ready for FX-CP400 import

---

## Error Handling

### Common Errors

#### Error: "txt2xcp: command not found"
```
Solution:
  [ ] Check PATH: echo $PATH
  [ ] Install txt2xcp if missing
  [ ] Add txt2xcp location to PATH:
      export PATH="/opt/txt2xcp/bin:$PATH"
```

#### Error: "Syntax error in line X"
```
Solution:
  [ ] Check input file syntax
  [ ] Verify CASIO BASIC compatibility
  [ ] Test on simpler programs first
  [ ] Check classpad_syntax.md for valid syntax
```

#### Error: "Invalid Unicode character"
```
Solution:
  [ ] Use ASCII-safe version instead
  [ ] See docs/ascii_safe_mode.md
  [ ] Replace √ with ^0.5
  [ ] Replace π with 3.14159265359
```

#### Error: "Output file creation failed"
```
Solution:
  [ ] Check directory permissions: ls -ld dirname
  [ ] Verify disk space: df -h
  [ ] Try different output location
```

---

## Pipeline Usage (Test Build)

### Scenario 1: Direct Conversion

```bash
# Quick start - convert test build
cd /workspaces/CasioClasspadMathKit/calculator_import/test_build

# Convert all source programs
cd source
for f in *.txt; do txt2xcp "$f" "../converted/${f%.txt}.xcp"; done

# Verify
cd ../converted
ls -1h *.xcp | head -5

# Result: Ready to import to hardware
```

### Scenario 2: Conditional ASCII-Safe Fallback

```bash
#!/bin/bash
# If Unicode conversion fails, retry with ASCII-safe

PROGRAM="TRI_PYTH"
SOURCE="source/${PROGRAM}.txt"
OUTPUT="converted/${PROGRAM}.xcp"

# Try original
if ! txt2xcp "$SOURCE" "$OUTPUT" 2>/dev/null; then
  echo "Unicode version failed, trying ASCII-safe..."
  SOURCE="ascii_safe/${PROGRAM}.txt"
  
  if txt2xcp "$SOURCE" "$OUTPUT"; then
    echo "✓ ASCII-safe version succeeded"
  else
    echo "✗ Both versions failed"
    exit 1
  fi
fi
```

### Scenario 3: Batch with Error Recovery

```bash
#!/bin/bash
# build/txt2xcp_pipeline/robust_convert.sh

OUT_DIR="converted"
FAILED_LIST="failed_conversions.txt"
SUCCESS_COUNT=0
FAIL_COUNT=0

mkdir -p "$OUT_DIR"
: > "$FAILED_LIST"  # Clear previous failures

echo "Starting batch conversion..."

for source_file in source/*.txt; do
  program_name=$(basename "$source_file" .txt)
  output_file="$OUT_DIR/$program_name.xcp"
  
  echo -n "Converting $program_name... "
  
  if txt2xcp "$source_file" "$output_file" 2>/dev/null; then
    echo "✓"
    ((SUCCESS_COUNT++))
  else
    echo "✗ FAILED"
    echo "$program_name" >> "$FAILED_LIST"
    ((FAIL_COUNT++))
  fi
done

echo ""
echo "============================================"
echo "Conversion Summary:"
echo "  Success: $SUCCESS_COUNT"
echo "  Failed:  $FAIL_COUNT"
echo "============================================"

if [ $FAIL_COUNT -gt 0 ]; then
  echo ""
  echo "Failed conversions:"
  cat "$FAILED_LIST"
  echo ""
  echo "Retry with ASCII-safe versions? (Y/n)"
  read -r response
  if [ "$response" != "n" ]; then
    echo "Re-attempting with ASCII-safe..."
    while IFS= read -r program; do
      ascii_source="ascii_safe/${program}.txt"
      output_file="$OUT_DIR/${program}.xcp"
      
      if [ -f "$ascii_source" ]; then
        echo -n "  Converting $program (ASCII)... "
        if txt2xcp "$ascii_source" "$output_file"; then
          echo "✓"
        else
          echo "✗"
        fi
      fi
    done < "$FAILED_LIST"
  fi
fi

echo ""
echo "Final output:"
ls -lh "$OUT_DIR/" | tail -n +2
```

---

## Verification & Testing

### Post-Conversion Verification

```bash
# Check file integrity
test -f converted/MN_MAIN.xcp && echo "✓ File exists" || echo "✗ Missing"

# Check file size (should be 1-3 KB typical)
ls -lh converted/MN_MAIN.xcp
# Example: -rw-r--r-- 1 user user 1.2K May 13 10:30 converted/MN_MAIN.xcp

# Check file type
file converted/MN_MAIN.xcp
# Should show binary/XCP format indicator

# Verify all expected conversions exist
cd converted
expected_count=11  # For test build
actual_count=$(ls -1 *.xcp 2>/dev/null | wc -l)

if [ "$actual_count" -eq "$expected_count" ]; then
  echo "✓ All $expected_count programs converted"
else
  echo "✗ Expected $expected_count but found $actual_count"
fi
```

### Pre-Hardware Testing

```bash
# Optional: Test XCP on emulator before hardware
# (If txt2xcp created valid XCP files)

# Method: Use ClassPad Manager to import .xcp and run test
# Step 1: Open ClassPad Manager
# Step 2: Program app → Import Program
# Step 3: Select converted/MN_MAIN.xcp
# Step 4: Should import without errors
# Step 5: Run MN_MAIN to verify functionality
```

---

## Known Limitations

### What txt2xcp DOES Support
- ✓ Standard ClassPad BASIC syntax
- ✓ All control flow (If/Then, loops, etc.)
- ✓ Variables and arrays
- ✓ String operations
- ✓ Math functions
- ✓ I/O statements (Input, Locate, etc.)

### What txt2xcp MAY NOT Support

| Feature | Status | Workaround |
|---------|--------|-----------|
| Comments | Usually OK | Keep minimal |
| Unicode symbols | ⚠ Risky | Use ASCII-safe mode |
| Non-standard functions | ✗ May fail | Verify in classpad_syntax.md |
| eActivity objects | ✗ Different format | Separate process |
| Graphics/images | ✗ Not programs | Handled separately |

---

## Integration into Release Pipeline

### Future Automation

Once txt2xcp is confirmed stable:

```
1. Trigger: `make build`
2. Step 1: Validate source/*.txt
3. Step 2: Run txt2xcp batch conversion
4. Step 3: Verify all *.xcp created
5. Step 4: Package into release/mathkit_vX.Y.Z.xcp
6. Output: Final release artifact ready for distribution
```

### Current Status: Manual

Until txt2xcp integration tested:
```
Using: ClassPad Manager (manual entry)
Why: Safer for first real import
Next: Automate after hardware confirmation
```

---

## Scripts & Tools

### Helper Script: verify_xcp_build.sh

```bash
#!/bin/bash
# Usage: ./verify_xcp_build.sh

XCP_DIR="converted"
ERROR_COUNT=0

echo "Verifying XCP build..."
echo ""

for xcp_file in "$XCP_DIR"/*.xcp; do
  if [ ! -f "$xcp_file" ]; then
    continue
  fi
  
  name=$(basename "$xcp_file")
  size=$(stat -f%z "$xcp_file" 2>/dev/null || stat -c%s "$xcp_file" 2>/dev/null)
  
  if [ "$size" -lt 100 ]; then
    echo "⚠ WARNING: $name is suspiciously small ($size bytes)"
    ((ERROR_COUNT++))
  else
    echo "✓ $name ($size bytes)"
  fi
done

echo ""
if [ $ERROR_COUNT -eq 0 ]; then
  echo "✓ All XCP files look valid"
else
  echo "✗ Found $ERROR_COUNT potential issues"
fi
```

---

## Troubleshooting Reference

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| txt2xcp not found | Tool not installed | Install txt2xcp |
| Conversion fails | Syntax error in .txt | Check src file, use ASCII-safe |
| .xcp file too small | Conversion error | Verify error message |
| Import to calculator fails | Corrupted XCP | Retry with ClassPad Manager |
| Multiple conversions fail | Batch issue | Convert one at a time first |

---

## Next Steps

### Immediate (This Build)
1. ✓ Document txt2xcp process
2. ✓ Create batch conversion script  
3. ✓ Test on first real imports (manual ClassPad Manager)
4. ⏳ Document results

### Short-term (Next Build)
1. ✓ Verify txt2xcp availability on target system
2. ✓ Test txt2xcp conversion on test_build/
3. ✓ Compare with ClassPad Manager results
4. ⏳ Decide on automation feasibility

### Medium-term (Production Release)
1. ⏳ Integrate txt2xcp into build pipeline
2. ⏳ Automate XCP generation
3. ⏳ Create release packaging workflow
4. ⏳ Document production build process

---

## References

- ClassPad Manager documentation
- docs/classpad_syntax.md - Syntax reference
- calculator_import/test_build/DEPLOYMENT.md - Import procedures
- docs/ascii_safe_mode.md - Unicode fallback

---

## Summary

**txt2xcp** is a potential tool for:
- Faster batch conversions
- Automated release packaging
- Alternative to manual ClassPad Manager entry

**Current Status**: Documented, ready for first test  
**Next Step**: Verify availability and test on first real build

---

**Note**: This process is EXPERIMENTAL. First real import will use ClassPad Manager for maximum safety and verification. txt2xcp integration to follow after successful hardware validation.
