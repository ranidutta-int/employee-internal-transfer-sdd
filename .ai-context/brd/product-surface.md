# Product surface — pages and who sees them

**Baseline:** `.ai-context/BRD.md` version 0.3  
**Status:** Decided  
**Date:** 2026-11-17

This file defines the experience surface. It is not wireframes. It is the page list, the duty that may open each page, and what empty or refused looks like. If this file and the baseline disagree, the baseline wins.

## How access works

1. The person signs in with Google and matches an active workforce row.
2. The service resolves which duties that person holds for the current request (member always; plus line authority, steward, evaluator, people operations, enabling owner, executive observer, administrator, as the snapshot and configuration say).
3. The experience shows only routes that duty may open. Deep links to a forbidden page return the same refused outcome as an API call: no data, no “you are not allowed to see X’s case” detail that leaks existence where the baseline forbids it.
4. Hiding a menu item is not the control. The service refuses the action.

## Page catalog

| Page ID | Route idea | Purpose |
| --- | --- | --- |
| P-01 | `/sign-in` | Google sign-in only. |
| P-02 | `/home` | Duty-specific home: openings/cases/tasks relevant to that duty. |
| P-03 | `/profile` | Own mastered facts, hierarchy slice, preferences. |
| P-04 | `/openings` | Discover openings that pass hard rules. |
| P-05 | `/openings/:id` | Opening detail, why-listed explanation, apply. |
| P-06 | `/applications/mine` | Own applications and states. |
| P-07 | `/applications/:id` | Own application detail, timeline, member-safe outcomes. |
| P-08 | `/tasks` | Inbox of decision and transition tasks waiting on me (or my delegate cover). |
| P-09 | `/tasks/:id` | One task: acknowledge warnings, support/decline/approve/complete/waive. |
| P-10 | `/openings/manage` | Draft, publish, pause, withdraw openings I may publish. |
| P-11 | `/openings/:id/matches` | Counts by excluding rule; named match list when allowed. |
| P-12 | `/pipeline` | Executive / people-operations journey view: stages, stalls, decline patterns. |
| P-13 | `/admin/organization` | Unit labels, movement types, duties, calendars, holidays, reason codes. |
| P-14 | `/admin/rules` | Default hard/warn rules, freshness is fixed at 24h display, frequency thresholds. |
| P-15 | `/admin/sync` | Sync health: last accepted snapshot age, failed deliveries, dead-letter count. |
| P-16 | `/admin/delegations` | Create/renew time-bounded delegations. |
| P-17 | `/notices` | In-portal notice list for the signed-in person. |
| P-18 | `/cases/:id` (steward/ops/observer) | Case detail for people who hold a duty on that case, with internal notes as allowed. |

Route strings are the product intent. Exact Next.js paths may use a prefix; the page IDs stay stable.

## Duty → page matrix

| Page | Member | Line / Evaluator / Releasing / Receiving | Enabling owner | People operations | Executive observer | Administrator |
| --- | --- | --- | --- | --- | --- | --- |
| P-01 Sign-in | Yes | Yes | Yes | Yes | Yes | Yes |
| P-02 Home | Yes | Yes | Yes | Yes | Yes | Yes |
| P-03 Profile | Own only | Own only | Own only | Own only | Own only | Own only |
| P-04 Openings | Yes | Yes | Yes | Yes | Yes | Yes |
| P-05 Opening detail | If visible | If visible | If visible | Yes | Yes | Yes |
| P-06 My applications | Yes | Yes | Yes | Yes | Yes | Yes |
| P-07 My application detail | Own | Own | Own | Own + cases in their queue | Pipeline cases | Pipeline / intervention cases |
| P-08 Tasks | If any | Yes | Yes | Yes | No approve queue | Reassignment only via recorded intervention |
| P-09 Task detail | If assignee | If assignee/delegate | If assignee | If assignee | No | Reassign with reason; cannot mark HRMS confirmation done |
| P-10 Manage openings | No | Receiving steward of destination | No | Yes | No | Yes |
| P-11 Matches | No | Receiving steward of that opening | No | Yes | Aggregates only | Counts only |
| P-12 Pipeline | No | No | No | Yes | Yes | Yes (read) |
| P-13 Admin org | No | No | No | Read | No | Yes |
| P-14 Admin rules | No | No | No | Read | No | Yes |
| P-15 Sync health | No | No | No | Read | Exception view | Yes |
| P-16 Delegations | No | Own delegation out | No | Yes | No | Yes |
| P-17 Notices | Own | Own | Own | Own | Own | Own |
| P-18 Case (duty view) | Own via P-07 | If on the chain | If they own a transition task | Yes | Yes (no approve) | Yes (intervention only) |

“Own” means the signed-in person’s employee id. A peer cannot open another member’s profile or application.

## What each home shows

| Duty on home (P-02) | Primary panels |
| --- | --- |
| Member | Relevant openings, my applications, tasks waiting on me. |
| Line / evaluator / steward | Tasks waiting on me, stalls past reminder, cases I already decided. |
| Enabling owner | Open transition tasks I own, failed tasks. |
| People operations | Policy queue, freezes, reopen window cases, sync age banner if stale, attention items. |
| Executive observer | Pipeline summary, stalls, decline-pattern attention, exceptions. No approve control. |
| Administrator | Sync health, configuration shortcuts, stuck-case interventions needing a reason. |

## Empty and refused states

| Situation | What the person sees |
| --- | --- |
| No relevant openings | Empty list with “No openings match your current eligibility.” No invent list. |
| Apply on a hidden opening via URL | Refused. Same message as not found for that member. |
| Open another person’s profile | Refused. |
| Executive opens a task approve URL | Page has no approve action; service refuses if called. |
| Snapshot older than 24 hours | Banner: workforce data is stale. Submit and decide controls disabled. Reading still works. |
| Sync failure | Administrators see failure and age on P-15. Members see the stale banner only when age crosses 24 hours. |

## Navigation rules

- Sign-in is the only public page.
- After sign-in, default landing is P-02.
- Notices (P-17) are reachable from every authenticated shell.
- Manage openings and admin pages appear in the shell only when the duty matrix allows them.
- Mobile web uses the same pages. Native apps stay out of scope.
