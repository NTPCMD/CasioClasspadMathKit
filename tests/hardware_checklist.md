# FX-CP400 Hardware Deployment Checklist

## Before import

- [ ] Commit all source changes to Git.
- [ ] Copy current validated `.xcp` exports into `backups/` with date/version suffix.
- [ ] Confirm every program name is 8 chars or fewer.
- [ ] Confirm every `Prog` target exists.
- [ ] Run parser tests in ClassPad Manager editor.
- [ ] Run runtime tests in emulator.

## Import package preparation

- [ ] Export fresh `.xcp` package from ClassPad Manager.
- [ ] Copy the exported package into `release/`.
- [ ] Copy the exact transfer-ready artifact into `calculator_import/`.
- [ ] Keep previous known-good export unchanged for rollback.

## Hardware import

- [ ] Connect FX-CP400 over USB.
- [ ] Enter USB storage mode on calculator.
- [ ] Copy the validated package to the calculator import location required by ClassPad Manager workflow.
- [ ] Eject storage safely.
- [ ] Import/open package on calculator.

## After import

- [ ] Run `RFMENU`.
- [ ] Run `MN_MAIN`.
- [ ] Verify UTF symbol rendering.
- [ ] Verify exit/return paths.
- [ ] Archive the validated package as a hardware-approved backup.
