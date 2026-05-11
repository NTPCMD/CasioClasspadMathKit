# FX-CP400 Import/Export Pipeline

## Required tools

- ClassPad Manager (PC)
- FX-CP400 emulator (if available in ClassPad Manager package)
- Physical FX-CP400 for final validation

## Pipeline

1. Maintain source in `src/**/*.txt`.
2. Open ClassPad Manager program editor.
3. Create/update program objects with matching names (<=8 chars).
4. Paste program text and save inside manager project.
5. Test in emulator runtime.
6. Export package from manager to calculator-compatible format (`.xcp` project/export workflow).
7. Import to calculator via USB storage workflow.
8. Re-run core regression checks on hardware.

## Drag/drop & conversion notes

- Plain text is not guaranteed to auto-convert directly to ClassPad objects without manager parsing.
- Use manager editor import/paste flow for reliable token conversion.
- Validate token rendering for special characters (`π`, `√`) after conversion.
