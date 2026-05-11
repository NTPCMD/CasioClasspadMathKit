# FX-CP400 Performance Optimization Review

## Current observations

### Repeated `Locate` calls
- Shared formatting is already centralized in `CFORMAT`.
- Leaf programs still perform multiple line-by-line `Locate` writes for result output.
- Recommendation: keep current pattern for clarity, but avoid full-screen redraws after every small calculation step.

### Repeated validation patterns
- Shared validation is already centralized in `CVALID`.
- Recommendation: continue moving reusable checks into `CVALID` rather than duplicating local validation code.

### Unnecessary globals
- Shared globals are required for `Prog`-based modular design.
- Recommendation: keep only documented shared variables persistent; clear temporary flags when they are not part of a stable interface.

### Menu recursion vs looping
- Current design uses `Lbl`/`Goto` loops instead of recursive menu calls.
- Recommendation: keep loop-based menus; avoid calling menus from menus unless needed for module transitions.

### Redraw frequency
- `CFORMAT` clears and redraws the standard shell each time a program opens.
- Recommendation: acceptable for exam workflows; avoid extra `ClrText` calls inside leaf programs after the initial formatted screen unless the screen must be reset.

## Priority recommendations

1. Preserve parser stability over micro-optimization.
2. Prefer fewer `Prog` hops only when they do not reduce modular clarity.
3. Avoid unnecessary Pause/refresh cycles in chained workflows.
4. Keep high-frequency exam paths shallow and predictable.
