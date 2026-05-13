# Build Structure

- `syntax_validation/`: pre-release parser checks and compatibility reports.
- `manifests/`: generated module lists and hashes.

## Build stages
1. Syntax review against `docs/parser_verification.md`.
2. Minimal-suite pass (`tests/minimal/*`).
3. Package assembly list for release.
