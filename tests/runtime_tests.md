# FX-CP400 Runtime Test Suite

Run these tests first in ClassPad Manager emulator, then on physical FX-CP400 hardware.

## Test group A: minimal reference programs

### `RFHELLO`
- Launch program.
- Expected: cleared text screen, `HELLO` at row 1 column 1, waits at `Pause`, exits on EXE.

### `RFINPUT`
- Launch program.
- Enter a numeric value.
- Expected: input returns to program, value is shown with `Locate`, exits after `Pause`.

### `RFRES`
- Launch program.
- Expected: displays `A^2=4` without requiring input.

### `RFVALID`
- Launch program.
- Enter `0`.
- Expected: displays `INVALID`.
- Launch again and enter `5`.
- Expected: displays `VALID`.

### `RFMENU`
- Launch program.
- Expected: each menu item opens its linked program and returns to the menu after completion.
- Exit branch should leave the menu without error.

## Test group B: toolkit runtime checks

### `MN_MAIN`
- Launch root menu.
- Navigate through every module branch.
- Confirm each branch returns to the correct caller.

### Shared state persistence
- From a leaf program that calls `CRESULT`, confirm values assigned before `Prog` remain visible in the callee.
- Confirm `VALID` and `ERRMSG` behave correctly across `CVALID` -> `CERROR`.

### UTF symbols
- Run any path that renders `π`.
- Confirm symbol displays correctly after import.
- Add one ad-hoc test program using `√` and confirm token retention.

### fRound behavior
- Use a known decimal such as `12.3456` and `DPREC=2`.
- Record whether output is 2 decimal places or 2 significant figures.
- Update docs if behavior differs from current assumption.

## Pass criteria

- No syntax error during execution.
- No `Prog` target failures.
- No broken return path.
- No corrupted screen rendering.
- No symbol corruption for validated UTF tests.
