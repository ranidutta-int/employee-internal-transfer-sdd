# Spec: Member home and profile

## Spec ID

member-home

## Status

Draft

## Linked BRD

.ai-context/brd/experience/member-home.brd.md  
Baseline: .ai-context/BRD.md (FR-40, FR-41, BR-39, BR-40, BD-03)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

A member sees their own facts, the hierarchy slice they are allowed to see, relevant openings, and their own cases. Preferences narrow that list and cannot widen it.

## Context

- Builds on: .ai-context/architecture.md (experience)
- Related: .ai-context/specs/opportunities/openings-and-matching.spec.md
- API contract: not written.

## Business behaviour

Mastered facts come from the snapshot. The hierarchy slice is the upward line and the people who report to the member. Other duties get a home for their own queue; those queues are specified by the owning module.

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| member-home.AC01 | A member has skills, role, designation, unit, probation, and movement history on the snapshot. | They open profile. | Each of those facts is visible and read-only. | FR-40 |
| member-home.AC02 | The snapshot has a reporting line and direct reports. | The member opens hierarchy. | They see their upward line and the people who report to them. | FR-40 |
| member-home.AC03 | Another member has a profile. | This member requests that profile. | The request is refused. | FR-40 |
| member-home.AC04 | Policy would show three openings, and the member's preference excludes one. | They open the openings list. | The excluded opening is absent. An opening that fails a hard rule stays absent even if the preference names it. | FR-41 |
| member-home.AC05 | The member writes an application statement. | They apply. | The statement is stored on that application. | FR-41 |
| member-home.AC06 | The signed-in duty is a member with an open case and a waiting task. | They open home. | Home shows their relevant openings, their cases, and tasks waiting on them. | FR-40 |
| member-home.AC07 | The signed-in duty is a line authority with a queue in one unit. | They open home. | Home shows that queue. It does not show another unit's decision queue. | FR-40 |

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| member-home.TEST01 | Positive | Profile with a full snapshot | All named facts visible, read-only | AC01 |
| member-home.TEST02 | Positive | Upward line plus direct reports | Only that slice | AC02 |
| member-home.TEST03 | Negative | Open another member's profile | Refused | AC03 |
| member-home.TEST04 | Negative | Preference tries to reveal a hard-failed opening | Opening stays hidden | AC04 |
| member-home.TEST05 | Positive | Preference excludes one eligible opening | That opening absent | AC04 |
| member-home.TEST06 | Positive | Statement on apply | Statement stored on the application | AC05 |
| member-home.TEST07 | Positive | Member home | Own openings, cases, and tasks | AC06 |
| member-home.TEST08 | Negative | Line authority home | Other units' queues absent | AC07 |

## API contract

Not specified.

## Explicitly out of scope

Compensation (A-13). Whole-organization chart for a member. Accessibility standard (Q-24).

## Non-functional constraints

NFR-02. The member can see their own case state from home.
