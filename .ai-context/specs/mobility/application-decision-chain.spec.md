# Spec: Application decision chain

## Spec ID

application-decision-chain

## Status

Draft

## Linked BRD

.ai-context/brd/mobility/application-decision-chain.brd.md  
Baseline: .ai-context/BRD.md (FR-20–FR-30, BR-21–BR-30)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

A visible, eligible member applies. Duties decide in the movement type's order. The default order is line authority, evaluation, releasing steward, people operations, then receiving steward (BR-21). Any decline stops the case.

## Context

- Builds on: .ai-context/architecture.md (mobility)
- Related: .ai-context/specs/organization/org-model.spec.md, .ai-context/specs/transition/movement-execution.spec.md
- API contract: not written yet. Chain order, limits, panel, and reasons are decided in BR-21 through BR-29.

## Business behaviour

The application stores the rule result and the snapshot time. Warnings must be acknowledged before support or approval. States used by this spec: submitted, in review, declined, withdrawn, approved for transition, stopped.

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| application-decision-chain.AC01 | The member can see the opening and is eligible. | They apply once. | The application is submitted and stores the rule result and snapshot time. | FR-20 |
| application-decision-chain.AC02 | An application for that opening already exists. | They submit again, including a double-click. | The existing application is reported. A second one is not created. | FR-20 |
| application-decision-chain.AC03 | The opening is hidden from the member. | They submit. | The submit is refused. | FR-20 |
| application-decision-chain.AC04 | A default movement type and line-authority support. | The support is recorded, warnings acknowledged. | Evaluation opens. Releasing steward is not yet assigned. | FR-21 |
| application-decision-chain.AC05 | The line authority declines. | The decline is saved with a reason code. | Evaluation does not start. The member sees the member-safe reason. | FR-21, FR-22 |
| application-decision-chain.AC06 | A decline is submitted with no reason code. | It is saved. | The save is refused. | FR-22 |
| application-decision-chain.AC07 | The case is in review and transition has not started. | The member withdraws. | The case is withdrawn. Open assignees and people who already decided are informed. | FR-23 |
| application-decision-chain.AC08 | The member already has one application in the decision chain or in transition. | They submit another. | The submit is refused. | FR-24 |
| application-decision-chain.AC09 | An opening becomes filled. | Other applications are still in review. | Those applications stop and the members are informed. | FR-25 |
| application-decision-chain.AC10 | An opening is paused. | An approver tries to advance an application. | The application stays and does not advance. | FR-25 |
| application-decision-chain.AC11 | The case carries an unacknowledged warning. | An approver records support. | The support is refused until the warning is acknowledged. | FR-26 |
| application-decision-chain.AC12 | A line-authority task is in progress and the snapshot then names a new line. | The current holder is still deciding. | The task stays with them. The new line authority is informed. | FR-27 |
| application-decision-chain.AC13 | The member's snapshot becomes suspended. | Any open case is advanced. | The case stops. A movement type cannot keep it moving. | FR-28 |
| application-decision-chain.AC14 | The releasing steward records retain. | People operations reopens within 10 business days with a reason. | The decline stays in history. The case returns to the releasing steward. | FR-29 |
| application-decision-chain.AC15 | An administrator freezes the unit. | A case in that unit is in review. | The case pauses in place. | FR-30 |
| application-decision-chain.AC16 | A case is stalled. | An administrator marks it approved with no intervention record. | The action is refused. | FR-30 |
| application-decision-chain.AC17 | Evaluation is a panel of three. | Two pass and one declines. | The case continues. The dissent stays on the record. | FR-21 |
| application-decision-chain.AC18 | A decline on an opening was recorded 89 days ago, and no application is active. | The member applies to that same opening. | The submit is refused. | BR-47 |
| application-decision-chain.AC19 | A panel ties. | The outcomes are saved. | The case declines. | FR-21 |
| application-decision-chain.AC20 | A decline on one opening is recorded, and no application is active. | The member applies to a different visible opening. | The submit is accepted, subject to the hard rules. | BR-47 |

These criteria follow baseline v0.2. People operations then receiving steward is the default order (BR-21). Reopen is allowed for 10 business days (BR-29).

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| application-decision-chain.TEST01 | Positive | Eligible apply | One submitted application with snapshot time | AC01 |
| application-decision-chain.TEST02 | Failure | Double submit | Still one application | AC02 |
| application-decision-chain.TEST03 | Negative | Hidden opening | Refused | AC03 |
| application-decision-chain.TEST04 | Positive | Line support on the default chain | Evaluation opens next | AC04 |
| application-decision-chain.TEST05 | Negative | Line decline | Chain stops; member-safe reason shown | AC05 |
| application-decision-chain.TEST06 | Negative | Decline without reason | Refused | AC06 |
| application-decision-chain.TEST07 | Positive | Withdraw before transition | Withdrawn; involved people informed | AC07 |
| application-decision-chain.TEST08 | Boundary | Second application while one is in chain or transition | Refused | AC08 |
| application-decision-chain.TEST09 | Failure | Opening filled mid-review | Other applications stop | AC09 |
| application-decision-chain.TEST10 | Negative | Paused opening | No advance | AC10 |
| application-decision-chain.TEST11 | Negative | Unacknowledged warning | Support refused | AC11 |
| application-decision-chain.TEST12 | Boundary | Line changes during an open task | Holder unchanged; new line informed | AC12 |
| application-decision-chain.TEST13 | Failure | Suspension | Open cases stop | AC13 |
| application-decision-chain.TEST14 | Positive | Retain, then reasoned reopen | Decline kept; reopen stored | AC14 |
| application-decision-chain.TEST15 | Negative | Freeze | Case pauses | AC15 |
| application-decision-chain.TEST16 | Security | Silent admin approve | Refused | AC16 |
| application-decision-chain.TEST17 | Positive | Panel 2 pass, 1 decline | Case continues; dissent kept | AC17 |
| application-decision-chain.TEST18 | Negative | Same opening reapplied at 89 days | Refused | AC18 |
| application-decision-chain.TEST19 | Negative | Panel tie | Case declines | AC19 |
| application-decision-chain.TEST20 | Positive | Different opening after a decline, nothing else active | Accepted | AC20 |

## API contract

Not specified.

## Explicitly out of scope

Changing the one-active-application rule, the majority panel, or the 90-day cooling-off. Those are decided in BR-25, BR-23, and BR-47.

## Non-functional constraints

NFR-01 and NFR-02.
