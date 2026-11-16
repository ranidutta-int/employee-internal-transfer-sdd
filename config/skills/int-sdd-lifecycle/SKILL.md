---
name: int-sdd-lifecycle
description:
    Manage the end-to-end INT SDD feature development lifecycle, including feature specs, status board updates, Gate 1
    peer reviews, implementation plans, task breakdown, test-first TDD, Gate 2 code reviews, and Definitions of Ready &
    Done.
---

# INT SDD Feature Engineering Lifecycle

## Purpose

This skill defines the mandatory feature-level engineering lifecycle according to the INT SDD Blueprint V1.0. Every
feature must progress sequentially through Specifications, Gate 1 Review, Implementation Plans, Task Breakdown,
Test-First TDD, and Gate 2 Code Review.

---

# Mandatory Lifecycle Flow

```text
BRD (.ai-context/BRD.md)
       ↓
1. Spec Authoring (.ai-context/specs/<feature-slug>.spec.md)
       ↓
2. HALT & WAIT for Gate 1 Spec Peer Review Approval
       ↓
Plan Authoring (.ai-context/plans/<feature-slug>.plan.md)
       ↓
Architecture Check
       ↓
Tasks Generation (.ai-context/tasks/<feature-slug>.tasks.md)
       ↓
4. Full Development Phase: Test-First (RED) ──► Implementation (GREEN) ──► Full Test Suite Verification (PASS)
       ↓
5. HALT & WAIT for Gate 2 Code Review Approval (Step 2 Approval)
       ↓
6. Release Note / Plan Creation (.ai-context/releases/RELEASE-vX.Y.Z.md)
       ↓
7. Automated Spec Iteration Loop: Scan BRD.md for Next Spec ──► (Loop back to Step 1)
```

## Canonical Spec State Machine & Status Definitions

```text
Draft → In Peer Review (Gate 1) → Changes Requested ↺
      → Approved → Plan Drafted → Plan Reviewed → Tasks Generated
      → Under Development → In QA → Ready for Release → Released (vX.Y.Z)
```

### Standardized Spec Statuses:

| Status              | Lifecycle Stage & Definition                                       |
| ------------------- | ------------------------------------------------------------------ |
| `Draft`             | Spec is being authored; not yet ready for peer review.             |
| `In Peer Review`    | Spec submitted for Gate 1 peer review.                             |
| `Changes Requested` | Gate 1 review requested updates before approval.                   |
| `Approved`          | Spec passed Gate 1 review with human approval comment captured.    |
| `Plan Drafted`      | Implementation plan (`.plan.md`) created from approved spec.       |
| `Plan Reviewed`     | Implementation plan reviewed and approved.                         |
| `Tasks Generated`   | Executable tasks (`.tasks.md`) derived and ready for TDD.          |
| `Under Development` | Feature actively being implemented (TDD Red → Green cycle).        |
| `In QA`             | Implementation complete, tests GREEN, undergoing Gate 2 review.    |
| `Ready for Release` | Passed Gate 2 review with code merged, pending release deployment. |
| `Released (vX.Y.Z)` | Deployed to production and released under tag `vX.Y.Z`.            |

## Mandatory Lifecycle Rules

- **No implementation without an approved Spec**.
- **Gate 1 HALT**: After generating a feature spec (`.spec.md`), the agent MUST STOP and wait for explicit human Gate 1
  approval.
- **Plan, Tasks & Tests Generation**: Only after Gate 1 approval is granted, create `.plan.md`, `.tasks.md`, and
  `.test_cases.md`.
- **TDD RED before GREEN**: Write failing executable tests (`tests/`) first and confirm RED before writing
  implementation code (GREEN).
- **Complete Development Before Gate 2**: All feature development tasks must be executed and test suites run to confirm
  100% PASS before triggering Gate 2 review. The agent MUST NOT request Gate 2 approval prematurely while development or
  test execution is still in progress.
- **Gate 2 HALT**: After all development tasks are 100% complete and implementation tests pass GREEN, the agent MUST
  STOP and wait for explicit human Gate 2 code review approval (Step 2 Approval).
- **Post-Gate 2 Release Note Creation**: Immediately following Gate 2 approval, create/update the Release Plan document
  (`.ai-context/releases/RELEASE-vX.Y.Z.md`) and sync `.ai-context/status.md`.
