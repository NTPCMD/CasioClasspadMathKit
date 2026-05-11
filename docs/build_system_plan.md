# FX-CP400 Build System Plan

## Target architecture

- `src/` = modular editable source of production programs
- `build/` = generated manifests, name/syntax audit output, staging copies for manager import
- `release/` = versioned `.xcp` export bundles and release notes
- `calculator_import/` = latest transfer-ready package and canonical minimal references

## Planned build stages

### 1. Source audit
- Check filename length <= 8.
- Check variable/shared identifier length <= 8 where enforced by project profile.
- Check every `Prog` target resolves.
- Check no banned tokens appear in strict mode.

### 2. Staging
- Copy validated source into `build/staged_sources/`.
- Emit manifest of program names and target folders.
- Emit import order list for ClassPad Manager entry.

### 3. Release preparation
- Store exported `.xcp` in `release/<version>/`.
- Copy last-known-good package into `backups/` before replacing current import artifact.
- Update `calculator_import/current/` with latest validated package.

## Future automation

- A repo-local validator script can later generate the source audit report.
- A manifest generator can prepare paste/import order.
- Release notes can capture tested hardware/emulator status per version.

## Non-goals for now

- No direct binary `.xcp` generation from plain text.
- No graphics packaging.
- No monolithic program build step.
