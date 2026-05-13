# FX-CP400 Hardware Checklist

- [ ] Import `tests/minimal/HELLO.txt` and execute.
- [ ] Import `tests/minimal/TEST_MENU.txt`; verify menu render and exit.
- [ ] Import `tests/minimal/TEST_INPUT.txt`; verify numeric input parsing.
- [ ] Import `tests/minimal/TEST_OUTPUT.txt`; verify `Locate` numeric output.
- [ ] Import `tests/minimal/TEST_VALID.txt`; verify branch + `Return`.
- [ ] Run `Prog` chain from `MN_MAIN` to one module and back.
- [ ] Verify `ClrText` behavior in all menu transitions.
- [ ] Verify `Goto`/`Lbl` loop stability (no freeze).
- [ ] Power-cycle calculator and verify variable persistence policy.
- [ ] UTF probe: `π` and `√` source imports + execution output.
- [ ] Memory test: progressively larger program import until warning.
- [ ] eActivity launch path from page link to target program.
