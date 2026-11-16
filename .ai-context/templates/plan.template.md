# Plan: <Feature Name>

Repository path: `.ai-context/plans/<module>/<feature-slug>.plan.md`

## Derived From

.ai-context/specs/<module>/<feature-slug>.spec.md (version, Gate 1 Approved date)

## Linked BRD / CR

- .ai-context/brd/<module>/<feature-slug>.brd.md
- .ai-context/change_requests/CR-YYYYMMDD-<slug>.md (if any)

## Status

Plan Drafted

## Architecture Approach

<owning modules under `src/frontend/modules/<domain>/` and `src/backend/modules/<domain>/`; new vs existing;
integration points; preservation of untouched surfaces>

## Data Model

<schema / client types / no backend migration / no new REST — say so explicitly>

## Constitution Check

Walk `.ai-context/constitution.md` in full. One checkbox per rule, with a one-line reason (or N/A). Do not leave these
three lines as the whole check.

- [ ] Governance & Roles
- [ ] Testing Discipline
- [ ] Security Posture
- [ ] Architectural Constraints
- [ ] Non-Functional Baselines
- [ ] Versioning Rules
- [ ] Delivery Discipline
- [ ] Artefact layout

## Explicitly Deferred

- <item with reason — include spec “deferred to plan.md” items>

## Sequencing

1. <step that leaves typecheck clean>
2. <failing tests RED, then implementation GREEN>