- **Automated Next Spec Loop**: After release logging, automatically scan `.ai-context/BRD.md` for the next
  un-implemented requirement, draft its spec, and loop back to Gate 1 review.
- No direct code updates when client feedback/design changes/screenshots are provided; save assets to
  `.ai-context/change_requests/`, update Spec, and obtain Gate 1 approval first.
- One task per agent execution. Prompt by stable artifact ID (`<feature-slug>`).

---

# Stable Feature Slug & Traceability

Every feature receives a short, kebab-case feature slug when its Spec is created (e.g. `user-authentication`,
`dynamic-form-management`).

The feature slug is the single stable traceability key across:

- Spec: `.ai-context/specs/<slug>.spec.md` (e.g. `dynamic-request-management.spec.md`)
- Plan: `.ai-context/plans/<slug>.plan.md` (e.g. `dynamic-request-management.plan.md`)
- Tasks: `.ai-context/tasks/<slug>.tasks.md` (e.g. `dynamic-request-management.tasks.md`)
- Test Cases: `.ai-context/test_cases/<slug>.test_cases.md` (e.g. `dynamic-request-management.test_cases.md`)
- Git branch: `feature/<slug>`
- Status Board: `.ai-context/status.md`

## CRITICAL RULE — FLAT FILE STRUCTURE (NO SUBDIRECTORIES)

All artifact files inside `.ai-context/` MUST be created directly as **flat files** at the root of their respective
category folder:

- **CORRECT**: `.ai-context/specs/dynamic-request-management.spec.md`
- **CORRECT**: `.ai-context/plans/dynamic-request-management.plan.md`
- **CORRECT**: `.ai-context/tasks/dynamic-request-management.tasks.md`
- **CORRECT**: `.ai-context/test_cases/dynamic-request-management.test_cases.md`

**PROHIBITED**: Never create subdirectories named after the feature slug (e.g., DO NOT create
`.ai-context/specs/dynamic-request-management/spec.md` or `.ai-context/plans/dynamic-request-management/plan.md`).
Feature subdirectories break file linking, automated tracking, and status board traceability.

## CRITICAL RULE — PORTABLE REPOSITORY-RELATIVE PATHS (NO ABSOLUTE PATHS)

All path references, file links, and code paths recorded inside repository artifacts (`.ai-context/`, `status.md`,
specs, plans, tasks, test cases, ADRs, releases) MUST be **relative to the repository root**:

- **CORRECT**: `.ai-context/specs/dynamic-request-management.spec.md`
- **CORRECT**: `src/backend/controllers/requestController.ts`
- **PROHIBITED**: Never write absolute local file system paths (e.g., `C:\Users\Username\...`, `c:/Users/...`,
  `file:///C:/Users/...`, `/home/user/...`).

**Git Portability Mandatory**: Absolute OS paths break when committed to Git because team members and CI/CD pipelines
operate under different user profile directories. All written path references MUST be relative to the repository
workspace root.

## Traceability ID Conventions

- Acceptance Criteria: `<slug>.AC1`, `<slug>.AC2`
- API Contracts: `<slug>.API01`, `<slug>.API02`
- Unit Test Cases: `<slug>.UT01`, `<slug>.UT02`
- Tasks: `<slug>.T01`, `<slug>.T02`

---

# Status Board — `.ai-context/status.md`

`.ai-context/status.md` is the repository project status board. It MUST follow this exact structure:

```markdown
# Project Status Board

_Last updated: YYYY-MM-DD_

## Active Specs

| Spec ID        | Title   | Status   | Owner   | Last Updated | Notes                     |
| -------------- | ------- | -------- | ------- | ------------ | ------------------------- |
| <feature-slug> | <title> | <status> | <owner> | YYYY-MM-DD   | <current state / blocker> |

## Daily Execution Log

### YYYY-MM-DD

- **<feature-slug>**: <what changed today, progress, blocker, or review outcome>
```

---

# 1. Feature Specifications (`.spec.md`)

Create specifications under: `.ai-context/specs/<feature-slug>.spec.md` (instantiated from
`.ai-context/templates/spec.template.md`)

A Spec MUST be authored from an approved BRD requirement (`.ai-context/BRD.md`). A requirement MUST NOT first appear in
the Spec.

## CRITICAL RULE — PROJECT TYPE & SCOPE MATCHING (FULL STACK MANDATE)

