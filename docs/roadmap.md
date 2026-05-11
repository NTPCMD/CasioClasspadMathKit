# Development Roadmap

## Current completed systems
- Base project/module folder architecture
- Core menu scaffold (main/algebra/measurement/trig)
- Initial reusable core systems (format/validation/constants/result/error)
- Starter calculators across algebra, linear, trig, stats, finance

## Pending modules
- Full geometry menu and program suite
- Graphing helpers and utility modules
- Settings menu with persistent user preferences
- Expanded measurement/composite-shape calculators

## High-priority features
- Finalize menu coverage for all modules
- Remove duplicated local validation by expanding CVALID modes
- Add list-based stats workflows (beyond fixed-size input)
- Add robust output mode switching in CRESULT

## Testing checklist
- Verify all menu loops return safely
- Verify no `Stop` usage in recoverable validation paths
- Verify each program sets TITLE and uses CFORMAT
- Verify validation failures route through ERRMSG/VALID behavior
- Verify decimal/exact formatting output paths

## Optimisation goals
- Reduce repeated Locate blocks with shared format/result handlers
- Minimize Goto usage while preserving menu loop reliability
- Reuse constants and mode flags from CCONST only

## Export milestones
- Keep ClassPad-safe program names and short identifiers
- Validate syntax against on-device ClassPad parser
- Package programs by module for staged calculator import
