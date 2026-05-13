# ClassPad File Format Notes

## `.xcp`

- ClassPad manager/export container used for ClassPad program transfer workflows.
- Preferred format for FX-CP400 packaging/import.

## `.g1m`

- Legacy/other Casio series transfer format.
- Not the primary target format for FX-CP400 workflow in this repository.

## `.c2p`

- Legacy ClassPad-era package references may exist in older tooling/docs.
- Treat as compatibility/legacy format unless manager workflow requires it.

## eActivity

- ClassPad documents that can include math content, text, and objects.
- Useful for guided worksheets; program modules remain the main runtime artifacts here.

## External generation status

- Direct generation of binary calculator objects from raw text is tooling-dependent.
- Use ClassPad Manager as the canonical conversion/validation step.
