# HRMS sync, hierarchy, and mid-journey fact changes

**Baseline:** `.ai-context/BRD.md` version 0.3  
**Status:** Decided  
**Date:** 2026-11-17

This file explains how workforce data enters the portal, how hierarchy is known, how the 15-minute status path works in the application, and what happens when unit, role, designation, or manager change while a case is open. If this file and the baseline disagree, the baseline wins.

## 1. Source of truth

The HRMS is the only system that creates, updates, or ends a workforce person. The portal stores a **workforce snapshot**: the last accepted copy of those facts. Members never edit mastered fields here. Corrections happen in the HRMS and arrive on the next accepted sync.

Permanent key: **employee id**.  
Google sign-in match key: **work email** (trim, case-insensitive). If work email changes, employee id keeps the same row.

## 2. How hierarchy is known

The portal does not invent an org chart.

| Fact | Where it comes from | How the portal uses it |
| --- | --- | --- |
| Unit (department / faculty / plant / …) | HRMS field `unit` | Eligibility, releasing steward resolution, freezes |
| Primary reporting line | HRMS field `primaryLineEmployeeId` | Line-authority duty |
| Other reporting lines | HRMS list `otherLineEmployeeIds` | Notify by default; approve only if the movement type requires it |
| People who report to me | Derived: every active row whose primary line is me | Profile hierarchy downward slice |
| Upward hierarchy | Walk primary line until the top or a configured depth | Profile hierarchy upward slice |
| Role / designation / skills / probation | HRMS fields | Eligibility hard/warn rules |

So a candidate’s hierarchy is: **HRMS lines → accepted snapshot → duty resolution**. If the snapshot says Manager X is the primary line, X is line authority for that member until a newer accepted snapshot says otherwise.

Profile shows only the member’s upward primary line and the people who report to them. It does not show the whole organization.

## 3. What the HRMS sends

### 3.1 Status events (fast path)

Events: `ACTIVE`, `ENDED`, `SUSPENDED`, `DO_NOT_MOVE`.

**SLA:** from the moment the HRMS records the event, the portal must accept and apply it within **15 minutes** (target).

Why: a leaver or suspended person must not keep applying or approving. This is a safety path, not a UI timer.

### 3.2 Fact updates (normal path)

Fields: name, work email, worker type, unit, primary line, other lines, role, designation, skills, probation and end date, location, movement history, employment start date.

**SLA:** accepted within **24 hours** of the HRMS change.

### 3.3 Transport (decided)

| Path | Mechanism | Interval / trigger |
| --- | --- | --- |
| Primary | HRMS → portal **webhook** `POST` HTTPS JSON | On each status event immediately; on fact change when HRMS emits it |
| Fallback | Portal → HRMS **pull** of changes since cursor | Every **5 minutes** for status; every **1 hour** for facts |
| Assignment write-back | Portal → HRMS `POST` assignment request | When transition reaches HRMS confirmation task |

Auth: mutual agreed secret or signed token on every call (exact scheme in the later API contract). Unsigned or failed auth is rejected and counted as a sync failure.

Idempotency: every payload carries `employeeId` + `hrmsEventId` (or equivalent). Replaying the same event does not create a second row or undo a later event. Ordering: if a newer `hrmsUpdatedAt` was already applied, an older event is ignored and logged.

## 4. How the 15-minute status path is handled in the application

This is not a frontend countdown. It is integration + enforcement.

```text
HRMS sets ENDED / SUSPENDED / DO_NOT_MOVE
        ↓
Webhook fires (or next 5-minute status poll catches it)
        ↓
NestJS identity module validates auth + idempotency
        ↓
Snapshot status updated; open cases for that person stopped
        ↓
Active sessions marked not allowed to act
        ↓
Next apply / approve / complete-task call reads snapshot → refused
```

### Application behaviour after acceptance

1. **Snapshot write** — status on that employee id becomes the new value.
2. **Case stop** — every open application or transition for that person goes to `stopped` with reason `employment_status_changed` (or suspension / do-not-move).
3. **Session** — the service marks the session “cannot act.” The next API call returns refusal. Existing browser tabs do not keep approving offline; each write hits the service.
4. **Notices** — member (if still reachable by policy), open assignees, and people operations are notified that cases stopped.
5. **Admin sync page** — shows last status-event accepted time.

### If the 15-minute target is missed

Until the portal accepts the event, the person may still act on the old snapshot. That is an **SLA miss**, visible on sync health (age of last status sync, dead-letter count). After acceptance, enforcement is immediate. There is no back-dated silent approve; actions already stored stay in the audit, and people operations can rescind incomplete transitions under the normal rescind rules.

