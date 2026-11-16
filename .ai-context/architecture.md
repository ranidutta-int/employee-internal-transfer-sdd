# Architecture — Internal Job Portal

Living design note. Update it when an approved plan adds an integration or a module boundary.

## Decision in force

TD-01, confirmed by the request on 2026-11-16: one application. The Next.js experience and the Node.js service are modules of a single deployable. They are not separate products.

Recorded as `.ai-context/decisions/ADR-001.md` (Proposed).

## Choices in force

| Concern | Choice |
| --- | --- |
| Experience | Next.js and Tailwind CSS |
| Service | NestJS on Node.js |
| Store | PostgreSQL through Prisma |
| Tests | Jest |
| Release | Docker, one repository, one release, two processes |
| HRMS | HTTPS JSON. Employee id is the key. Work email matches Google. Assignment confirmation is required and cannot be waived. |

## Intended shape, when implementation is allowed

```text
src/
  frontend/
    app/            route shells only
    modules/        domain UI and client calls
    shared/
  backend/
    app/            process bootstrap, middleware, route mounting
    config/
    modules/        domain behaviour and persistence ports
    shared/
tests/
  frontend/
  backend/
```

Do not create this tree before a spec is Gate 1 Approved and its plan is Plan Reviewed.

## Module boundaries

| Module | Owns |
| --- | --- |
| identity | Workforce snapshot intake, Google sign-in match, session eligibility |
| organization | Units, labels, duties, movement types, delegation, escalation policy |
| experience | Duty-specific home, profile, hierarchy slice, mobility preferences |
| opportunities | Openings, hard and warn rules, match explanation, match counts |
| mobility | Applications, decision chain, withdrawal, freeze effects on cases |
| transition | Checklists, waivers, rescind, HRMS assignment confirmation tracking |
| notifications | The notice matrix, retries, and delivery failure records |
| governance | Observation, steward patterns, audit read, administrator configuration |

A module calls another through its public surface. The mobility module does not reach into identity storage. It asks whether the member may act.

## Boundaries that keep the product industry-neutral

- A workflow step stores a duty, not a job title.
- Customer-facing words for a unit or a duty are configuration.
- Enabling-function tasks are a list on the movement type. An employer that has no separate access team has no access task.
- Industry examples in the BRD are labels. They are not code branches.

## State, at the business level

Opening: draft, open, paused, filled, withdrawn, expired.

Application: submitted, in review, declined, withdrawn, approved for transition, in transition, complete, rescinded, stopped.

These names are the business lifecycle in BRD journeys C and D. They are not a database schema.

## Security shape

- The service enforces duties. The experience hides controls the person cannot use, and still relies on the service.
- General logs exclude workforce facts, application content, internal notes, and tokens (NFR-01).
- There is no local password store for members.
