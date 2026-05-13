# Real Export Checklist

## Backup workflow

1. Keep `src/**/*.txt` in Git as the authoritative source.
2. Before each export, copy the previous validated `.xcp` into `backups/`.
3. Use versioned release directories in `release/`.
4. Never overwrite the last known-good hardware-approved package.

## Import workflow

1. Validate source names and `Prog` targets.
2. Paste validated text into ClassPad Manager programs.
3. Save and run emulator checks.
4. Export a fresh `.xcp` bundle.
5. Copy the exact transfer artifact into `calculator_import/`.
6. Transfer to hardware and run smoke tests.

## Recovery workflow

1. If import fails, remove the bad package from the current deployment set.
2. Re-import the last known-good `.xcp` from `backups/`.
3. Re-run `RFMENU` and `MN_MAIN` smoke tests.
4. Log the failed version and the exact observed error.

## Corruption prevention

- Eject calculator storage cleanly.
- Do not interrupt manager save/export operations.
- Re-open exported packages once before transfer when possible.
- Validate UTF tokens after every new export path.

## Versioning system

Recommended release naming:

- `mathkit-fxcp400-vYYYY.MM.DD-buildNN.xcp`
- keep matching notes in `release/<version>/README.md`
