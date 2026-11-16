# Gate board template

Copy to `.ai-context/GATE.md` (one file for all modules). Do not put GATE.md under `brd/` or `specs/`.

Fill **Named reviewers** once, then Gate 1 / Gate 2 per feature row.

```markdown
# Gate board — all modules

## Named reviewers (all modules)

| Role                                    | Name | Email |
| --------------------------------------- | ---- | ----- |
| Gate 1 reviewer (spec / BRD) — PM or TL |      |       |
| Gate 2 reviewer (code) — TL             |      |       |
| Spec author — <module>                  |      |       |

## Feature log

| Module   | Spec ID | Spec path                                   | BRD path                                 | Gate 1         | Gate 1 reviewer | Gate 1 date | Gate 1 comment | Gate 2      | Gate 2 reviewer | Gate 2 date | Gate 2 comment |
| -------- | ------- | ------------------------------------------- | ---------------------------------------- | -------------- | --------------- | ----------- | -------------- | ----------- | --------------- | ----------- | -------------- |
| <module> | <slug>  | `.ai-context/specs/<module>/<slug>.spec.md` | `.ai-context/brd/<module>/<slug>.brd.md` | In Peer Review |                 |             |                | Not started |                 |             |                |
```
