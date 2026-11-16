# Spec: Movement execution

## Spec ID

movement-execution

## Status

Draft

## Linked BRD

.ai-context/brd/transition/movement-execution.brd.md  
Baseline: .ai-context/BRD.md (FR-31–FR-34, BR-31–BR-34, BR-43, BR-44)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

Approval creates a checklist. The HRMS must accept the new assignment before the movement is complete. Employers with no separate enabling team do not receive tasks for functions they do not have.

## Context

- Builds on: .ai-context/architecture.md (transition)
- Related: .ai-context/specs/mobility/application-decision-chain.spec.md
- API contract: not written. The HRMS write-back mechanism is Q-11.

## Business behaviour

States used here: approved for transition, in transition, complete, rescinded. A waiver stores the person and the reason. Predecessors of the HRMS confirmation are configurable; with none configured, confirmation runs last (A-32b).

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| movement-execution.AC01 | The decision chain is not complete. | Transition is requested. | No checklist is created. | FR-31 |
| movement-execution.AC02 | The chain has completed. | The checklist is built. | HRMS assignment confirmation is on it. | FR-31 |
| movement-execution.AC03 | The movement type enables no access task. | The checklist is built. | There is no access task. | FR-31 |
| movement-execution.AC04 | A task owner waives a task and gives no reason. | The waiver is saved. | The save is refused. | FR-32 |
| movement-execution.AC05 | Every required task is done or waived, and the HRMS has not accepted the assignment. | Completion is requested. | The movement stays in transition. | FR-32 |
| movement-execution.AC06 | Every required task is done or waived, and the HRMS has accepted the assignment. | Completion is recorded. | The movement is complete. | FR-32 |
| movement-execution.AC07 | The effective date has not arrived and the movement is incomplete. | People operations rescinds with a reason. | Open tasks cancel. The chain is informed. The state is rescinded. | FR-33 |
| movement-execution.AC08 | The movement is already complete. | Someone tries to undo it in place. | The undo is refused. A reversal requires a new movement. | FR-33 |
| movement-execution.AC09 | One task fails and another task does not depend on it. | The failure is recorded. | The failed task stays open. The independent task can still be completed. People operations are informed. | FR-34 |
| movement-execution.AC10 | A secondment type is enabled and has an expected end. | The movement completes. | People operations receive a return reminder. The return is not itself a completed movement. | BR-43 |
| movement-execution.AC11 | A movement completes and the vacancy notice is on, which is the default. | Completion is recorded. | The releasing steward is notified. No opening is created. | BR-44 |
| movement-execution.AC12 | A task owner tries to waive HRMS assignment confirmation. | The waiver is saved. | The save is refused. | BR-32 |

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| movement-execution.TEST01 | Negative | Chain still in review | No checklist | AC01 |
| movement-execution.TEST02 | Positive | Chain complete | HRMS confirmation present | AC02 |
| movement-execution.TEST03 | Boundary | Type with only the HRMS task | Checklist length is that one task | AC03 |
| movement-execution.TEST04 | Negative | Waiver without reason | Refused | AC04 |
| movement-execution.TEST05 | Failure | HRMS rejects the assignment | Stays in transition | AC05 |
| movement-execution.TEST06 | Positive | Tasks settled and HRMS accepts | Complete | AC06 |
| movement-execution.TEST07 | Positive | Rescind before effective date | Tasks cancelled; state rescinded | AC07 |
| movement-execution.TEST08 | Negative | Undo after complete | Refused | AC08 |
| movement-execution.TEST09 | Failure | One task fails, another is independent | Independent task can finish | AC09 |
| movement-execution.TEST10 | Positive | Secondment end date | Reminder only | AC10 |
| movement-execution.TEST11 | Positive | Completion with the default notice | Steward notified; no opening created | AC11 |
| movement-execution.TEST12 | Negative | Waive HRMS confirmation | Refused | AC12 |

## API contract

Not specified. Q-11 chooses how assignment acceptance is requested.

## Explicitly out of scope

Pay calculation. Granting access. Learning content.

## Non-functional constraints

NFR-01. A movement is not marked complete on a partial HRMS failure (FS-12).
