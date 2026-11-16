# Spec: <Feature Name>

Repository path: `.ai-context/specs/<module>/<feature-slug>.spec.md`  
Record Gate 1 / Gate 2 on `.ai-context/GATE.md` (all modules) and copy the same row into **Gate Approvals & History**.

## Spec ID

<feature-slug>

## Status

Draft | In Peer Review | Changes Requested | Approved | Plan Drafted | Plan Reviewed | Tasks Generated | Under
Development | In QA | Ready for Release | Released (vX.Y.Z)

## Linked BRD

.ai-context/brd/<module>/<feature-slug>.brd.md (Index: .ai-context/BRD.md)

## Gate Approvals & History

| Gate                 | Approver        | Date       | Outcome  | Approval Comment                                               |
| -------------------- | --------------- | ---------- | -------- | -------------------------------------------------------------- |
| Gate 1 (Spec Review) | <Approver Name> | YYYY-MM-DD | Approved | "<User/Reviewer approval comment provided during review>"      |
| Gate 2 (Code Review) | <Approver Name> | YYYY-MM-DD | Approved | "<User/Reviewer approval comment provided during code review>" |

## Intent

<One paragraph: what changes, for whom, under what condition>

## Context

- Builds on: .ai-context/architecture.md (<section>)
- Related: .ai-context/specs/<module>/<related-spec>.spec.md
- API contract: <link if external>

## API Contract (Mandatory if API surface exists)

### <slug>.API01 — <METHOD> <path>

**Request payload:**

```json
{ "field": "type" }
```

**Success response (`<code>`):**

```json
{ "field": "type" }
```

**Exceptions:**

| Code | Condition     | Response body |
| ---- | ------------- | ------------- |
| 4xx  | `<condition>` | `<shape>`     |

## Acceptance Criteria

1. `<slug>`.AC1 — Given `<state>`, when `<action>`, then `<outcome>`.
2. `<slug>`.AC2 — Given `<state>`, when `<action>`, then `<outcome>`.

## Unit Test Cases (spec-derived)

| Test ID       | Maps to AC | Scenario     | Expected     |
| ------------- | ---------- | ------------ | ------------ |
| `<slug>`.UT01 | AC1        | `<scenario>` | `<expected>` |

## Explicitly Out of Scope

- <item>

## Non-Functional Constraints (from constitution.md)

- <latency / throughput / compliance constraint>
