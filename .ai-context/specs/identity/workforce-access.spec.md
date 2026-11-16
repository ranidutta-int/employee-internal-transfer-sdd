# Spec: Workforce access

## Spec ID

workforce-access

## Status

Draft

## Linked BRD

.ai-context/brd/identity/workforce-access.brd.md  
Baseline: .ai-context/BRD.md (FR-01–FR-07, BR-01–BR-06)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

A person enters only when the HRMS has already sent an active workforce row, and only through Google matched to that row's work identity.

## Context

- Builds on: .ai-context/architecture.md (identity)
- Related: .ai-context/specs/experience/member-home.spec.md
- API contract: not written yet. The behaviour is decided in BR-03, BR-06, BR-51, and BR-52.

## Business behaviour

Journeys A1–A3. The portal accepts create, update, and end from the HRMS. It offers no add-person path. Mastered fields are shown without a save control. When the last accepted snapshot is older than 24 hours, new applications and new decisions pause (BR-06).

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| workforce-access.AC01 | The HRMS sends a new active person. | The snapshot is accepted. | The person can be matched at sign-in, and no human entered the row. | FR-01 |
| workforce-access.AC02 | The snapshot has no row for a person. | Someone attempts to create that person in the portal. | The attempt is refused. No add-person action is offered. | FR-02 |
| workforce-access.AC03 | An active row whose work identity is W. | Google sign-in returns W. | A session starts. | FR-03 |
| workforce-access.AC04 | Google returns an identity that matches no active row. | Sign-in is attempted. | Sign-in is refused. No row is created. | FR-03 |
| workforce-access.AC05 | A person has a session, and a later snapshot says employment has ended. | They next submit, decide, or complete a task. | The action is refused. | FR-04 |
| workforce-access.AC06 | The snapshot holds a designation. | The member opens profile. | The designation is visible and has no save control. | FR-05 |
| workforce-access.AC07 | A sync attempt fails. | An administrator opens sync health. | The failure and the age of the last accepted snapshot are visible. | FR-06 |
| workforce-access.AC08 | The last accepted snapshot is older than 24 hours. | A member submits an application or an approver records a decision. | The action pauses. The case is not advanced. Reading still works. | FR-07 |

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| workforce-access.TEST01 | Positive | HRMS join for a new active person | Row matchable at sign-in; no manual create | AC01 |
| workforce-access.TEST02 | Negative | Create person with no snapshot row | Refused | AC02 |
| workforce-access.TEST03 | Positive | Google identity equals work identity | Session starts | AC03 |
| workforce-access.TEST04 | Negative | Google identity matches nothing | Refused; no row | AC04 |
| workforce-access.TEST05 | Failure | End-of-employment accepted after session start | Next action refused | AC05 |
| workforce-access.TEST06 | Negative | Member tries to save designation | No save; value unchanged | AC06 |
| workforce-access.TEST07 | Failure | Sync error | Admin sees failure and last age | AC07 |
| workforce-access.TEST08 | Boundary | Snapshot age 24 hours plus a minute | New application and new decision pause | AC08 |
| workforce-access.TEST09 | Boundary | Snapshot age 23 hours | Submit and decide still allowed | AC08 |

## API contract

Not specified in this version. Behaviour is decided. Endpoint text comes with the build spec.

## Explicitly out of scope

Local passwords. Email or SMS sign-in. Manual roster import.

## Non-functional constraints

NFR-01. Tokens and workforce facts stay out of general logs.
