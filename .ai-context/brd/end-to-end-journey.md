# End-to-end journey

**Baseline:** `.ai-context/BRD.md` version 0.3  
**Status:** Decided  
**Date:** 2026-11-17

This is the story of one member moving inside the organization, then the situations that interrupt that story and the outcome for each. The same duties apply in a campus, a laboratory, a hospital, a plant, or a technology firm. Titles on screen are the employer's words. The steps below use duties.

Authoritative rules are BR-01 through BR-68 in the baseline. Operational detail for pages, HRMS sync, hierarchy, data loss, urgency, and mid-journey closure lives in:

- `.ai-context/brd/product-surface.md`
- `.ai-context/brd/hrms-sync-and-hierarchy.md`
- `.ai-context/brd/resilience-urgency-and-closure.md`

If this narrative and the baseline ever differ, the baseline wins.

## People in the story

| Who | What they do here |
| --- | --- |
| Member | Sees their own facts and their own openings. Applies or withdraws. |
| Line authority | The primary reporting line. Supports or declines before anyone else evaluates. |
| Evaluator | One person, or a panel. Pass or decline. |
| Releasing steward | Accountable for the current unit. Releases, names a later date, or declines. |
| People operations | Checks policy, confirms the effective date, and owns reopen, freeze, and rescind. |
| Receiving steward | Accepts the member into the destination unit. |
| Enabling-function owner | Completes one task the movement type turned on, such as a payroll effective date or a door pass. |
| Executive observer | Reads the pipeline. This duty has no approve button. |
| Administrator | Configures labels, types, and calendars. A case change from this duty is a recorded intervention. |

## Decisions that shape the story

| Topic | Decision |
| --- | --- |
| How someone appears | Only an accepted HRMS sync. No manual add. |
| Sign-in | Google, matched to the work email on the row. The permanent key is the employee id. |
| What they see | Openings that pass every hard rule. Warn rules stay visible and must be acknowledged before any support or approval. |
| Probation | Hidden on a default movement type. A type may loosen that. |
| Time in the current assignment | Under 6 months is hidden on a default movement type. A type or opening may change the number. |
| Frequent unit changes | A warn, not a hide, when the member has changed unit 2 or more times in the last 12 months. An opening may switch that to hide, or turn it off. |
| How many applications | One active application, from submit through transition. |
| After a decline | 90 days before applying again to that same opening. A different opening is allowed once no application is active. |
| Default chain | Line authority, evaluation, releasing steward, people operations, receiving steward. |
| Extra reporting lines | Informed. They do not approve unless the movement type says they must. |
| Panel | Majority passes. A tie declines. A vacant seat is escalated, not counted as a pass. A named veto duty can stop a majority. |
| Absence | Active delegate for up to 30 days, one hop only. Then skip-level, then people operations. Silence never approves. |
| Service time | Reminder after 2 business days. Escalation after 5. The clock uses Monday–Friday plus the organization's holidays, in the headquarters timezone. The clock pauses on a freeze and does not reset when a delegate takes the task. |
| Snapshot age | Older than 24 hours pauses new applications and new decisions. Ended employment, suspension, and a do-not-move flag apply as soon as they are accepted, with a target of 15 minutes from the HRMS event (webhook + 5-minute status poll). |
| Hierarchy | From HRMS primary/other lines only. Profile = upward line + direct reports. |
| Mid-journey fact change | Live unit/role/line/skills for routing and recheck; apply-time eligibility kept for audit. Unit change before release → new unit’s releasing steward. |
| Urgency | Expedite shortens reminder/escalate to 1 and 2 business days. Does not skip duties. |
| Data loss | Case DB RPO 24h / RTO 8h. After restore, refresh from HRMS before mobility writes. |
| Pay | No amounts in this portal. People operations may have a task to confirm pay in the HRMS. |
| Notices | In the portal and to the work email. No SMS. No personal mailbox. |
| After approval | Checklist from the movement type. HRMS assignment confirmation is always on it and cannot be waived. |
| Mid-journey stop | Member-safe stop catalog; if transition started, people-ops cleanup item; no false “complete.” |
| Audit | Kept 7 years after the case closes. |
| Pages | Duty → page matrix in product-surface.md. |