- **Full Stack Projects**: Every feature spec (`.spec.md`) MUST define both **Frontend** (`src/frontend/`,
  `tests/frontend/`) and **Backend** (`src/backend/`, `tests/backend/`) Acceptance Criteria, API Contracts, Unit Test
  Scenarios, and Code Deliverables.
- **Frontend-Only Projects**: Specs define UI components, state management, client routing, and frontend unit test cases
  (`tests/frontend/`).
- **Backend-Only Projects**: Specs define API contracts, services, controllers, database models, and backend
  unit/integration test cases (`tests/backend/`).

## Spec Template (sourced from `.ai-context/templates/spec.template.md`)

````markdown
# Spec: <Feature Name>

## Spec ID

<feature-slug>

## Status

Draft | In Peer Review | Changes Requested | Approved | Plan Drafted | Plan Reviewed | Tasks Generated | Under
Development | In QA | Ready for Release | Released (vX.Y.Z)

## Linked BRD

.ai-context/BRD.md#BRD-NNN

## Gate Approvals & History

| Gate                 | Approver        | Date       | Outcome  | Approval Comment                                               |
| -------------------- | --------------- | ---------- | -------- | -------------------------------------------------------------- |
| Gate 1 (Spec Review) | <Approver Name> | YYYY-MM-DD | Approved | "<User/Reviewer approval comment provided during review>"      |
| Gate 2 (Code Review) | <Approver Name> | YYYY-MM-DD | Approved | "<User/Reviewer approval comment provided during code review>" |

## Intent

<One paragraph: what changes, for whom, under what condition>

## Context

- Builds on: .ai-context/architecture.md (<section>)
- Related: .ai-context/specs/<related-spec>.spec.md
- API contract: <link if external>

## API Contract (Mandatory if API surface exists)

### <slug>.API01 — <METHOD> <path>

**Request payload:**

```json
{ "field": "type" }
```
````

**Success response (<code>):**

```json
{ "field": "type" }
```

**Exceptions:** | Code | Condition | Response body | |---|---|---| | 4xx | <condition> | <shape> |

## Acceptance Criteria

1. <slug>.AC1 — Given <state>, when <action>, then <outcome>.
2. <slug>.AC2 — Given <state>, when <action>, then <outcome>.

## Unit Test Cases (spec-derived)

| Test ID     | Maps to AC | Scenario   | Expected   |
| ----------- | ---------- | ---------- | ---------- |
| <slug>.UT01 | AC1        | <scenario> | <expected> |

## Explicitly Out of Scope

- <item>

## Non-Functional Constraints (from constitution.md)

- <latency / throughput / compliance constraint>

````

---

# Gate 1 — Spec Peer Review

Gate 1 is a formal Spec Peer Review before design or coding begins.

## Gate 1 Validation Checklist
- [ ] Reviewer is different from the author.
- [ ] Reviewer identity verified: Gate 1 approval MUST be performed and pushed to Git by the **Project Manager (PM)** or **Technical Lead (TL)**.
- [ ] Git commit/push for Gate 1 approval (`.ai-context/specs/<slug>.spec.md`) matches PM/TL Git identity (`user.email`). Developers MUST NOT push Gate 1 approvals.
- [ ] Intent is one unambiguous paragraph.
- [ ] Every Acceptance Criterion uses Given / When / Then format and has a unique ID.
- [ ] API Contract defines request payload, success response, and exception table (if API surface exists).
- [ ] Explicitly Out of Scope section is present.
- [ ] Non-Functional Constraints are sourced from `constitution.md`.
- [ ] Linked BRD entry is verified.
- [ ] No overlap with an existing Spec.
- [ ] Status explicitly set to `Approved` or `Changes Requested`.
- [ ] Human approval comment captured and logged under `## Gate Approvals & History` in `.ai-context/specs/<feature-slug>.spec.md`.

## Mandatory Gate 1 Git & Role Enforcement
- **Gate 1 Approval Authority**: Gate 1 (Spec & BRD Scope Review) is reserved exclusively for **Project Managers (PM)** and **Technical Leads (TL)**.
- **Git Identity Verification**: The Git commit and push for Gate 1 Spec Approval (`.spec.md` transition to `Approved`) MUST be executed under the Git credentials (`user.email`) of the PM or TL.
- **Developer Prohibition**: Developers/Engineers are strictly prohibited from self-approving or pushing Gate 1 approvals to Git.

