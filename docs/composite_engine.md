# Composite Shape Engine Plan

## Shape stack system
- Maintain `SHAPE_ID` and per-shape dimension slots.
- Push each selected shape into a stack index.
- Allow edit/remove before final computation.

## Area accumulation
- Compute each shape area independently.
- Add signed area to `AREA_TOTAL`.
- Support subtraction mode for cut-outs.

## Perimeter accumulation
- Track exposed edges only.
- Add edge contributions into `PERIM_TOTAL`.
- Allow toggles for shared/internal edges.

## Decomposition workflow
- Prompt user to decompose a complex figure into known primitives.
- Validate each primitive before accepting it.
- Show running totals after each primitive entry.

## User prompts
- Consistent prompt order: shape type → dimensions → include/exclude.
- Include summary confirmation before final result output.
- Use reusable prompt renderer from core UI.

## Reusable shape objects
- Define per-shape handlers (rectangle, circle sector, triangle, trapezium).
- Each handler returns area, perimeter contribution, and validity flag.
- Route through shared validation and result formatting systems.
