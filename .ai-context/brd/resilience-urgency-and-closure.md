# Resilience, urgency, non-response, and mid-journey closure

**Baseline:** `.ai-context/BRD.md` version 0.3  
**Status:** Decided  
**Date:** 2026-11-17

This file covers data loss, long chains and urgency, people who do not respond, and what happens when a journey closes in the middle. If this file and the baseline disagree, the baseline wins.

## 1. Data loss and recovery

### 1.1 What “data loss” means here

| Loss | Meaning |
| --- | --- |
| Sync event loss | HRMS sent a change the portal never accepted |
| Portal database loss | Snapshot, cases, or audit damaged or restored from backup |
| Notice loss | Email or in-portal delivery failed |
| HRMS reject / silence | Assignment write-back did not complete |

### 1.2 Decisions

| Topic | Decision |
| --- | --- |
| System of record for people | HRMS. After a portal restore, workforce facts are rebuilt by full or cursor-based pull from HRMS before mobility writes reopen. |
| System of record for cases | Portal database. Cases are not stored in the HRMS. |
| Backup | Continuous or daily database backups. **RPO 24 hours** maximum for case data. **RTO 8 hours** target to restore service. |
| After restore | 1) Restore DB. 2) Run HRMS fact + status pull to refresh snapshots. 3) Until status pull succeeds, mobility writes stay paused. 4) Administrators confirm sync health before unpausing. |
| Lost webhook events | Fallback 5-minute status poll and hourly fact poll close the gap. Dead-letter replay for failed accepted-auth payloads. |
| Duplicate events | Idempotent on `hrmsEventId`; safe to replay. |
| Lost email notice | In-portal notice remains source for “notified in product.” Email retries 3 times over 24 hours. Decision is not undone. |
| Partial write | A decision or apply is atomic: either stored with audit, or not stored. No half-applied approval. |

### 1.3 What we do not do

- We do not invent workforce rows after restore.
- We do not auto-approve cases that were mid-flight during an outage.
- We do not delete audit to “clean” a restore.

### 1.4 Application handling (short)

```text
Outage or restore
        ↓
Bring database back
        ↓
Pull HRMS status + facts until cursors are current
        ↓
Keep apply/decide paused if status freshness unmet
        ↓
Resume; stuck tasks use normal escalation / reassignment
```

## 2. Long approval chains and urgency

### 2.1 Default chain length

Default movement type: line authority → evaluation → releasing steward → people operations → receiving steward (5 human steps, plus optional panel seats and optional veto).

A movement type may add executive sign-off or a compliance veto. There is **no hard maximum** number of configured duties, but product guidance is: keep the default at five; add gates only when policy requires them.

### 2.2 Urgency (expedite) — decided

| Topic | Decision |
| --- | --- |
| What it is | A flag on an **opening** or on a **case** (set by people operations or the receiving steward) called **Expedite**. |
| What it is not | Silent skip of duties. Silence still never approves. |
| Effect on SLA | Reminder at **1 business day**. Escalation at **2 business days**. Then skip-level, then people operations on the same hop pattern as normal, but with the shorter clock. |
| Effect on notices | Assignees get an expedite-marked notice; people operations is copied when expedite is set. |
| Effect on chain | Duties are **not** removed. Combined same-person duties still collapse. |
| Who may set it | People operations, or receiving steward of that opening, with a reason code. Member cannot self-expedite. |
| Who may clear it | People operations. |
| Audit | Setting and clearing expedite are audited. |

Emergency with a vacant top-of-tree or missing people-ops incumbent still follows EC-25 / BR-36 (insert executive sign-off when the only people-ops person is the applicant). Expedite does not replace that.

### 2.3 Application handling

```text
Case or opening marked Expedite + reason
        ↓
Service clock uses 1-day reminder / 2-day escalate
        ↓
Notices show Expedite
        ↓
Chain duties still required; no auto-approve
```

## 3. Non-response (already decided, spelled for product)

| Clock | Normal | Expedite |
| --- | --- | --- |
| Reminder to assignee (+ delegate if any) | 2 business days | 1 business day |
| Escalate to skip-level | 5 business days | 2 business days |
| Escalate to people operations | +3 business days after skip-level silent | +2 business days |
| Executive exception | +5 business days people-ops silent | +3 business days |