## Definition of Ready
A feature is Ready for technical planning & development only when:
- Spec status is `Approved` via Gate 1 by PM/TL and pushed under PM/TL Git identity.
- BRD requirement is linked.
- Acceptance criteria are testable and IDed.
- Architecture review is complete.
- Tasks are generated and approved for execution.

## Mandatory Gate 1 HALT Execution Mechanism
- **STRICT TURN TERMINATION RULE**: When requesting Gate 1 Spec Approval, the agent MUST present the Spec or Multi-Spec Impact Matrix to the user and **IMMEDIATELY END ITS TURN WITHOUT CALLING ANY FURTHER TOOLS**.
- **PROHIBITED PRE-APPROVAL ACTIONS**: The agent MUST NOT generate `.plan.md`, `.tasks.md`, `.test_cases.md`, or write application code before explicit human Gate 1 approval is received in a subsequent prompt from PM/TL.

---

# 2. Implementation Plans (`.plan.md`)

Create implementation plans under:
`.ai-context/plans/<feature-slug>.plan.md` (instantiated from `.ai-context/templates/plan.template.md`)

A Plan MUST be derived from an approved Spec and MUST NOT redefine business intent.

## Plan Template (sourced from `.ai-context/templates/plan.template.md`)
```markdown
# Plan: <Feature Name>

## Derived From
.ai-context/specs/<feature-slug>.spec.md

## Architecture Approach
<components touched, new vs existing, integration points>

## Data Model
<schema changes, migrations, if any>

## Constitution Check
- [ ] No new datastore introduced without ADR
- [ ] Testing discipline matches constitution.md
- [ ] Security posture matches constitution.md

## Explicitly Deferred
- <item with reason>

## Sequencing
1. <high-level build order>
````

---

# 3. Tasks Breakdown (`.tasks.md`)

Create executable tasks under: `.ai-context/tasks/<feature-slug>.tasks.md` (instantiated from
`.ai-context/templates/tasks.template.md`)

Tasks MUST be derived from the approved Plan.

## Task Template (sourced from `.ai-context/templates/tasks.template.md`)

```markdown
# Tasks: <Feature Name>

## Derived From

.ai-context/plans/<feature-slug>.plan.md

## Sequence

