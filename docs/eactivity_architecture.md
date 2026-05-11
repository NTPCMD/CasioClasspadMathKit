# eActivity Integration Architecture

## Role of eActivity in MathKit

eActivity is a secondary delivery layer for guided lessons, not the primary runtime shell.
The runtime toolkit still starts from `MN_MAIN` and modular `Prog` calls.

## Planned eActivity structure

### 1. Launch page
- Brief instructions
- Quick links to core modules
- `Prog "MN_MAIN"` launch entry where supported

### 2. Formula sheet pages
- One page per module
- Static formulas and symbol legend
- References to matching toolkit program names

### 3. Interactive example pages
- Step-by-step worked examples
- Prompt students to run a linked program
- Record expected calculator output next to the example

### 4. Quick navigation page
- Short list of common exam tools
- Direct links to high-frequency programs (quadratic, pythagorean, mean, compound interest)

## Integration constraints

- Keep program names stable and <=8 chars.
- Do not duplicate business logic inside eActivity.
- eActivity pages should call modular programs rather than reimplement them.
- Treat eActivity files as release artifacts stored alongside `.xcp` exports.
