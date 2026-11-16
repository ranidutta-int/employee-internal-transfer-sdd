# Generate Plan Workflow

Turns a **Gate 1 Approved** spec into an implementation plan for this frontend-only Next.js app. Triggered when the user
says generate-plan, write a plan, or turn this spec into a plan.

INT SDD Blueprint V1.0: BRD → Spec → Gate 1 → **Plan** → Architecture Check → Tasks. A plan must not redefine business
intent. It must not run against a spec that is still `Draft`, `In Peer Review`, or `Changes Requested`.

**This repository (not the Blueprint greenfield layout):**

| Do                                                          | Do not                                                |
| ----------------------------------------------------------- | ----------------------------------------------------- |
| `.ai-context/plans/<module>/<feature-slug>.plan.md`         | `.ai-context/plans/<feature>.md` (other-project name) |
| `.ai-context/specs/<module>/<feature-slug>.spec.md`         | Flat `.ai-context/specs/<slug>.spec.md`               |
| `src/modules/<domain>/`                                     | `src/frontend/` or `tests/frontend/`                  |
| Colocated `src/**/*.test.ts(x)` + `tests/support/<domain>/` | New top-level module folders unless an ADR says so    |

Template: `.ai-context/templates/plan.template.md`. Keep its top-level sections. Do not invent extra product
requirements.

---

## Preconditions (stop if any fail)

1. Read `.ai-context/status.md` and `.ai-context/GATE.md`. Do not guess.
2. Open the spec. `## Status` must be `Approved` or later (`Plan Drafted` = revise the same plan file). If Gate 1 is not
   Approved, **HALT**. No plan, no tasks, no `src/` work.
3. Related / Builds-on specs listed in the spec must already be Approved or Released.

---

## What to do

1. **Read the Approved spec in full** — Intent, Context, API Contract (if any), every given/when/then AC, Explicitly Out
   of Scope (resolve items marked “deferred to plan.md”), Non-Functional Constraints, spec-derived UTs. Do not plan from
   a chat summary.

2. **Read `.ai-context/architecture.md`** — the plan must fit existing conventions unless the spec documents a deviation
   that needs an ADR:
    - API: one Axios instance (`src/services/`); module endpoints in `src/modules/<domain>/api/` (or that module’s
      `services/`).
    - State: TanStack Query for server entities; Zustand only for session / ephemeral UI. Do not cache API entities in
      Zustand.
    - Routing: `src/app/**` orchestrates only; no Axios, domain policy, or form schemas in route files. Catch-all +
      `routeComponentMap.ts` + RBAC / `ProtectedRoute` / menu guard.
    - Forms: new SDD work is React Hook Form + Zod in `src/modules/<domain>/schemas/`. Inherit Formik + Yup only when
      the spec says this surface is already that stack.

3. **Identify affected files** under the owning `src/modules/<domain>/` tree: `api/`, `components/`, `hooks/`,
   `schemas/`, `stores/`, `types/`, `utils/`. New module-level folders are rare. Cross-domain use goes through that
   module’s `index.ts`. Also name colocated tests and `tests/support/<domain>/` registries. Do not invent REST paths.

4. **List new npm dependencies, if any**, and why an existing package cannot do the job. Constitution: new dependencies
   need named reviewer sign-off. Prefer existing Axios, TanStack Query, Zustand, RHF/Zod, RTL, `jest.spyOn`.

5. **ADR before the plan is finalized** — if the approach needs a new pattern, HTTP client, datastore, state library, or
   cross-cutting concern, write `.ai-context/decisions/ADR-NNN.md` from `.ai-context/templates/adr.template.md`
   **before** saving the plan. Do not fold that in as an implementation detail after coding.

6. **Constitution Check, line by line** — read `.ai-context/constitution.md` in full. Every rule under Governance,
   Testing Discipline, Security Posture, Architectural Constraints, Non-Functional Baselines, Versioning, Delivery
   Discipline, and Artefact layout gets its own checkbox with a one-line reason (or “N/A because …”). Silence is a gap,
   not a pass. Do not leave the template’s three placeholder bullets.

7. **Explicitly Deferred** — pull the spec’s deferred items first. Flag ambiguity; do not silently pick behaviour the
   spec left open. Add `## Open Questions` only for unresolved items.

8. **Sequence so each step leaves the app working** — independently verifiable, typecheck-clean (`npm run type-check`),
   no half-wired intermediate that would break `main` / `Dev`. Order typically: types / API wrappers → failing tests
   (RED) → implementation (GREEN) → wiring. Sequencing is the input to `tasks.md` later — do **not** write `.tasks.md`
   in this workflow.

9. **Save the plan** at `.ai-context/plans/<module>/<feature-slug>.plan.md` (same module folder as the spec). Required
   sections:

    ```text
    # Plan: <Feature Name>
    Repository path: .ai-context/plans/<module>/<feature-slug>.plan.md
    ## Derived From
    ## Linked BRD / CR          (this-repo paths only)
    ## Status                   (Plan Drafted)
    ## Architecture Approach
    ## Data Model               (or “no schema / no new REST”)
    ## Constitution Check
    ## Explicitly Deferred
    ## Sequencing
    ```

    Repository-relative paths only. No `C:\…` or `file:///`.

10. **On completion**
    - Spec `## Status` → `Plan Drafted` (append history; do not delete Gate 1 rows).
    - `.ai-context/status.md` Active Specs row + daily log, same day.
    - Tell the user: a **human Plan Reviewed** is next. Tasks generation is a separate step and must not start until
      Status is `Plan Reviewed`.

---

## Do not

- Implement screens or write application code.
- Generate `.tasks.md` or executable tests in this step.
- Copy React Native UI onto web (align intent and consumed HTTP only).
- Let a live API or sibling-repo plan rewrite this repo’s BRD.
- Commit unless the user asked.
