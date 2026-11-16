# Architecture note

The product is one application: a Next.js experience and a Node.js service. ADR-001 records that decision as Proposed.

Modules and the rule that steps bind to duties, not to an employer's job titles, are in `.ai-context/architecture.md`.

Do not add `src/` until a spec is Gate 1 Approved and its plan is Plan Reviewed. The stack is Next.js, NestJS, PostgreSQL, Prisma, Tailwind CSS, Jest, and Docker (BR-55).
