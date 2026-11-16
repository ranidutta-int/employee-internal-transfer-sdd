# Spec: Observation and control

## Spec ID

observation-and-control

## Status

Draft

## Linked BRD

.ai-context/brd/governance/observation-and-control.brd.md  
Baseline: .ai-context/BRD.md (FR-36–FR-39, FR-42, BR-36–BR-38, BR-41)

## Gate Approvals and History

| Gate | Approver | Date | Outcome | Approval Comment |
| --- | --- | --- | --- | --- |
| Gate 1 (Spec Review) | | | Not submitted | |
| Gate 2 (Code Review) | | | Not started | |

## Intent

Executives can see the journey. Administrators can configure policy. Neither duty silently decides a case.

## Context

- Builds on: .ai-context/architecture.md (governance)
- Related: .ai-context/specs/mobility/application-decision-chain.spec.md
- API contract: not written.

## Business behaviour

The default attention rule is more than 70% declines across at least 5 decisions in 90 days, or 5 consecutive declines (BR-37). Audit events are kept for 7 years after the case closes (BR-38). Service time uses Monday to Friday in the headquarters timezone, excluding the organization's holidays (BR-41).

## Acceptance criteria

| ID | Given | When | Then | FR |
| --- | --- | --- | --- | --- |
| observation-and-control.AC01 | The signed-in duty is executive observer. | They open the pipeline. | They see journeys, time in stage, stalls, and decline counts. No approve action is offered. | FR-36 |
| observation-and-control.AC02 | An administrator sets a unit label and the duties on a movement type. | A later case uses that type. | The case uses that label and those duties. | FR-37 |
| observation-and-control.AC03 | A steward has 5 decisions in 90 days and more than 70% are declines, or 5 consecutive declines. | The period is evaluated. | People operations and the executive observer see an attention item. The steward still holds the duty. No application is approved by the item. | FR-38 |
| observation-and-control.AC04 | An administrator has turned the attention item off. | Decline counts are viewed. | Counts are visible. No attention item is raised. | FR-38 |
| observation-and-control.AC05 | A decline, a delegation, a waiver, or an intervention is stored. | An auditor opens the case. | That event is on the case, with who did it. | FR-39 |
| observation-and-control.AC06 | Saturday, Sunday, or a date on the organization's holiday list falls inside a service window. | The window is counted. | That day does not count. The timezone is the organization's headquarters timezone. | FR-42 |

## Spec-derived test design

| Test | Type | Scenario | Expected | AC |
| --- | --- | --- | --- | --- |
| observation-and-control.TEST01 | Positive | Executive opens pipeline | Pattern data visible | AC01 |
| observation-and-control.TEST02 | Security | Executive looks for approve | No approve action | AC01 |
| observation-and-control.TEST03 | Positive | Admin changes a movement type | Later case follows it | AC02 |
| observation-and-control.TEST04 | Boundary | Decline count one past the threshold | Attention item; steward unchanged | AC03 |
| observation-and-control.TEST05 | Boundary | Threshold unset | Counts only | AC04 |
| observation-and-control.TEST06 | Positive | Record a decline | Audit shows the actor | AC05 |
| observation-and-control.TEST07 | Boundary | Service time crosses a configured non-working day | That day is excluded | AC06 |

## API contract

Not specified.

## Explicitly out of scope

Automatic removal of a steward. Executive approval without an explicit movement-type step (A-29).

## Non-functional constraints

NFR-01. Pipeline views of named cases stay out of general logs.
