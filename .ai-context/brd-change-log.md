# BRD change log

## Version 0.2 — 2026-11-16

**Status:** Pending Gate 1. Section 7 is closed.

**Summary:** Closed every open question. Wrote the member journey and the edge-case outcomes.

**Added requirements:** BR-45 through BR-58. Journey file `.ai-context/brd/end-to-end-journey.md`.

**Modified requirements:** BR-06, BR-10, BR-11, BR-13, BR-15, BR-16, BR-18, BR-21 through BR-44. Notable changes from the v0.1 working assumptions: primary line approves and other lines are notified; a panel passes by majority; freshness is 24 hours; one active application through transition; 90-day cooling-off on the same opening; work email plus in-portal notices; backfill notice on and no automatic opening; stack is PostgreSQL, Prisma, NestJS, Tailwind CSS, Jest, and Docker.

**Removed requirements:** The open assumption list.

**API impact:** None. Contracts are still not written.

**Database impact:** PostgreSQL is selected. No schema is written.

**Frontend impact:** None.

**Backend impact:** None.

**Test impact:** Spec criteria updated to the decided numbers. Nothing executed.

**Gate 1 status:** Not submitted.

**Approved by:** Decided in this baseline on 2026-11-16.

---

# Earlier entries


This log records requirement baseline changes. It does not replace `.ai-context/BRD.md`.

## Version 0.1 — 2026-11-16

**Status:** Pending Gate 1

**Summary:** Initial baseline ingested from the client brief of 2026-11-16. No prior baseline existed.

**Added requirements:** BR-01 through BR-44, FR-01 through FR-42, NFR-01 through NFR-03. NFR-04 recorded as open with no number.

**Modified requirements:** None.

**Removed requirements:** None.

**Unchanged requirements:** None. This is the first baseline.

**Affected business domains:** Identity, organization, member experience, openings, decision chain, transition, notifications, governance.

**Affected modules:** identity, organization, experience, opportunities, mobility, transition, notifications, governance.

**API impact:** None. Contracts are not designed.

**Database impact:** None. No database is chosen (Q-12).

**Frontend impact:** None. No application code.

**Backend impact:** None. No application code.

**Test impact:** Spec-derived scenarios drafted. Nothing executed.

**Existing implementation impact:** No implementation exists.

**Architecture impact:** TD-01 recorded (Next.js and Node.js, one deployable). ADR-001 is Proposed.

**Gate 1 status:** Not submitted.

**Approval date:** Pending.

**Approved by:** Pending.

**Approval notes:** Assumed rules in the question log need a business owner before this baseline can be treated as approved policy.