Rules that never change:

- Elapsed time never approves.
- One delegate hop, max 30 days.
- Applicant cannot be their own delegate.
- Vacant primary line with no delegate → people operations immediately (no waiting for 5 days).
- Administrator may reassign a stuck task to an active duty holder or people operations with a reason; cannot mark HRMS confirmation done.

Member sees: current stage, waiting on which duty (label), and whether the case is escalated — not private notes.

## 4. Journey closes in the middle

### 4.1 Stop reasons (member-safe labels)

| Code | When | Member-safe label |
| --- | --- | --- |
| `withdrawn_by_member` | Member withdraws before transition | You withdrew this application. |
| `declined` | Any duty declines | Not approved — {reason catalog label}. |
| `position_filled` | Seat reserved by another transition / opening filled | The position is no longer available. |
| `opening_withdrawn` | Opening withdrawn while still in review | The opening was withdrawn. |
| `opening_expired` | Opening expired in review | The opening expired. |
| `eligibility_changed` | Hard rule fails on recheck / workforce fact change | Your eligibility changed, so this application cannot continue. |
| `employment_status_changed` | Ended / suspended / do-not-move | This application was stopped because your employment status changed. |
| `frozen` | Unit/type freeze (pause, not terminal) | This application is paused by a freeze. |
| `rescinded` | People ops rescinds incomplete transition | This move was cancelled before it took effect. |
| `stopped_admin` | Recorded intervention stop | This application was stopped by people operations. |

`frozen` is pause. The others in this table are terminal for that application unless reopen rules apply (`declined` within 10 business days by people ops only).

### 4.2 Who is told

| Event | Member | Open assignees | Prior deciders | People ops | Executive |
| --- | --- | --- | --- | --- | --- |
| Withdraw / decline / filled / eligibility / employment stop | Yes | Yes | Yes | Yes on decline, eligibility, employment | Exception on employment / mass impact |
| Freeze start/end | Yes | Yes | — | Yes | Exception |
| Rescind | Yes | Open transition owners | Chain | Yes | Summary |

### 4.3 Cleanup ownership

| Stage when closed | Portal does | Humans may still do outside |
| --- | --- | --- |
| In review (before transition) | Cancel open tasks; store terminal state; notices; no HRMS assignment call | Nothing required in HRMS for a move that never started |
| In transition, before HRMS accept | Cancel open portal tasks; rescind/stop state; **do not** send or must send compensating cancel if a request was already sent and HRMS supports cancel — if HRMS has no cancel API, people operations gets a manual “confirm HRMS has no pending assignment” task | People ops confirms HRMS and any half-finished enabling work |
| After some enabling tasks done, before complete | Same as above; completed enabling tasks are listed on the case as “done before stop”; owners get a notice to reverse outside the portal if their system needs it | Enabling owners reverse in their own systems; portal does not auto-call payroll/access APIs beyond tracking |
| After complete | No in-place undo; new movement required | Normal |

The portal **tracks** completion of enabling tasks; it does not roll back payroll or door access itself. On mid-transition stop, it always:

1. Leaves a clear case timeline of what was done.
2. Opens a people-operations cleanup checklist item when any transition task was already marked done or an HRMS request was sent.
3. Never marks the movement `complete`.

### 4.4 Application handling

```text
Stop trigger (withdraw / decline / fill / eligibility / status / rescind)
        ↓
Atomic state change + audit
        ↓
Cancel open tasks
        ↓
Notices per matrix
        ↓
If transition had started → people-ops cleanup item
        ↓
Member sees member-safe label on My applications
```

## 5. Interaction with workforce fact changes

If a mid-journey unit/role/manager change causes a hard eligibility failure, the stop reason is `eligibility_changed`. Detail for duties: timeline shows which fact categories changed. See `.ai-context/brd/hrms-sync-and-hierarchy.md` section 6.

If employment status changes, reason is `employment_status_changed` and all cases stop, including in transition (then cleanup rules above apply).
