# Project status board

_Last updated: 2026-11-16_

## Active specs

| Spec ID | Title | Status | Owner | Last updated | Notes |
| --- | --- | --- | --- | --- | --- |
| — | Internal Job Portal baseline v0.2 | Pending Gate 1 | Unassigned | 2026-11-16 | Questions closed. Journey: `.ai-context/brd/end-to-end-journey.md`. No plan, no tasks, no `src/`. |

Draft specs, all **not submitted**:

- `workforce-access`
- `org-model`
- `member-home`
- `openings-and-matching`
- `application-decision-chain`
- `movement-execution`
- `journey-notifications`
- `observation-and-control`

## Daily execution log

### 2026-11-16 — decisions and journey

- Closed Section 7 of the baseline. Version is now 0.2.
- Added `.ai-context/brd/end-to-end-journey.md` with the happy path and EC-01 through EC-70.
- Aligned the draft specs that named the old working assumptions.

### 2026-11-16 — initial baseline

- Ingested the client brief into BRD v0.1.
- Wrote journeys A–E, question log Q-01–Q-34, business rules BR-01–BR-44, and functional requirements FR-01–FR-42. Q-15 is closed: no hand-picked slate.
- Separated confirmed decisions (BD-01–BD-12, TD-01) from working assumptions.
- Drafted module BRDs and business-level specs with Given / When / Then criteria and spec-derived test designs.
- Left API contracts, technical plan, tasks, and application code unwritten. Q-12 and the open business questions would otherwise be frozen inside those artifacts.
- ADR-001 recorded as Proposed.

## Next

Submit the baseline for Gate 1 when a review is wanted. HTTP contracts and a technical plan come with that submission. Section 7 is already closed.
