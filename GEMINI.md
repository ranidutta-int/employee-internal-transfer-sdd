# INT AI-First Engineering Policy

## Authority

Follow the INT SDD Blueprint V1.0.

The project repository is the primary source of project context.

Do not rely on chat history as the project's permanent source of truth.

---

## Artifact Authority

Use the following hierarchy:

1. constitution.md
2. BRD.md
3. Approved Spec
4. Approved Plan
5. Approved Tasks
6. Test Cases
7. Existing Implementation

Code is generated output.

Do not use implementation code to silently redefine requirements.

---

## Mandatory Lifecycle

BRD ↓ Spec ↓ Gate 1 ↓ Plan ↓ Architecture Check ↓ Tasks ↓ Test-first RED ↓ Implementation GREEN ↓ Gate 2 ↓ Merge ↓
Release ↓ Production Support

---

## Core Rules

- No implementation without an approved Spec.
- No direct code edits when client feedback/screenshots are provided; auto-detect affected Spec ID(s) (or generate a
  Multi-Spec Impact Matrix), update Specs, and wait for Gate 1 approval first.
- No implementation before tests exist and are confirmed RED.
- One task per agent execution.
- Prompt by stable artifact ID.
- Preserve existing implementation.
- Do not invent requirements.
- Do not bypass Gate 1.
- Gate 1 HALT: When presenting a Spec for Gate 1 approval, the agent MUST immediately stop calling tools and end turn.
  No plan, task, or code generation is permitted before human Gate 1 approval.
- Complete all development tasks and confirm tests pass GREEN before requesting Gate 2 approval.
- Gate 2 HALT: When requesting Gate 2 approval, the agent MUST present the code review request and immediately stop
  calling tools and end turn. No release notes, status board release sync, or next spec transition is permitted before
  human Gate 2 approval.
- Do not bypass Gate 2.
- Keep architecture.md current.
- Keep status.md current.
- Append-Only Prompt History: .ai-context/prompt_history.md is a mandatory, append-only chronological log. The agent
  MUST NEVER overwrite existing prompt entries when logging change requests or new prompt executions.
- Do not expose secrets or PII in project artifacts.
- Human owns all merged code.

---

## Context Rules

Prefer repository context over chat history.

Read only the files required for the current task.

Do not scan the entire repository for localized work.

Use .agentignore aggressively.

---

## Agent Behavior

When requirements are ambiguous:

STOP.

Do not guess.

Identify the ambiguity and request clarification or update the relevant artifact.

When implementation is significantly wrong:

STOP.

Fix the Spec or Plan instead of repeatedly prompting against ambiguity.
