# Spec: Journey notifications

## Spec ID

journey-notifications

## Status

Draft

## Linked BRD

.ai-context/brd/notifications/journey-notifications.brd.md  
Baseline: .ai-context/BRD.md (FR-35, BR-35, Section 9)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

The people named in the baseline matrix hear about the step. Delivery failure does not roll back a decision.

## Context

- Builds on: .ai-context/architecture.md (notifications)
- Related: every module that records a material step
- API contract: not written.

## Business behaviour

Notices go in the portal and to the work email (BR-35). Delegates receive the covered duty's notices while the delegation is active. Executive observers receive immediate exceptions and a daily summary.

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| journey-notifications.AC01 | A member submits an application. | The submit is stored. | The member and the line authority each have a notice. | FR-35 |
| journey-notifications.AC02 | A delegate is covering the line authority. | That task is assigned. | The delegate has the notice. | FR-35 |
| journey-notifications.AC03 | A routine approval is recorded. | Notices are sent. | The executive observer does not receive that routine notice. | FR-35 |
| journey-notifications.AC04 | A transition task fails. | The failure is stored. | The owner, people operations, and the member have a notice. The executive observer has an exception notice. | FR-35 |
| journey-notifications.AC05 | A decision was stored and the work email fails. | Delivery is attempted. | The in-portal notice remains. The email retries 3 times over 24 hours. The decision remains. | FR-35 |
| journey-notifications.AC07 | A member submits. | Notices are created. | A work-email notice goes to the work email on the snapshot, not to a personal mailbox. | FR-35 |
| journey-notifications.AC06 | A member has notices for their own case and another member's case exists. | They open notices. | They see only their own. | FR-35 |

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| journey-notifications.TEST01 | Positive | Submit | Member and line authority notified | AC01 |
| journey-notifications.TEST02 | Positive | Active delegate | Delegate notified | AC02 |
| journey-notifications.TEST03 | Negative | Routine approval | No executive routine notice | AC03 |
| journey-notifications.TEST04 | Failure | Transition task fails | Owner, people operations, member, executive exception | AC04 |
| journey-notifications.TEST05 | Failure | Work email fails | In-portal notice remains; 3 retries in 24 hours; decision unchanged | AC05 |
| journey-notifications.TEST07 | Security | Submit notice | Sent to the work email only | AC07 |
| journey-notifications.TEST06 | Security | Member opens notices | Other members' notices absent | AC06 |

## API contract

Not specified.

## Explicitly out of scope

SMS, push, and personal mailboxes (BR-35).

## Non-functional constraints

NFR-01. Notice text stored for the member must not be copied into general logs.