### Enforcement on every sensitive write

Before apply, support, decline, approve, waive, or complete task, the service loads the actor’s current snapshot status. If not `ACTIVE` (or if `DO_NOT_MOVE` applies to mobility writes), the write is refused. Checking only the Google token is not enough.

## 5. How fact updates are applied

```text
HRMS changes unit / role / designation / manager / skills / …
        ↓
Webhook or hourly fact pull
        ↓
Snapshot fields updated for that employee id
        ↓
Profile and eligibility use the new facts
        ↓
Open cases: re-resolve duties and re-check eligibility (section 6)
```

Failed sync: administrators see failure and last accepted age. When age > 24 hours, new applications and new decisions pause for everyone (BR-06). Reading remains available.

## 6. Mid-journey fact changes (unit, role, designation, hierarchy, skills)

### 6.1 Frozen vs live

| Stored on the application at submit | Always read live from latest snapshot |
| --- | --- |
| Opening id and movement type | Unit, primary line, other lines |
| Eligibility result and warnings at submit time (audit) | Role, designation, skills, probation, status |
| Member statement | Worker type, location |
| Snapshot timestamp at submit | Releasing / receiving steward resolution for tasks not yet opened |

Audit keeps what was true at apply. Decisions use **live** facts.

### 6.2 Before every decision

The service:

1. Reloads the member’s snapshot.
2. Re-runs hard and warn rules for that opening.
3. Re-resolves who holds the next duty from live unit and lines.
4. If any hard rule now fails → case **stops**; member and people already involved are notified; reason is member-safe (for example “eligibility changed”).
5. If new warnings appear → they must be acknowledged before support or approval.
6. If only steward/line holders changed → see handoff rules below.

### 6.3 Unit changes (Department A → Department B)

Example: member applied while in unit A; HRMS moves them to unit B before release.

| Stage when unit change is accepted | Outcome |
| --- | --- |
| Before line-authority task opens | Line authority from new primary line. Releasing steward will be B’s steward when that step opens. |
| Line-authority task in progress | Current holder keeps the open task (BR-27). New primary line is informed. Later steps use live unit B. |
| Before releasing-steward task opens | Releasing steward resolves to **unit B** (current unit). Unit A is notified that they are no longer the releasing unit on this case. |
| Releasing-steward task already in progress with A | Holder at A keeps that open task. If they release, people operations must confirm the origin is still coherent; if A declines because the person left A, that is a normal decline. If the task is not yet decided and A’s steward becomes inactive for this member, escalate/reassign under normal absence rules. |
| After release, before transition complete | Live unit is B for display; destination and HRMS assignment still follow the opening. Eligibility recheck still runs. |
| Opening becomes nonsensical (for example already in destination unit B and opening is “move to B”) | Hard rule failure on recheck → case stops. |

The product does **not** silently keep unit A as releasing steward after the person’s current unit is B and the releasing task has not started.

### 6.4 Role or designation changes

Recheck eligibility. If the opening required a role/designation and the live snapshot no longer passes → stop. If it still passes → continue; audit shows the change. Approvers see current role/designation on the task screen.

### 6.5 Manager / hierarchy changes

Same as BR-27 / EC-50:

- Open task stays with current holder.
- New line authority is informed and can read the case.
- Tasks not yet opened go to the new line.
- If the current holder is inactive → delegate or escalate.

### 6.6 Skills change

Recheck. Missing required skill → stop. Newly added skill can only help future visibility; it does not reopen a stopped case automatically.

### 6.7 What the member and approvers see

- Case timeline event: “Workforce facts updated from HRMS” with which categories changed (unit, line, role, …), not a raw payload dump.
- Member-safe stop reason if the case stops.
- Task screens always show **current** unit, role, designation, and line, plus the apply-time eligibility snapshot for audit.

## 7. Sync health and operations

Administrators (and people operations read-only) see on the sync page:

- Time of last accepted status event
- Time of last accepted fact sync
- Age of oldest stale concern
- Webhook failure count / dead-letter depth
- Last pull cursor time

Dead-letter: payloads that failed validation after auth are kept for replay by an administrator. Replay is audited. Poison messages that fail repeatedly stay in dead-letter and do not block other employees’ sync.

## 8. Assignment write-back (completing a move)

When the HRMS confirmation task runs, the portal sends employee id, destination unit, role/designation if changed, effective date, movement type, case id. No pay figures.

- Accept → movement may complete when other required tasks are done/waived.
- Reject → stay in transition; show HRMS reason to people operations.
- No answer for 1 business day → people-operations task.

The portal never marks the HRMS confirmation waived.