## The happy path

### 1. The HRMS sends the person

The member does nothing. The HRMS sends a row with a stable employee id, work email, name, status, worker type, unit, primary reporting line, any other lines, role, designation, skills, probation and its end, location, movement history, and employment start date.

The portal accepts that over HTTPS JSON: webhook first, pull fallback. It stores the snapshot. There is no screen to type a person in. Hierarchy on profile is built only from those lines.

Status changes (employment ended, suspended, do-not-move) are accepted with a target of 15 minutes from the HRMS event — not a UI timer: webhook or the 5-minute status poll updates the snapshot, open cases stop, and the next apply/approve is refused. Other facts arrive within 24 hours (hourly fact pull if webhooks are quiet).

### 2. The member signs in

They choose Google. The portal trims the email, ignores capitals, and matches it to the work email of an active row.

They land on a home that shows three things: openings that pass their hard rules, their own cases, and tasks waiting on them.

### 3. They read their profile

Profile shows designation, role, skills, unit, probation, worker type, and movement history. The hierarchy shows their upward primary line and the people who report to them.

Those fields have no save button. A wrong manager or a missing skill is corrected in the HRMS and arrives on the next sync. The profile says that.

They may save mobility preferences and, later, a statement on an application. A preference can only remove openings. It cannot reveal one a hard rule hid.

### 4. They see relevant openings

