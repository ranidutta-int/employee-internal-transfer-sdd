# Internal Job Portal

Specification-driven baseline for an internal mobility product. People already employed by an organization discover openings they are eligible for, apply, and move through a configurable approval and transition journey.

The human-resource system of record supplies the workforce. The portal is a Next.js and Node.js modular monolith. It is written so a technology firm, a campus, a laboratory, a hospital, or any other employer can use the same duties and rules, with their own titles.

## Where to read

| Need | Document |
| --- | --- |
| Business baseline | [`.ai-context/BRD.md`](.ai-context/BRD.md) |
| Member journey and edge cases | [`.ai-context/brd/end-to-end-journey.md`](.ai-context/brd/end-to-end-journey.md) |
| Pitch demo (start → complete) | [`.ai-context/brd/pitch-demo-journey.md`](.ai-context/brd/pitch-demo-journey.md) |
| Pages and who sees them | [`.ai-context/brd/product-surface.md`](.ai-context/brd/product-surface.md) |
| HRMS sync, hierarchy, mid-journey fact changes | [`.ai-context/brd/hrms-sync-and-hierarchy.md`](.ai-context/brd/hrms-sync-and-hierarchy.md) |
| Data loss, expedite, mid-journey closure | [`.ai-context/brd/resilience-urgency-and-closure.md`](.ai-context/brd/resilience-urgency-and-closure.md) |
| What is approved to build | [`.ai-context/GATE.md`](.ai-context/GATE.md) and [`.ai-context/status.md`](.ai-context/status.md) |
| Stack and boundaries | [`.ai-context/architecture.md`](.ai-context/architecture.md) |
| Engineering rules for this product | [`.ai-context/constitution.md`](.ai-context/constitution.md) |
| Process rules | `GEMINI.md` and `.agent/` |
| Domain notes | [`agent/README.md`](agent/README.md) |

Feature BRDs live in `.ai-context/brd/<module>/`. High-level specs live in `.ai-context/specs/<module>/`. Both are drafts pending Gate 1 on the baseline.

## Current state

BRD **v0.3** is **Pending Gate 1**. The question log is closed. Start with the journey, then the three operational companions (pages, HRMS sync, resilience).

There is no application code yet. The stack is Next.js, NestJS, PostgreSQL, Prisma, Tailwind CSS, Jest, and Docker. HTTP contracts and a task plan are the next documents when a build is requested.
