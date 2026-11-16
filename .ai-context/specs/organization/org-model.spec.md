# Spec: Organization model

## Spec ID

org-model

## Status

Draft

## Linked BRD

.ai-context/brd/organization/org-model.brd.md  
Baseline: .ai-context/BRD.md (FR-08–FR-14, BR-07–BR-13)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

Duties, labels, and delegation resolve who acts. Industry titles do not add steps.

## Context

- Builds on: .ai-context/architecture.md (organization)
- Related: .ai-context/specs/mobility/application-decision-chain.spec.md
- API contract: not written yet. Delegation and escalation are decided in BR-10 and BR-11.

## Business behaviour

An administrator sets the unit label and the duties on a movement type. Unavailable primaries are covered by one delegate for at most 30 days. No delegate leads to escalation after 5 business days. Elapsed time never approves. The primary reporting line approves; other lines are notified unless the movement type requires them (BR-10, BR-11, BR-13).

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| org-model.AC01 | An administrator sets the unit label to the employer's own word. | A member views their unit. | That word is what they see. | FR-08 |
| org-model.AC02 | A movement type omits a duty. | A case of that type runs. | The omitted duty never appears and never blocks the case. | FR-09 |
| org-model.AC03 | The snapshot names a reporting line. | The member opens hierarchy. | The line shown is the snapshot line. | FR-10 |
| org-model.AC04 | The primary is unavailable and a delegate is active. | A task for that duty is open. | The delegate receives it. The audit stores both names. | FR-11 |
| org-model.AC05 | The only candidate delegate is the applicant. | Delegation is saved for that case. | The save is refused. | FR-11 |
| org-model.AC06 | No delegate exists and 5 business days have passed with no decision. | The case is still waiting. | The task escalates to the skip-level. The case is not approved. | FR-12 |
| org-model.AC07 | Two consecutive duties resolve to the same person. | The case reaches that pair. | One task records an outcome for each duty. A decline on either stops the case. | FR-13 |
| org-model.AC08 | The snapshot has a primary line and a second line, and the movement type does not require every line. | The member applies. | The primary line must support the move. The second line is notified and has no task. | FR-14 |

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| org-model.TEST01 | Positive | Custom unit label | Member sees that label | AC01 |
| org-model.TEST02 | Negative | Movement type with no extra compliance duty | Case does not wait for compliance | AC02 |
| org-model.TEST03 | Positive | Snapshot line changes | Hierarchy shows the new line | AC03 |
| org-model.TEST04 | Positive | Primary on leave, delegate active | Delegate holds the task; both names audited | AC04 |
| org-model.TEST05 | Negative | Delegate is the applicant | Refused | AC05 |
| org-model.TEST06 | Failure | Vacant post, no delegate, service time elapsed | Escalated; still undecided | AC06 |
| org-model.TEST07 | Boundary | Same person holds the next two duties | One task, two outcomes | AC07 |
| org-model.TEST08 | Negative | Combined task, one duty declines | Case stops | AC07 |
| org-model.TEST09 | Positive | Two reporting lines, type does not require both | Primary supports; second is notified only | AC08 |

## API contract

Not specified.

## Explicitly out of scope

Choosing the service-time number. That number is Q-02.

## Non-functional constraints

BR-41. Service time uses the business calendar once Q-23 is answered.
