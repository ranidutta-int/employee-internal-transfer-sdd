# Internal Job Portal

Specification-driven baseline for an internal mobility product. People already employed by an organization discover openings they are eligible for, apply, and move through a configurable approval and transition journey.

The human-resource system of record supplies the workforce. The portal is a Next.js and Node.js modular monolith. It is written so a technology firm, a campus, a laboratory, a hospital, or any other employer can use the same duties and rules, with their own titles.

## Where to read

| Need | Document |
| --- | --- |
| Business baseline | [`.ai-context/BRD.md`](.ai-context/BRD.md) |
| Member journey and edge cases | [`.ai-context/brd/end-to-end-journey.md`](.ai-context/brd/end-to-end-journey.md) |
| What is approved to build | [`.ai-context/GATE.md`](.ai-context/GATE.md) and [`.ai-context/status.md`](.ai-context/status.md) |
| Stack and boundaries | [`.ai-context/architecture.md`](.ai-context/architecture.md) |
| Engineering rules for this product | [`.ai-context/constitution.md`](.ai-context/constitution.md) |
| Process rules | `GEMINI.md` and `.agent/` |
| Domain notes | [`agent/README.md`](agent/README.md) |

Feature BRDs live in `.ai-context/brd/<module>/`. High-level specs live in `.ai-context/specs/<module>/`. Both are drafts pending Gate 1 on the baseline.

## Current state

BRD **v0.2** is **Pending Gate 1**. The question log is closed. Read the member journey first: [`.ai-context/brd/end-to-end-journey.md`](.ai-context/brd/end-to-end-journey.md).

There is no application code yet. The stack is Next.js, NestJS, PostgreSQL, Prisma, Tailwind CSS, Jest, and Docker. HTTP contracts and a task plan are the next documents when a build is requested.
