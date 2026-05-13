# eActivity Integration Architecture (FX-CP400)

## Goals
- Launch toolkit programs from eActivity pages.
- Provide formula references and worked examples.
- Keep exam-time navigation to 2-3 taps.

## Proposed structure
1. **Home eActivity page**
   - Topic index (Algebra, Trig, Stats, Finance, Geometry).
   - Quick links to run `MN_MAIN` or direct module menus.
2. **Formula sheets**
   - Static text/math boxes only (no heavy dynamic objects).
3. **Worked examples**
   - Step blocks with matching program launch hints.
4. **Interactive strips**
   - Input instruction + launch command mapping.
5. **Exam quick-access page**
   - Minimal text and highest-frequency tool links.

## Compatibility guidance
- Keep program names stable; eActivity links depend on exact names.
- Avoid Unicode-heavy labels unless verified in target firmware.
- Version eActivity assets together with release manifest.
