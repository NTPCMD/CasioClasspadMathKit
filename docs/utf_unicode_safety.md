# UTF / Unicode Safety (FX-CP400)

## Risk summary
Symbols like `π` and `√` may fail when imported from UTF-8 plain text, depending on editor/tokenizer path.

## Test protocol
1. Create two minimal programs: one with literal UTF symbol, one with ASCII fallback.
2. Import through emulator editor, then via `.g1m` route.
3. Execute and inspect rendering + calculation output.
4. Export and re-open source to detect corruption.

## Fallback strategy
- Preferred transport source uses ASCII markers:
  - `PI` token placeholder for `π`
  - `SQRT(` call or `ROOT` placeholder for `√`
- Final device entry/tokenization replaces placeholders only after import success.

## Policy
Until all import paths are verified, release bundle remains ASCII-safe.
