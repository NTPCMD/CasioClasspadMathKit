# FX-CP400 Memory & Runtime Compatibility Notes

## Program naming

- Enforce max 8-character program names for compatibility safety.
- All callable programs in `src/` now follow this limit.

## Variable naming

- Keep variable identifiers to 8 chars or fewer.
- Shared core variables are aligned to this rule (`RESMODE`, `DPREC`, `AMODE`, etc.).

## Program size and nesting

- Keep programs modular and small; use `Prog` calls between core/module units.
- Avoid deep nesting chains; keep call depth practical and test each path.

## Global variable behavior

- Variables are shared across called programs unless explicitly reset.
- Core systems rely on shared flags (`VALID`, `ERRMSG`) and shared output vars (`RESVAL`, `RESLBL`).

## String handling

- Strings are used for prompts/titles/error text and compared via `=` / `≠`.
- Validate UTF symbol behavior (`π`, `√`) in final import testing.