- [ ] <slug>.T01 — <independently verifiable work> — Acceptance: <AC ID(s)>
- [ ] <slug>.T02 — <independently verifiable work> — Acceptance: <AC ID(s)>
```

### Task Execution Rules:

- Implement one task at a time.
- Task states: `Not Started` -> `In Progress` -> `In Review` -> `Merged`.
- Never execute multiple tasks unattended in a single batch diff.

---

# 4. Test Cases & Test-First (TDD) Discipline

Test-case specifications belong under: `.ai-context/test_cases/<feature-slug>.test_cases.md`

Cross-feature integration scenarios belong under: `.ai-context/test_cases/_integration.md`

Executable automated test files belong under `tests/frontend/` or `tests/backend/`.

## CRITICAL RULE — MODULAR SPEC-DERIVED TEST CASES & TRACEABILITY

- **Modular Spec Derivation**: All test case specifications (`.ai-context/test_cases/<feature-slug>.test_cases.md`) and
  executable test files (`tests/frontend/`, `tests/backend/`) MUST be derived directly from the corresponding modular
  sub-feature spec (`.ai-context/specs/<feature-slug>.spec.md`).
- **1-to-1 Acceptance Criteria Mapping**: Every test case ID (`<slug>.TC01`, `<slug>.TC02`) MUST explicitly map to an
  Acceptance Criterion (`<slug>.AC1`, `<slug>.AC2`) defined in that modular spec.
- **Full Stack Test Coverage**: For Full Stack projects, tests derived from a modular spec MUST cover both Frontend UI
  scenarios (`tests/frontend/`) and Backend API/Service scenarios (`tests/backend/`) specified in that modular spec.

## EXECUTABLE TEST CREATION & JEST CONFIGURATION PROTOCOL

1. **Post-Gate 1 Artifact Generation**: Once a modular feature spec (`.ai-context/specs/<feature-slug>.spec.md`) is
   approved via Gate 1, the agent automatically generates:
    - Implementation Plan: `.ai-context/plans/<feature-slug>.plan.md`
    - Task Breakdown: `.ai-context/tasks/<feature-slug>.tasks.md`
    - Test Case Specification: `.ai-context/test_cases/<feature-slug>.test_cases.md`
2. **Development Phase Executable Test Generation**: During the development stage (TDD RED phase), before writing any
   implementation code in `src/`, executable automated test code files (`.test.js` / `.test.jsx`) are generated under
   `tests/` matching the project's Jest / test runner configuration.
3. **Hierarchy Mirroring**: Executable test code files under `tests/` MUST sit inside the exact `modules/`, `config/`,
   or `shared/` directory structure matching `src/`.

## Test-First Rule

```text
Test Case Spec → Write Executable Test → Run Test → RED → Implementation → Run Test → GREEN
```

1. Write the failing test first and confirm it fails (RED) for the expected missing capability.
2. Implement code until the test passes (GREEN).
3. Do NOT retrofit tests after coding.

---

# 5. Gate 2 — Code Review

Gate 2 occurs after tests are GREEN and before merging into main.

## Mandatory Role Split for Development & Gate 2

- **Developer / SSE Role**: Developers/Engineers execute TDD (RED ➔ GREEN), implement feature tasks in `src/` and
  `tests/`, run test suites, and submit the completed diff for Gate 2 Code Review under their Git identity.
- **Technical Lead / Reviewer Role**: The Technical Lead (TL) conducts the technical code review, verifies test coverage
  evidence, and approves Gate 2 before merging.

## Gate 2 Validation Checklist

- [ ] Developer implementation diff verified: Code implemented by Developer/SSE under developer Git identity.
- [ ] Every Acceptance Criterion verified individually against the diff by ID (`<slug>.AC#`).
- [ ] Tests were written first and confirmed RED before GREEN.
- [ ] No AI attribution in comments or commit messages (`Generated by AI` prohibited).
- [ ] Security checklist passed: No secrets, credentials, or PII in logs; auth & rate limits enforced.
- [ ] `architecture.md` / `ADR` updated if architectural changes occurred.
- [ ] `status.md` and Spec status updated same day.
- [ ] Human Gate 2 approval comment captured and logged under `## Gate Approvals & History` in
      `.ai-context/specs/<feature-slug>.spec.md`.

## Definition of Done

A feature is Done for merge only when:

- All Acceptance Criteria individually verified.
- TDD cycle verified (RED then GREEN).
- Gate 2 checklist complete.
- Security checks passed.
- `status.md` updated.
- Human code review complete.

## Mandatory Gate 2 HALT Execution Mechanism

- **STRICT TURN TERMINATION RULE**: When development is 100% complete and test suites pass GREEN, the agent MUST present
  the Gate 2 Code Review Request (with completed diff summary and passing test execution results) and **IMMEDIATELY END
  ITS TURN WITHOUT CALLING ANY FURTHER TOOLS**.
- **PROHIBITED PRE-APPROVAL ACTIONS**:
    1. Do NOT create release note files (`.ai-context/releases/RELEASE-vX.Y.Z.md`).
    2. Do NOT update status to `Ready for Release` or `Released (vX.Y.Z)`.
    3. Do NOT scan `BRD.md` for the next requirement or draft the next spec.
    4. Do NOT execute any post-review work until explicit human Gate 2 approval is received in a subsequent prompt.

## Post-Gate 2 Release Logging & Next Spec Transition Protocol

Immediately following successful Gate 2 code review approval for a feature spec (`<feature-slug>`):

1. **Generate/Update Release Plan**: Create or update the project Release Plan document
   (`.ai-context/releases/RELEASE-vX.Y.Z.md`) using `.ai-context/templates/release.template.md`, recording the completed
   spec ID, feature title, spec link, intent summary, and Gate 2 review sign-off.
2. **Status Board Sync**: Update `.ai-context/status.md` and `.ai-context/specs/<slug>.spec.md` to transition the spec
   status to `Ready for Release` or `Released (vX.Y.Z)`.
3. **Automated Transition to Next Spec**: Immediately scan `.ai-context/BRD.md` for the next un-implemented requirement,
   author its modular sub-feature spec (`.ai-context/specs/<next-slug>.spec.md`), and present it for Gate 1 review
   approval.

---

# 6. Handling Client Observations, Feedback & Design Changes

When client feedback, screenshots, or design change requests are provided post-development:

## CRITICAL RULE — NO DIRECT CODE MODIFICATIONS

