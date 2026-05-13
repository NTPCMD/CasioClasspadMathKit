# Performance Optimization Pass (FX-CP400)

## Findings focus
- Repeated `Locate` calls can slow redraw.
- Deep `Prog` chains add call overhead.
- Menu recursion should be replaced with label loops.
- Globals should be narrowed to required shared vars.

## Recommendations
1. Batch screen writes: `ClrText` once, then grouped `Locate` outputs.
2. Use `Lbl/Goto` menu loops locally instead of repeated parent recalls.
3. Keep common tools within 2-3 taps from `MN_MAIN`.
4. Cache validated values in reserved vars to avoid re-input.
5. Prefer lightweight utility modules for frequently used operations.

## Exam responsiveness targets
- Core tool launch: <=3 interactions.
- Return to menu after solve: <=1 pause + 1 key.
- No full-screen redraw if only one line changes.
