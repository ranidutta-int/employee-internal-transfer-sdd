# AI usage log

The SDD participant guide asks for a record of what an assistant proposed and what was kept. This log is that record for the baseline. It is not a prompt dump.

| Date | Stage | What was asked of the assistant | What was kept | What was rejected or left open |
| --- | --- | --- | --- | --- |
| 2026-11-16 | Discovery | Turn the client brief into an industry-neutral internal mobility baseline. Find journeys, edge cases, and ambiguities. Do not write application code. | Journeys A–E. Duties instead of industry titles. Both outcomes for frequent movement. Delegation then escalation, with no approval by timeout. Question log with working assumptions labeled Assumed. | Numeric defaults for freshness, service time, decline thresholds, and response time. Those stay as open questions. |
| 2026-11-16 | Specification shape | Follow the participant guide's evidence shape: journey, questions, separated decisions, numbered rules, Given / When / Then. | Module specs and a traceability matrix from FR to acceptance criteria. | API contracts, task plans, and code. Writing them would freeze unanswered business questions. |
| 2026-11-16 | Stack | Record the requested monolith. | ADR-001 Proposed: one Next.js and Node.js application. | Database, data-access library, Node HTTP framework, styling, deployment, and test runner (Q-12). |

The password-reset sample inside the participant guide was used only as a shape. None of its rules were copied into this product.

| Date | Stage | What was asked of the assistant | What was kept | What was rejected or left open |
| --- | --- | --- | --- | --- |
| 2026-11-16 | Decisions | Close every open question with an industry-level operating rule, and write the member journey with edge cases. | 24-hour freshness, 5-business-day escalation, majority panels, primary-line approval, one active application, 90-day same-opening cooling-off, work email plus in-portal notices, PostgreSQL, Prisma, NestJS, Tailwind, Jest, Docker. Journey EC-01–EC-70. | HTTP contracts and application code. Those were not requested. |