- The agent MUST NOT modify application code or stylesheets directly upon receiving client feedback or design update
  prompts/screenshots.
- Direct code updates without prior Spec modification and Gate 1 approval are strictly prohibited.

## Step 0: Change Request Document Creation & Asset Archiving

- Whenever screenshots, UI mockups, documents, or attachments accompany a prompt requesting changes or new development:
    1. **Archive Provided Assets**: The agent MUST save the provided screenshot/document into
       `.ai-context/change_requests/CR-<YYYYMMDD>-<slug>-<filename>` (e.g.
       `.ai-context/change_requests/CR-20260830-employee-portal-redesign.png`).
    2. **Create Change Request Document**: Instantiate `.ai-context/change_requests/CR-<YYYYMMDD>-<slug>.md` from
       `.ai-context/templates/change-request.template.md`, documenting the source prompt, description of change,
       affected specs, and asset links.
    3. **Append to Prompt History (CRITICAL APPEND-ONLY RULE)**: Read `.ai-context/prompt_history.md` first, then
       **APPEND** the new execution entry to the bottom of the file (NEVER overwrite existing entries). Include the
       timestamp, prompt summary, relative link to `.ai-context/change_requests/...`, and affected Spec IDs.
    4. **Link in Spec & Impact Matrix**: Include repository-relative links to the archived change request document and
       assets inside the **Multi-Spec Impact Matrix** and under `## Context` / `## Linked Change Request` in all
       affected `.spec.md` files.

## Step 1: Automatic Spec Discovery & Multi-Spec Impact Analysis

- The agent MUST scan `.ai-context/status.md` and `.ai-context/specs/*.spec.md` to identify all affected Feature Spec
  IDs (`<feature-slug>`).
- Traceability mapping: UI components / files shown in screenshots or described in feedback are mapped back to their
  governing Spec IDs.
- **Handling Multi-Spec Changes**: If a single screenshot or feedback prompt impacts multiple features/specs (e.g.,
  Header nav in `auth`, Layout in `dashboard`, Input form in `form-builder`):
    1. The agent generates a **Multi-Spec Impact Matrix** listing all affected Spec IDs, the specific UI/functional area
       in each, and relative links to `.ai-context/change_requests/`.
    2. For modular updates: Draft per-spec delta updates across all impacted `.spec.md` files.
    3. For major cross-cutting redesigns: Propose creating a dedicated **Composite Change Spec** (e.g.
       `specs/ui-redesign-v2.spec.md`) linked to all affected base specs.
- If the observation represents a completely new capability not covered by any existing spec, the agent MUST propose
  creating a new Spec.

## Step 2: Spec Modification (Delta & AC Revisions)

- For each affected Spec ID, the agent drafts updated Acceptance Criteria (e.g. `<feature-slug>.AC_REV1`, updated
  layout/behavior specs).
- Spec statuses transition to `In Peer Review` with pending Gate 1 review.

## Step 3: Unified Gate 1 Approval Request & Halt

- The agent presents the **Multi-Spec Impact Matrix** and all proposed Spec diffs to the user in a single structured
  view.
- The agent explicitly requests **Gate 1 Approval** for the entire change set.
- **The agent MUST STOP and wait for explicit human Gate 1 approval.**

## Step 4: Plan & Tasks Revision

- Once Gate 1 approval is granted, update `.ai-context/plans/<feature-slug>.plan.md` and
  `.ai-context/tasks/<feature-slug>.tasks.md`.

## Step 5: Test-First (RED)

- Update or add test cases under `tests/` or `.ai-context/test_cases/`.
- Run tests to confirm they fail (RED) due to the new expected behavior/design.

## Step 6: Full Implementation (GREEN) & Test Suite Verification

- Execute code and style changes for all tasks to satisfy the updated Spec.
- Run the full test suite and confirm all tests pass cleanly (GREEN).

## Step 7: HALT & Request Gate 2 Review (Step 2 Approval)

- Present the completed diff and passing test results to the user.
- **HALT & WAIT** for explicit human Gate 2 code review approval before merging or finalizing status. Update
  `.ai-context/status.md`.

---

# Repository Branching & Traceability

- Feature Branch: `feature/<feature-slug>`
- Bug-Fix Branch: `fix/<issue-slug>`
- Commit Messages: `Implements <slug>.T01` (Do NOT include AI attribution).
