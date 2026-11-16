# Project context — Internal Job Portal

One-page orientation. The requirement baseline is `.ai-context/BRD.md`. Engineering rules are `.ai-context/constitution.md`. Shape is `.ai-context/architecture.md`.

## Objective

An internal mobility product for people who already belong to the organization. The HRMS supplies identity and employment facts. Members sign in with Google OAuth, see openings that match their facts, and move through a duty-based approval and transition journey.

The same product must serve employers in any field. Titles are labels. Workflow steps are duties.

## Project type and stack

| Item | Value |
| --- | --- |
| Project type | Full stack |
| Experience | Next.js, Tailwind CSS |
| Service | NestJS on Node.js |
| Architecture | Modular monolith, one repository, one release, two processes |
| Authentication | Google OAuth, matched to the work email on an active HRMS row |
| Database | PostgreSQL |
| Data-access library | Prisma |
| Deployment target | Docker, not tied to one cloud |
| Test runner | Jest |
| Baseline | BRD v0.2. Section 7 is closed. |

## Domain modules

`identity`, `organization`, `experience`, `opportunities`, `mobility`, `transition`, `notifications`, `governance`

## SDD posture

- Control plane: `.agent/` and `GEMINI.md`
- Skills: `config/skills/`
- Knowledge base: `.ai-context/`
- Baseline: BRD v0.2, **Pending Gate 1**. Questions are closed. HTTP contracts and code are not started.
- Specs are business-level drafts. They are not in peer review.
- Plans, tasks, API contracts, and `src/` are not started.

## Context rules

Prefer repository files over chat history. Read only the files needed for the current task. Ignore `.ai-context/prompt_history.md` when gathering implementation context.
