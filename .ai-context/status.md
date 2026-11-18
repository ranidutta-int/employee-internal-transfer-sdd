# Project status board

_Last updated: 2026-11-17_

## Active specs

| Spec ID | Title | Status | Owner | Last updated | Notes |
| --- | --- | --- | --- | --- | --- |
| — | Internal Job Portal baseline v0.3 | Pending Gate 1 | Unassigned | 2026-11-17 | Questions closed. Companions for pages, HRMS sync, resilience. No plan, no tasks, no `src/`. |

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

### 2026-11-17 — pitch demo journey

- Added `.ai-context/brd/pitch-demo-journey.md`: beat-by-beat walkthrough from HRMS sync to complete, for pitch demos. Alternate endings kept short; full edge cases stay in `end-to-end-journey.md`.

### 2026-11-17 — operational BRD detail

- Baseline moved to **v0.3**.
- Added `.ai-context/brd/product-surface.md` (pages + duty matrix).
- Added `.ai-context/brd/hrms-sync-and-hierarchy.md` (webhook/pull, 15-minute status path, hierarchy, mid-journey unit/role/line changes).
- Added `.ai-context/brd/resilience-urgency-and-closure.md` (data loss, expedite, non-response clocks, mid-journey stop/cleanup).
- Extended journey with EC-71–EC-80. Added BR-59–BR-68 and FR-43–FR-51.

### 2026-11-16 — decisions and journey

- Closed Section 7 of the baseline. Version was 0.2.
- Added `.ai-context/brd/end-to-end-journey.md` with the happy path and EC-01 through EC-70.

### 2026-11-16 — initial baseline

- Ingested the client brief into BRD v0.1 and built SDD folders.
- Drafted module BRDs and specs. No application code.

## Next

Submit for Gate 1 when ready. Next artefacts after that: HTTP contracts, data model, plan/tasks. Module specs should be refreshed to cite v0.3 companions where they still say “API not written” only.
