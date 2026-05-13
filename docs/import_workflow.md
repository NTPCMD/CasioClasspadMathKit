# FX-CP400 Import Workflow (Emulator + Hardware)

Date: 2026-05-13

## Primary safe workflow (recommended)
1. Open ClassPad Manager / Emulator Program app.
2. Create new program with target name (e.g., `MN_MAIN`).
3. Open matching `.txt` source from repository.
4. Paste source into program editor.
5. Run syntax check by executing once.
6. Repeat for all modules.
7. Export calculator memory/program package to `.g1m` as release artifact.
8. Connect FX-CP400 by USB mass-storage/manager mode.
9. Use CASIO transfer utility to send `.g1m` to device.
10. On calculator, open Program app and run `MN_MAIN`.

## Backup workflow
1. Before import, export full calculator state to backup `.g1m`.
2. Keep backup in `calculator_import/backups/` with date tag.
3. If deployment fails, restore backup `.g1m` then retry module-by-module.

## File format notes
- `.txt`: source interchange only; safest for human diff/review.
- `.g1m`: calculator data container for program import/export.
- `.xcp`: eActivity/classpad content package path is model/tool dependent.

## Exact menu path placeholders (to verify per installed tool version)
- Emulator: `Program -> New -> Edit -> Execute`.
- Transfer utility: `File -> Import/Send -> Device`.
- Calculator: `Main Menu -> Program -> Select -> EXE`.

## Import validation after transfer
- Confirm required core modules exist (`MN_MAIN`, `MN_SET`, `CINPUT`, `CVALID`, `CFORMAT`).
- Run minimal suite from `tests/minimal/`.
- Run `MN_MAIN` and traverse two levels of menus.
