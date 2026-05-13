# Import Tests

## Parser/import smoke sequence
1. Transfer `calculator_import/minimal_sources/*.txt` to emulator editor.
2. Paste each into new program object named exactly like filename stem.
3. Execute each once and record parse/runtime status.

## Bundle import sequence
1. Build/obtain `.g1m` package.
2. Import into FA-CP1 / emulator transfer tool.
3. Sync to connected FX-CP400.
4. Verify all expected program names exist.

## Recovery tests
1. Export current calculator contents to backup `.g1m`.
2. Remove toolkit objects.
3. Re-import release bundle.
4. Re-run `MN_MAIN` and validate menu navigation.
