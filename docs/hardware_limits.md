# FX-CP400 Hardware Limits (Operational Guidance)

Date: 2026-05-13
Status: mixed verified/model-dependent; requires final on-device confirmation.

## Constraint table
| Item | Current status | Deployment recommendation |
|---|---|---|
| Max program size | MODEL-DEPENDENT | Keep each module small (<10 KB source) and split by feature. |
| Program name length | CONFIRMED practical limit behavior | Use <=8 uppercase chars where possible. |
| Numeric variables | CONFIRMED A-Z baseline | Reserve shared vars map in module docs. |
| String variables/length | MODEL-DEPENDENT | Prefer short prompts; avoid long persistent strings. |
| Recursion depth | UNCONFIRMED | Avoid recursion; use loop+labels for control flow. |
| Nested `Prog` calls | MODEL-DEPENDENT | Keep chain depth shallow (<=3) for safety. |
| Total storage availability | MODEL-DEPENDENT | Keep release with margin and document module sizes. |
| eActivity size/object count | MODEL-DEPENDENT | Split activities by topic, avoid monolithic notebooks. |

## Actionable limits policy
- No deep recursive patterns.
- Prefer menu loops over nested `Prog` trees.
- Keep source ASCII-safe for transport; tokenize symbols in final editor.
- Maintain backup `.g1m` before any deployment.