A publisher (the destination's receiving steward, people operations, or an administrator) has published an opening: movement type, destination unit, seat count (default 1), and rules marked hard or warn.

The member's list is calculated from the snapshot:

- Current probation on a default type: hidden.
- Fewer than 6 months in the current assignment on a default type: hidden.
- Required skill missing: hidden.
- Required skill data not sent by the HRMS at all: hidden, and the publisher sees "skill data unavailable" rather than "this person lacks the skill."
- Worker type other than employee, on a default type: hidden.
- Two or more unit changes in the last 12 months: shown, with a warning.

They can open "why this opening." The answer lists the rules that passed and the warnings. There is no score and no rank.

A colleague with different skills or a different probation flag sees a different list.

### 5. They apply

They submit once. The application stores the rule result, the warnings, and the snapshot time. Their statement, if they wrote one, is stored on the application.

Home now shows the case as submitted. The line authority and the member each get an in-portal notice and a work-email notice.

### 6. The line authority supports

The line authority opens the task, sees the warnings, acknowledges them, and supports.

Evaluation opens. The releasing steward is not asked yet.

### 7. Evaluation passes

The evaluator passes. On a panel, a majority must pass. Each person records their own outcome.

### 8. The current unit releases

The releasing steward releases and proposes an effective date. The date is today or later, and at most 90 days away.

If the organization has turned on "snap to the next pay-period start," the date moves to that start. That setting is off until they turn it on.

### 9. People operations approves

People operations checks policy, confirms the effective date, and approves. They still see every warning, and they acknowledge them.

### 10. The destination accepts

The receiving steward approves. If this person already evaluated the case, this is still a second task, because release and people operations sat between the two duties. The screen says why both exist: one was the evaluation, one is the acceptance of the seat.

The chain is complete. One seat is reserved. Transition starts.

### 11. The move is carried out

The checklist always contains "HRMS accepts the new assignment." Other tasks exist only when this movement type enabled them. An employer with no separate access team has no access task.

Owners mark tasks done. People operations may waive an optional task with a reason. The HRMS confirmation cannot be waived.

The assignment request sent to the HRMS carries the employee id, destination unit, role and designation when the opening changes them, effective date, movement type, and case id. It carries no pay figures.

Independent tasks can finish while others are open. The HRMS confirmation runs last unless the movement type named earlier predecessors.

### 12. The move is complete

The HRMS accepts. Every required task is done or waived. The member sees the case as complete. People operations is told. The releasing steward receives one notice that the origin unit may now have a vacancy. No opening is created for them. They publish one themselves if they want it.

The executive observer sees this in the pipeline and in the daily summary. They did not receive a notice for each routine approval along the way.

## When the path breaks

Each row is the situation, the outcome, and the reason that outcome is the fair one in an ordinary employer.

| ID | Situation | What happens | Why |
| --- | --- | --- | --- |
| EC-01 | The Google account is a personal mailbox, or it matches no active row. | Sign-in is refused. No row is created. There is no screen to link a different mailbox. | The workforce row is the identity. A personal mailbox must not attach itself to an employee. |
| EC-02 | The HRMS has already ended the employment. | Sign-in is refused. Open cases are already stopped. | A leaver must not apply or approve. |
| EC-03 | Employment ends while a case is open, including an emergency end. | Every open case for that person stops. Their session can no longer act. Target: the status is accepted within 15 minutes of the HRMS event. | The case is about a person who still works here. |
| EC-04 | The snapshot says suspended, or do-not-move. | The same stop as EC-03. A movement type cannot turn this off. | Suspension and a do-not-move flag are employment decisions, not something an opening overrides. |
| EC-05 | The last accepted snapshot is older than 24 hours. | New applications and new decisions pause. People can still read. Administrators see the age. A banner says the workforce data is stale. | Overnight sync plus a day of grace is enough. Beyond that, approvers would be acting on a leavers list. |
| EC-06 | The HRMS feed fails. | Administrators see the failure and the age. EC-05 applies once the snapshot crosses 24 hours. Cases already stored are not deleted. | A failed feed must be visible, and it must not invent people. |
| EC-07 | The member says their manager or skills are wrong. | Profile stays read-only and tells them the correction is made in the HRMS. | Two personnel files drift apart. This portal shows the HRMS; it does not edit it. |
| EC-08 | The HRMS sent no skills, and the opening requires a skill. | The opening is hidden. The publisher's count says "skill data unavailable," not "lacks the skill." | Missing data is not the same as a failed skill. |
| EC-09 | The member is in probation on a default movement type. | The opening is hidden. A movement type that is explicitly about leaving a training assignment may set the rule to warn, or off. | Probation is a normal hold. Some employers still need a path out of a training seat. |
| EC-10 | The member changed unit twice in the last 12 months. | The opening is shown. A warning is attached. Every approver acknowledges it before they can support or approve. | Frequent movement is a signal, not automatically a block. The destination should see it. |
| EC-11 | That opening was configured to hide frequent movers instead. | The opening is absent for this member. | Both outcomes exist. The opening chooses. The default is the warning in EC-10. |
| EC-12 | The member has been in the current assignment for under 6 months. | Hidden on a default type. The type or the opening may set a different number, including zero. | Six months is the usual minimum before an internal move. It is a default, not a law for every employer. |
| EC-13 | The worker type is contingent, intern, or a joint appointment. | Hidden on a default type. The movement type may list the types it allows. | Default openings are for employees. Other types are a deliberate choice. |
| EC-14 | A credential is missing, or the destination is a restricted unit. | Hidden when the rule is hard. Shown with a warning when the rule is warn. | Regulated work needs a gate. The opening decides which credentials are hard. |
| EC-15 | The opening is limited (a sensitive seat). | It does not appear in ordinary discovery. It still uses rules. A typed list of named people is refused. | Sensitive seats get tighter rules, not a private shortlist. |
| EC-16 | Nobody matches. | The publisher sees a count of zero and which rules exclude people. They change the rules or leave the opening open. They cannot push it onto ineligible people. | An empty opening is a rule problem, not a reason to bypass eligibility. |
| EC-17 | The member tries to save a preference that names a hidden opening. | The opening stays hidden. | Preferences narrow. They do not grant eligibility. |
| EC-18 | The member double-clicks apply, or submits again to the same opening. | One application. The second attempt shows the existing one. | A double click must not create two cases. |
| EC-19 | They already have an application in review or in transition. | The new submit is refused. They withdraw the active one first, if transition has not started. | Two units must not both be planning on the same person. |
| EC-20 | They were declined and apply to the same opening within 90 days. | Refused. People operations may waive the 90 days with a reason. | Immediate reapply to the same seat creates noise. A different opening is still allowed once nothing else is active. |
| EC-21 | They were declined and apply to a different visible opening, and nothing else is active. | Accepted, subject to the hard rules. | A decline is about that opening, not a ban. |
| EC-22 | They submit to an opening their list does not show. | Refused. | The list is the eligibility decision. A direct link does not skip it. |
| EC-23 | The person who would approve is the applicant. | That task does not stay with them. It goes to their delegate, or it escalates. | Nobody approves their own move. |
| EC-24 | The only people-operations person is the applicant. | An executive sign-off step is inserted for that case. The observer duty still has no approve button; the sign-off duty does, on this case only. | A one-person people team cannot be the policy approver of their own move. |
| EC-25 | There is no line authority, and no delegate. The member is at the top of the tree. | The step is recorded as "no incumbent" and moves to people operations immediately. It does not sit for 5 days. | A vacant top of the tree must not freeze the only path. |
| EC-26 | The approver is on leave and has a delegate. | The delegate receives the open task and the notices. The audit stores both names. The service clock keeps running. | Cover is normal. Delegation must not buy a fresh SLA. |
| EC-27 | The delegate is also unavailable. | One hop only. The task escalates. It is not delegated again. | Chains of stand-ins hide the decision. |
| EC-28 | Someone sets the applicant as the delegate. | The delegation is refused. | Cover must be another person. |
| EC-29 | A delegation is entered for more than 30 days. | Refused. They may renew when it ends. | Standing delegations become invisible approvers. |
| EC-30 | No delegate, and 5 business days pass with no decision. | A reminder went out at 2 business days. At 5, the task escalates to the skip-level. After 3 more silent business days it sits with people operations. If people operations is silent for 5 business days, the executive observer gets an exception. The case is still undecided. | Chase the humans. Do not approve by silence. Weekends and configured holidays do not count. A freeze pauses the clock. |
| EC-31 | Two duties in a row are the same person. | One task. They record an outcome for each duty. A decline on either stops the case. | A small organization should not click approve twice with no new information. |
| EC-32 | The same person evaluates and, later, accepts as receiving steward, with other duties in between. | Two tasks, with a line of explanation. | Those are different questions: "can they do the work" and "does the unit accept them after release and policy." |
| EC-33 | The HRMS also sent a second reporting line. | The second line is notified when the application is submitted. They have no task. A movement type that says "all lines must support" gives them a task. | Most matrix lines need to know. They do not all need a veto. The type opts into the veto. |
| EC-34 | A panel of three: two pass, one declines. Quorum was not changed. | The case continues. The dissent stays on the record. | One dissent should not silently kill an internal move. Majority is the default. |
| EC-35 | The panel ties. | The case declines. | A tie is not a pass. |
| EC-36 | A panel seat is vacant. | That seat escalates like any missing approver. It is not dropped from the count and not treated as a pass. | A short panel must not lower the bar by accident. |
| EC-37 | The movement type names a veto duty, and that duty declines while the majority passed. | The case stops. | Regulated work can require a named veto. It is on the type, not on every opening. |
| EC-38 | An approver supports while a warning is still unacknowledged. | The support is refused until they acknowledge it. | The warning exists so the destination sees frequent movement, a loosened probation, or a similar signal. |
| EC-39 | A decline is saved with no reason code. | Refused. | The member must see a member-safe reason. The catalog includes skills gap, timing, retain in current unit, another candidate selected, policy, credentials, position withdrawn, and other. "Other" requires a member-safe sentence. |
| EC-40 | The member can see the decline. | They see the stage, the outcome, and the member-safe label. They do not see internal notes or other applicants. | Evaluators need private notes. Members need a real reason. |
| EC-41 | The releasing steward chooses retain. | The case declines with the retain reason. The history keeps it. | Keeping someone is a decision, not a deleted application. |
| EC-42 | People operations reopens within 10 business days, with a reason. | The case returns to the duty that declined. The decline stays in the history. | A single steward should not be unreviewable. The window is short so old cases do not restart quietly. |
| EC-43 | They try to reopen on day 11. | Refused. The member applies again, and EC-20 applies if it is the same opening. | After 10 business days the path is a new application. |
| EC-44 | The member withdraws before transition. | The case is withdrawn. Open assignees and people who already decided are told. | Withdrawal is the member's right until the organization has started to carry the move out. |
| EC-45 | The member tries to withdraw after transition has started. | Refused. They ask people operations to rescind. | Tasks may already have been done. Rescind is the controlled stop. |
| EC-46 | Another applicant's transition starts and takes the last seat. | This in-review application stops. The member is told the position is filled. No transition starts for them. | The seat is reserved when transition starts, first stored case first. |
| EC-47 | The chain completed, and transition has already started, when the opening is withdrawn. | This transition continues. In-review applications stop. | A selected person is past the posting. People still in review are not. |
| EC-48 | The opening is paused. | Applications stay. They do not advance. The service clock pauses. | A pause is a hold, not a cancellation and not a silent continue. |
| EC-49 | The publisher tightens a rule while the case is open, and the member now fails a hard rule. | The next advance is refused. People already involved are told. New warnings must be acknowledged again. | Eligibility is true at each decision, not only on the day of apply. |
| EC-50 | The primary reporting line changes while a task is in progress. | The person who already holds the task keeps it. The new line authority is told and can read the case. Tasks not yet opened follow the new line. If the holder becomes inactive, EC-26 or EC-30 applies. | The person mid-review has the context. The new manager should not be surprised. |
| EC-51 | An administrator freezes the unit or the movement type. | Affected cases pause in place. New applications into that unit are refused. The service clock pauses. Members and assignees are told at the start and at the end. | A hiring freeze that still completes moves is not a freeze. |
| EC-52 | A steward declines 5 times in a row, or declines more than 70% of at least 5 decisions in 90 days. | People operations and the executive observer see an attention item. The steward still holds the duty. Nobody is auto-approved. | The pattern should be visible. Removing the steward is a human decision outside this case. |
| EC-53 | An executive observer looks for an approve action. | There is none. A movement type may add a separate executive sign-off step. Only that step approves, and only on those cases. | Seeing the company is not the same as deciding a case. |
| EC-54 | An administrator marks a case approved with no intervention record. | Refused. | Configuration is not a back door. |
| EC-55 | A case is stuck and an administrator reassigns the task. | Allowed only to an active person who holds that duty, or to people operations, and only with a reason stored on the case. They cannot mark the HRMS confirmation done. | Someone must be able to unstick a vacant seat without pretending the HRMS accepted the move. |
| EC-56 | The effective date is yesterday, or more than 90 days ahead. | Refused. | Back-dated moves and open-ended dates break payroll periods and the audit. |
| EC-57 | A transition task fails. | That task stays open. The owner and people operations are told. The member is told. An exception goes to the executive observer. A task that does not depend on it can still be completed. | One broken door-pass request must not cancel an unrelated payroll task, and it must not complete the move. |
| EC-58 | Someone waives the HRMS confirmation. | Refused. | The HRMS is the system of record. A waiver would leave two versions of the assignment. |
| EC-59 | The HRMS rejects the assignment. | The movement stays in transition. People operations sees the reason the HRMS returned. | Rejection is a fact. The case must not show complete. |
| EC-60 | The HRMS does not answer for 1 business day. | People operations receives a task. The movement stays in transition. | A silent HRMS is an operations problem, not a completed move. |
| EC-61 | People operations rescinds before the effective date, while the movement is incomplete. | Open tasks cancel. The chain is told. The state is rescinded. | There is time to stop a move that has not taken effect. |
| EC-62 | Someone tries to undo a completed move in place. | Refused. A reversal is a new movement. | History stays intact. The way back is the same governed path. |
| EC-63 | A secondment completes. | The expected end is stored. People operations is reminded 30 days before that date. The return is a new movement, already identified, and it does not run by itself. | Dated assignments need a reminder. They do not auto-transfer a person. |
| EC-64 | The work email of a notice bounces. | The in-portal notice remains. The email is retried 3 times over 24 hours. The failure is visible. The decision stands. | A mailbox problem must not rewind an approval. |
| EC-65 | A screen or a task asks for a salary figure. | There is no such field. A movement type may add "confirm compensation in the HRMS," which stores done or not done, never an amount. | Pay belongs in the HRMS. Copying it here widens who can see it. |
| EC-66 | The movement type has no access task, no facilities task, and no payroll task. | The only checklist item is HRMS assignment confirmation. | An employer that does not have those teams must not wait for them. |
| EC-67 | The member opens another person's profile, application, or notices. | Refused. | The journey is visible to the duties that hold it, not to peers. |
| EC-68 | A named list of everyone who matches an opening is opened by the receiving steward or by people operations. | The list is shown, and the view is audited. | They are staffing the seat. The view must be traceable. |
| EC-69 | An executive observer or an administrator opens that same eligible-population list. | Refused. The executive sees in-flight cases and aggregate counts. The administrator sees counts. | Observation and configuration are not a workforce search. |
| EC-70 | Two chains become ready for the last seat at the same moment. | The first transition record stored reserves the seat. The other case stops as position filled. | One stored order is clearer than a meeting to pick a winner after both were approved. |
| EC-71 | Member opens `/admin/sync` or another member’s `/applications/:id`. | Refused. Home and menus only show allowed pages. | Duty → page matrix is the product surface. |
| EC-72 | HRMS webhook is down; status Ended is set at 10:00. | By the next status poll (≤ 5 minutes cadence), and within the 15-minute target overall, portal accepts Ended, stops cases, refuses writes. | Fallback pull exists so webhook outage does not equal blind trust. |
| EC-73 | Member applied in unit A; before release, HRMS moves them to unit B. | Releasing steward becomes B’s steward for the not-yet-opened release task. Unit A is notified. Eligibility recheck runs. | Current unit is live; we do not keep A as releaser by default. |
| EC-74 | Same as EC-73, but A’s releasing task was already in progress. | A’s holder keeps the open task. Later steps use live B. If A declines because the person left, that is a normal decline. | In-progress holder keeps context; live facts still drive what comes next. |
| EC-75 | Role/designation/skills change mid-case so a hard rule fails. | Case stops with `eligibility_changed`. People already involved are told. | Decisions use live eligibility. |
| EC-76 | People operations marks the case Expedite. | Reminder at 1 business day; escalate at 2. Chain duties unchanged. No auto-approve. | Urgency shortens the clock; it does not delete governance. |
| EC-77 | Database restored from yesterday’s backup. | Mobility writes stay paused until HRMS status/fact pull refreshes snapshots. Cases come back from backup; people are not invented. | HRMS remains people source of truth; portal remains case source of truth. |
| EC-78 | Member withdraws in review. | Terminal `withdrawn_by_member`; tasks cancelled; assignees and prior deciders notified; no HRMS assignment call. | Move never started in HRMS. |
| EC-79 | People ops rescinds after a transition task was marked done but before HRMS accept. | Case `rescinded`; open tasks cancelled; people-ops cleanup item lists what was already done; movement is not complete. | Portal tracks cleanup; it does not silently pretend payroll/access never happened. |
| EC-80 | Opening filled while this application is in review. | Stop `position_filled`; member-safe message that the position is no longer available. | Seat was reserved by another transition or the opening closed. |

## What the member can always see

Their own profile, their own openings, the rule explanation for an opening they can see, their own case state, the member-safe outcome, and their own notices. They never need to ask another team which stage a case is in.

## What stays out of the story

Public applicants. A hand-picked slate. A second directory of approvers who are not in the HRMS. SMS. Pay figures. A hidden ranking. Native mobile applications. An automatic backfill opening. Approval by the passage of time.
