# Project Constitution

**Status:** Draft, pending business-owner review of `.ai-context/BRD.md` v0.1  
**Adopted:** 2026-11-16  
**Product:** Internal Job Portal

Stack-specific rules for this product live here. Organizational process rules live in `GEMINI.md` and `.agent/`. Do not rewrite `.agent/`.

## Governance and roles

| Role | Name | Responsibility |
| --- | --- | --- |
| Business owner | To be named | Confirms or rejects assumed rules in `.ai-context/BRD.md` |
| Gate 1 reviewer (spec / BRD) | To be named on `.ai-context/GATE.md` | Specification readiness. Must not be the author. |
| Gate 2 reviewer (code) | To be named on `.ai-context/GATE.md` | Implementation evidence, after code exists |
| Spec author | To be named | Writes the spec. Does not self-approve Gate 1. |

- Gate 1 HALT and Gate 2 HALT apply.
- Baseline v0.2 decides the rules in `.ai-context/BRD.md`. Specs follow those rules.
- Reviewer names for every module are recorded once, on `.ai-context/GATE.md`.

## Testing discipline

- Tests are written from acceptance criteria, confirmed failing, then made to pass. That starts only after Gate 1 approves a spec and a plan is reviewed.
- Every acceptance criterion in an approved spec needs a test that can fail. Happy-path-only coverage is a Gate 2 rejection.
- Executable tests will live under `tests/frontend/` and `tests/backend/`. `.ai-context/test_cases/` holds the scenarios, not the code.
- The test runner is Jest for the experience and the service (BR-55).
- Coverage floors on changed files: 80% for identity, duty checks, and audit; 70% for mobility, opportunities, and transition; 60% for helpers and presentation.

## Security posture

- Workforce facts, application content, internal evaluation notes, tokens, and secrets do not appear in logs, specs, plans, tasks, or prompt history.
- Google sign-in is matched to the HRMS work identity. The portal does not offer a local password for members.
- A person who is not in the accepted workforce snapshot cannot be created in the portal.
- Duty checks are enforced on the server. Hiding a control in the interface is not the control.
- Named views of matching people are audited.
- New dependencies need a named reviewer before they enter a manifest. No manifest exists yet.

## Architectural constraints

- One application: Next.js for the experience, NestJS on Node.js for the service, PostgreSQL via Prisma, Tailwind CSS, Docker (BR-55, TD-01 through TD-05).
- Domain modules own their behaviour. Intended layout, when implementation is allowed:

```text
src/frontend/   app, modules, shared
src/backend/    app, config, modules, shared
tests/frontend/
tests/backend/
```

- Cross-module use goes through the module's public surface.
- The HRMS remains the system of record for workforce identity and for the assignment that results from a movement.
- A different database, data-access library, service framework, styling system, or deployment target requires a new ADR. The choices in BR-55 are the ones in force.
- Do not hard-code an industry title into a workflow step. Steps bind to duties.

## Non-functional baselines

- NFR-01 through NFR-04 in `.ai-context/BRD.md` apply. Decision writes and ordinary reads are within 2 seconds for 95% of calls, excluding HRMS and email time.
- Service times use Monday to Friday in the headquarters timezone, excluding the organization's holidays (BR-41).

## Versioning rules

- One feature branch per spec slug when implementation starts: `feature/<feature-slug>`.
- A breaking change to an approved API contract is a spec revision.
- Spec status must be Approved before implementation tasks run. Plan status must be Plan Reviewed before task execution starts.
- Commit messages must not carry AI-attribution suffixes.

## Delivery discipline

- No implementation task runs without an approved spec.
- Prompt by artifact ID. One task, one prompt, once tasks exist.
- `.ai-context/status.md` is updated the same day a spec or baseline state changes.
- `.ai-context/prompt_history.md` is append-only.
- Client feedback goes through `.ai-context/change_requests/` and Gate 1.
- A named human is accountable for every merged line.

## Artefact layout

```text
.ai-context/BRD.md
.ai-context/GATE.md
.ai-context/brd/<module>/<feature-slug>.brd.md
.ai-context/specs/<module>/<feature-slug>.spec.md
.ai-context/plans/<module>/<feature-slug>.plan.md      after Gate 1 only
.ai-context/tasks/<module>/<feature-slug>.tasks.md     after the plan is Plan Reviewed
.ai-context/test_cases/<feature-slug>.test_cases.md
```

- Do not create `.ai-context/modules/`.
- Do not use `.ai-context/specs/<slug>/spec.md`. The slug stays in the filename.
- There is no application tree until a spec is approved and its plan is reviewed.
