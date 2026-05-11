# FX-CP400 Import/Export Pipeline

## Required tools

- ClassPad Manager for ClassPad II / FX-CP400
- FX-CP400 emulator runtime inside ClassPad Manager (or bundled manager runtime)
- Physical FX-CP400 for final hardware validation

## Canonical source inputs

- Production source: `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/src/**/*.txt`
- Minimal hardware references: `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/minimal_sources/*.txt`

## Exact working workflow

### A. Convert text source into manager program objects

1. Launch **ClassPad Manager**.
2. Open the **Program** application/editor inside Manager.
3. Create a **new program** object.
4. Enter the exact program name from the source filename (must stay <=8 chars).
5. Open the matching `.txt` source file.
6. Copy the source text.
7. Paste the text into the Manager program editor.
8. Save the program object.
9. Re-open the saved program and confirm tokens were preserved.
10. Repeat for every required program.

### B. Emulator validation

1. Run `RFMENU`.
2. Run `RFHELLO`, `RFINPUT`, `RFRES`, and `RFVALID`.
3. Run `MN_MAIN` and at least one leaf program from each populated module.
4. Record any syntax or runtime failure before exporting.

### C. Export package

1. Save the Manager project.
2. Export the validated program set to the ClassPad transfer package format used by Manager.
3. Store the export in `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/release/`.
4. Copy the transfer-ready artifact into `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/calculator_import/`.
5. Copy the previous good package into `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/backups/`.

### D. Calculator import

1. Connect the FX-CP400 by USB.
2. Put the calculator into USB storage / transfer mode.
3. Copy the validated export package using the ClassPad transfer workflow.
4. Safely eject the device.
5. Open/import the package on the calculator.
6. Run the smoke tests from `/home/runner/work/CasioClasspadMathKit/CasioClasspadMathKit/tests/hardware_checklist.md`.

## Supported formats

- `.xcp` = primary ClassPad export/import package
- `.g1m` = not primary for FX-CP400 in this project
- `.c2p` = legacy ClassPad-era package reference
- eActivity objects = secondary guided-content layer, not primary runtime delivery

## Drag/drop status

- Plain `.txt` is **not** a direct hardware artifact.
- Drag/drop goal is the validated exported package, not raw source text.
- Raw text must pass through ClassPad Manager editor/token conversion first.
