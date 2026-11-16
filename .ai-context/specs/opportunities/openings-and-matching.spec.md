# Spec: Openings and matching

## Spec ID

openings-and-matching

## Status

Draft

## Linked BRD

.ai-context/brd/opportunities/openings-and-matching.brd.md  
Baseline: .ai-context/BRD.md (FR-15–FR-19, BR-14–BR-20)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

Two members see different openings because their snapshot facts differ. Rules are visible. A hidden score is not used.

## Context

- Builds on: .ai-context/architecture.md (opportunities)
- Related: .ai-context/specs/mobility/application-decision-chain.spec.md
- API contract: not written.

## Business behaviour

Publishers are the receiving steward of the destination, people operations, or an administrator. Opening states are draft, open, paused, filled, withdrawn, and expired. Each rule is hard or warn. Working assumptions A-17, A-18, A-19, A-20, and A-26 apply where named.

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| openings-and-matching.AC01 | An open opening whose hard skill rule the member fails. | The member opens the list. | That opening is absent. | FR-15 |
| openings-and-matching.AC02 | The member passes every hard rule and fails a warn rule. | The member opens the list. | The opening is present and the warning is attached. | FR-15 |
| openings-and-matching.AC03 | Movement frequency is configured as hide, and the member is over the threshold. | The member opens the list. | The opening is absent. | FR-15 |
| openings-and-matching.AC04 | The same frequency rule is configured as warn. | The member opens the list. | The opening is present and the warning is attached. | FR-15 |
| openings-and-matching.AC05 | A default movement type, and the member is in probation. | The member opens the list. | The opening is absent. | FR-15 |
| openings-and-matching.AC06 | A visible opening. | The member asks why it is listed. | The answer lists passed rules and warnings, and no score. | FR-16 |
| openings-and-matching.AC07 | Several members fail different rules. | The publisher asks who matches. | Counts are grouped by the excluding rule. | FR-17 |
| openings-and-matching.AC08 | A duty that is not allowed opens the named match list. | They request it. | The request is refused. | FR-17 |
| openings-and-matching.AC09 | An allowed duty opens the named list. | The list is shown. | The view is audited. | FR-17 |
| openings-and-matching.AC10 | An opening is limited. | An ordinary member browses discovery. | It is absent. A hand-picked list of people is refused. | FR-18 |
| openings-and-matching.AC11 | A submitted application later fails a hard rule. | The next approver tries to advance it. | Advancement is refused and the people already involved are told. | FR-19 |

AC05 uses A-26. AC06 uses A-17. AC07–AC09 use A-18 and A-19. AC11 uses A-20.

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| openings-and-matching.TEST01 | Negative | Missing required skill | Hidden | AC01 |
| openings-and-matching.TEST02 | Positive | Warn-only frequency | Shown with warning | AC02, AC04 |
| openings-and-matching.TEST03 | Negative | Frequency set to hide | Hidden | AC03 |
| openings-and-matching.TEST04 | Boundary | Frequency exactly at the threshold | Follows the configured hard or warn outcome | AC03, AC04 |
| openings-and-matching.TEST05 | Negative | Probation on a default type | Hidden | AC05 |
| openings-and-matching.TEST06 | Positive | Why listed | Rules and warnings, no score | AC06 |
| openings-and-matching.TEST07 | Positive | Publisher counts | Grouped by excluding rule | AC07 |
| openings-and-matching.TEST08 | Negative | Disallowed named list | Refused | AC08 |
| openings-and-matching.TEST09 | Security | Allowed named list | Audited view | AC09 |
| openings-and-matching.TEST10 | Negative | Hand-picked slate on a limited opening | Refused | AC10 |
| openings-and-matching.TEST11 | Failure | Hard rule fails after submit | Cannot advance | AC11 |

## API contract

Not specified.

## Explicitly out of scope

Ranking. External applicants. Named invitations.

## Non-functional constraints

NFR-01. Match lists and skills stay out of general logs.
