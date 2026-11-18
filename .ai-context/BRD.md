# Internal Job Portal — business requirements baseline

**Version:** 0.3  
**Status:** Pending Gate 1  
**Date:** 2026-11-17  
**Source brief:** `docs/source/ijp-client-brief.md`  
**Journey:** `.ai-context/brd/end-to-end-journey.md`  
**Pitch demo (start to finish):** `.ai-context/brd/pitch-demo-journey.md`  
**Product surface (pages):** `.ai-context/brd/product-surface.md`  
**HRMS sync and hierarchy:** `.ai-context/brd/hrms-sync-and-hierarchy.md`  
**Resilience, urgency, closure:** `.ai-context/brd/resilience-urgency-and-closure.md`  
**Method:** the SDD participant guide (journey, question log, separated decisions, testable rules). The guide's sample feature is not a requirement of this product.

This file is the requirement baseline. Module BRDs and the detailed BRD companions under `.ai-context/brd/` restate rules they own. If any companion and this file disagree, this file wins.

Questions in Section 7 remain closed. v0.3 adds operational detail: screens, sync mechanics, hierarchy, mid-journey fact changes, data loss, urgency, and mid-journey closure. Application code and HTTP path contracts are still not in this version.

---

## 1. Business problem

People who already work for an organization need a governed way to see internal openings they qualify for, apply, and move. The human-resource system of record (HRMS) already knows who they are. The portal must not become a second place where staff type employees in, and it must not assume the employer is a technology delivery organization.

A campus, a laboratory, a hospital, a plant, a professional practice, and a technology firm must be able to run the same duties. Their own titles are labels.

## 2. Confirmed product constraints

These came from the request on 2026-11-16.

| ID | Constraint | Kind |
| --- | --- | --- |
| BD-01 | The HRMS is the only source of workforce identity. A person appears here because the HRMS sent them. | Business, confirmed |
| BD-02 | Sign-in is Google OAuth, and only for a person the portal already holds as an active workforce record. | Business, confirmed |
| BD-03 | Home and profile show hierarchy, skills, role, designation, and the other employment facts the HRMS supplies. | Business, confirmed |
| BD-04 | A person sees openings that match their facts. Two people can see two different sets. | Business, confirmed |
| BD-05 | Skill, role, probation, and past movement behaviour are eligibility inputs. | Business, confirmed |
| BD-06 | Frequent movement has two lawful outcomes, chosen by the organization: hide the opening, or show it and carry a warning to approvers. | Business, confirmed as a capability |
| BD-07 | The default chain is line authority, evaluation, releasing steward, people operations, then receiving steward. A movement type may reorder the last two or run them in parallel. | Business, decided (Q-05) |
| BD-08 | After approval, only the tasks that movement type enables are created, and the HRMS must accept the assignment. That confirmation cannot be waived. | Business, decided (Q-11) |
| BD-09 | Absence uses one delegate hop of at most 30 days, then escalation. Silence never approves. | Business, decided (Q-02, Q-30) |
| BD-10 | A steward is flagged at more than 70% declines across at least 5 decisions in 90 days, or at 5 consecutive declines. The flag does not remove them and does not approve anyone. | Business, decided (Q-03) |
| BD-11 | An executive observer can see the whole journey and cannot approve. Sign-off is a separate duty a movement type adds. | Business, decided (Q-29) |
| BD-12 | Each material step notifies in the portal and by work email. | Business, decided (Q-09) |
| TD-01 | One repository and one release. Next.js for the experience. NestJS (Node.js) for the service. | Technical, decided |
| TD-02 | PostgreSQL is the store. Prisma is the data-access library. | Technical, decided (Q-12) |
| TD-03 | Tailwind CSS is the styling system. | Technical, decided (Q-12) |
| TD-04 | Deployment is Docker, cloud-agnostic. The experience and the service are two processes of one application. | Technical, decided (Q-12) |
| TD-05 | Jest is the test runner for the experience and the service. | Technical, decided (Q-12) |
| TD-06 | The HRMS exchange is HTTPS JSON. The stable key is employee id. Google matches work email. | Technical, decided (Q-10, Q-27) |

## 3. Glossary

| Term | Meaning |
| --- | --- |
| Member | A person in the workforce snapshot. |
| Organizational unit | A grouping the customer names in their own language: department, faculty, plant, practice, function, or another word. |
| Duty | A responsibility in the workflow. Titles are not duties. |
| Movement | A change of unit, role, assignment, or location inside the organization. |
| Movement type | A configured kind of movement. The customer turns types on. Permanent move, lateral move, promotion, and secondment are the types the product must be able to represent. |
| Opening | A published internal opportunity. |
| Application | A member's request to be considered for one opening. |
| Transition | The work after approval that makes the movement real. |
| Hard rule | A rule that hides an opening when the member fails it. |
| Warn rule | A rule that still shows the opening and travels with the application for approvers to acknowledge. |
| Workforce snapshot | The latest workforce facts the portal has accepted from the HRMS. |

### Duties

| Duty | What they do in the portal |
| --- | --- |
| Member | Sees own profile, relevant openings, and own cases. Applies, withdraws, and answers requests. |
| Line authority | The next person in the member's reporting line. Supports or declines before evaluation. |
| Evaluator | Judges the application. May be one person or a panel. |
| Releasing steward | Accountable for the member's current unit. Releases, proposes a later effective date, or declines. |
| Receiving steward | Accountable for the destination unit. Approves or declines the destination. |
| People operations | The employer's people function. Approves employment policy and can reopen, freeze, or rescind within the rules. |
| Enabling-function owner | Completes one configured transition task (payroll effect, access, workspace, credential check, or another task the customer enabled). |
| Executive observer | Reads journeys, stalls, and patterns across the organization. |
| Administrator | Configures labels, movement types, rules, calendars, and reason codes. |
| Delegate | Covers a named duty for a time range when the primary is unavailable. |

Illustrative titles, so the same duty can be labeled per employer:

| Duty | Technology employer | Campus | Laboratory or plant |
| --- | --- | --- | --- |
| Line authority | Reporting manager | Supervisor | Line manager |
| Releasing steward | Current unit lead | Current head of department | Current area head |
| Receiving steward | Destination unit lead | Receiving head of department | Receiving area head |
| People operations | Human resources | Human resources or faculty affairs | Human resources |
| Extra optional gate | None, unless configured | Dean, for a cross-faculty move | Quality or compliance, for a regulated role |

The extra gate exists only when that movement type includes it.

## 4. Actors and the nine journey questions

| Question | Answer in this baseline |
| --- | --- |
| Who is the user? | Members, and the duties in the glossary. Every actor is a person in the workforce snapshot. |
| What triggers the journey? | The HRMS sends a workforce record; a publisher opens an opportunity; a member applies; an approver decides; a transition owner completes a task. |
| What does the user do? | Sign in, read profile, apply, decide, delegate, configure, or complete a transition task. |
| What does the system do? | Match rules, route work, record reasons, notify, and wait for the HRMS to accept the assignment. |
| What happens next? | The next duty in the movement type, or transition, or a stop. |
| What are the possible outcomes? | Ineligible and hidden; eligible and shown; applied; declined; withdrawn; approved; in transition; complete; rescinded; stopped because employment or eligibility changed. |
| What happens when something goes wrong? | Section 8. The case stays truthful: no half-applied assignment, no approval by timeout, no silent skip of a missing person. |
| Which information is required? | Workforce facts from the HRMS; opening rules; a reason code on every decline; an acknowledgement of every warning before support or approval. |
| Which business rules apply? | Section 6. Every rule is decided. The narrative of failures is `.ai-context/brd/end-to-end-journey.md`. |

## 5. Business journeys

### 5.1 Journey A — a person becomes able to sign in

| Step | User does | System does | Outcome / failure |
| --- | --- | --- | --- |
| A1 | Nothing. The HRMS sends a join, change, or leave. | Accepts the snapshot fields named in BR-56. | Unknown person is not created by a human. A failed sync is visible to administrators. |
| A2 | Chooses Google sign-in. | Matches the Google identity to the work identity on an active snapshot row. | Match and active → session. No row, inactive, ended, or a different personal identity → refusal. No account-linking screen. |
| A3 | Opens profile. | Shows mastered facts and the reporting line the member is allowed to see. | Mastered facts have no edit control. A correction happens in the HRMS and arrives on a later sync. |

### 5.2 Journey B — relevant openings

| Step | User does | System does | Outcome / failure |
| --- | --- | --- | --- |
| B1 | Publisher creates an opening and its rules. | Saves draft, then open, when the publisher publishes. | A movement type with no receiving steward cannot be published. |
| B2 | Member opens the openings list. | Evaluates hard rules and warn rules against the snapshot. | Any hard failure → opening absent. Warn only → opening present, warning attached. |
| B3 | Member asks why an opening is listed. | Lists the rules that passed and the warnings. | No score, rank, or unexplained recommendation. |
| B4 | Publisher asks who matches. | Shows counts by excluding rule. | A named list opens for people operations and the receiving steward of that opening. The view is audited. |

### 5.3 Journey C — application and decision

| Step | User does | System does | Outcome / failure |
| --- | --- | --- | --- |
| C1 | Member applies. | Stores the rule result and the snapshot time. Checks the concurrent-application limit. | Hidden or ineligible opening cannot be submitted. Over the limit → refusal and the reason. |
| C2 | Line authority supports or declines. | On support, opens evaluation. On decline, stops the chain. | Missing line authority → delegation or escalation (Journey E). The applicant never receives this task for their own case. |
| C3 | Evaluator passes or declines. | A panel passes by majority. A tie declines. A named veto duty can still stop the case. | Decline stops the chain with a reason code. |
| C4 | Releasing steward releases, asks for a later date, or declines. | A later date becomes the proposed effective date. | Decline stops the chain. People operations may reopen with a reason (A-34). |
| C5 | People operations and the receiving steward approve. | Default order is people operations, then receiving steward. A movement type may reorder those two or run them in parallel. | Either decline stops the chain. |
| C6 | Member withdraws before transition. | Stops open tasks and informs people who already acted or still hold a task. | Withdrawal after transition has started is a rescind (Journey D), not a quiet delete. |

### 5.4 Journey D — transition

| Step | User does | System does | Outcome / failure |
| --- | --- | --- | --- |
| D1 | Nobody. The chain has completed. | Builds the checklist from the movement type. | The HRMS assignment confirmation is always on the list. Other tasks exist only when that movement type enables them. |
| D2 | Each owner marks a task done, or waives it with a reason. | Independent tasks can run together. Assignment confirmation waits for configured predecessors, or runs last when none are set. | A failed task stays open, informs the owner and people operations, and does not cancel unrelated tasks. |
| D3 | People operations rescinds before the effective date, while the movement is incomplete. | Cancels open tasks and informs the chain. | After completion, a reversal is a new movement. |
| D4 | HRMS accepts the assignment and required tasks are done or waived. | Marks the movement complete and shows the member the completed state. | HRMS rejection leaves the movement in transition. The portal does not invent a second personnel record. |

### 5.5 Journey E — absence, stall, and oversight

| Step | User does | System does | Outcome / failure |
| --- | --- | --- | --- |
| E1 | Primary is on leave, ended, or the post is vacant. | Routes new and open tasks to the active delegate. Audit stores both names. | No delegate → task waits, then escalates to the configured skip-level, then to people operations. |
| E2 | Executive observer opens the pipeline. | Shows journeys, time in stage, stalls, and decline patterns. | This duty has no approve action. |
| E3 | Administrator configures a label, a rule, a freeze, or a recorded unblock. | Freeze pauses affected cases in place. Unblock stores the reason and the duty being acted for. | Configuration does not itself decide a case. |

## 6. Business rules

Every rule below is decided for this baseline. A movement type may loosen a rule only where the rule says so.

| ID | Rule | Status |
| --- | --- | --- |
| BR-01 | A workforce row is created, updated, or ended only by an accepted HRMS sync. | Decided |
| BR-02 | There is no screen, import, or invite that adds a person who is absent from the snapshot. | Decided |
| BR-03 | Google sign-in succeeds only when the trimmed, case-insensitive email matches the work email of an active row. The employee id is the permanent key. A changed work email keeps the same row. The member cannot bind another mailbox. | Decided (Q-27) |
| BR-04 | After an accepted snapshot says employment has ended, that person cannot submit, decide, or complete tasks. | Decided |
| BR-05 | HRMS-mastered fields are visible and not editable. Corrections are made in the HRMS. | Decided |
| BR-06 | The freshness limit is 24 hours from the last accepted snapshot. Past that age, new applications and new decisions pause. Reading stays available, with the age visible. Ended employment, suspension, and do-not-move apply as soon as they are accepted. | Decided (Q-01) |
| BR-07 | Workflow steps are duties. The customer's title is a label and does not add a step. | Decided |
| BR-08 | A movement type lists the duties that take part and their order. A duty omitted from the type does not appear and does not block the case. | Decided |
| BR-09 | Reporting line and unit membership come from the HRMS. | Decided |
| BR-10 | While a primary is unavailable, open tasks for that duty go to one active delegate. A delegation lasts at most 30 days and may be renewed. There is no second hop. The applicant cannot be the delegate, and a person cannot delegate to themselves. The audit stores both names. The service clock does not reset. | Decided (Q-30) |
| BR-11 | Reminder at 2 business days. Escalation at 5 business days to the skip-level, then 3 business days to people operations, then an executive exception after 5 further business days. Elapsed time never approves. A missing line authority with no delegate goes to people operations immediately and is recorded as no incumbent. | Decided (Q-02) |
| BR-12 | Consecutive duties that resolve to the same person are one task with one outcome recorded per duty. A decline on any of those duties stops the case. Duties separated by another duty stay as separate tasks, and the screen says why both exist. | Decided (Q-32) |
| BR-13 | The primary reporting line supports the move. Other lines are notified and have no task, unless the movement type requires every line to support it. | Decided (Q-07) |
| BR-14 | A member sees an opening only when every hard rule passes. Warn rules stay visible and must be acknowledged by each approver before support or approval. | Decided |
| BR-15 | Movement frequency can be hard or warn. The default on a movement type is warn when the member has changed unit 2 or more times in the previous 12 months. An opening may set hide, a different count, or off. | Decided |
| BR-16 | On a default movement type, current probation is a hard rule. A movement type may set it to warn or off. | Decided (Q-26) |
| BR-17 | The explanation of an opening lists passed rules and warnings. The portal does not rank members and does not keep a hidden score. | Decided |
| BR-18 | Publishers see match counts by excluding rule. The named match list is limited to people operations and the receiving steward of that opening. Each opening of the list is audited. Executive observers see in-flight cases and aggregates. Administrators see counts. | Decided (Q-19) |
| BR-19 | A limited opening uses tighter rules and is absent from ordinary discovery. The portal does not take a hand-picked list of people. | Decided (Q-15) |
| BR-20 | Eligibility is evaluated again before each decision, including after a publisher changes rules. A new hard failure stops the case and tells the people already involved. New warnings must be acknowledged again. | Decided |
| BR-21 | The default chain is line authority, evaluation, releasing steward, people operations, receiving steward. A movement type may reorder the last two duties or run them in parallel. | Decided (Q-05) |
| BR-22 | Any decline stops the chain and requires a reason code. The member sees the stage, the outcome, and the member-safe label. Internal notes stay with the deciding duties, people operations, executive observers, and administrators. The catalog is: skills gap, timing, retain in current unit, another candidate selected, policy, credentials, position withdrawn, and other. Other requires a member-safe sentence. | Decided (Q-20) |
| BR-23 | Evaluation starts only after line-authority support. A panel passes by majority. A tie declines. A vacant seat escalates and is not counted as a pass. A veto duty named on the movement type can stop the case despite a majority. | Decided (Q-06) |
| BR-24 | A member may withdraw before transition starts. After transition starts, the stop is a people-operations rescind. | Decided |
| BR-25 | A member has at most one application in the decision chain or in transition. | Decided (Q-04) |
| BR-26 | An opening has a seat count, default 1. A seat is reserved when transition starts, in the order the transition record is stored. In-review applications stop once reserved seats equal the count, or when the opening is filled, withdrawn, or expired. A transition that has already started continues if the opening is later withdrawn. A paused opening keeps applications, does not advance them, and pauses their service clock. | Decided |
| BR-27 | An in-progress task stays with the person who holds it when the primary line changes. The new line authority is informed and can read the case. Tasks not yet opened follow the new line. This is not configurable. | Decided (Q-22) |
| BR-28 | Ended employment, suspension, and a do-not-move flag stop every open case. None of the three can be turned off by a movement type. | Decided (Q-16) |
| BR-29 | Retain is a decline with that reason code. People operations may reopen within 10 business days, with a reason. The case returns to the duty that declined. The decline stays in the history. After 10 business days the member applies again. | Decided (Q-34) |
| BR-30 | An administrator may freeze a unit or a movement type. Affected cases pause in place. New applications into a frozen unit are refused. The service clock pauses. An administrative reassignment of a stuck task goes only to an active holder of that duty or to people operations, with a reason. It cannot mark HRMS confirmation done. | Decided (Q-21) |
| BR-31 | Transition starts only after the chain completes. Every movement type includes HRMS assignment confirmation. Other tasks exist only if the type enables them. | Decided |
| BR-32 | A task is done or waived. A waiver stores who waived it and why. HRMS assignment confirmation cannot be waived. The movement is complete only when every required task is done or lawfully waived and the HRMS has accepted the assignment. | Decided |
| BR-33 | Before the effective date, people operations may rescind an incomplete movement. After completion, reversal is a new movement. The effective date is today or later, and at most 90 days ahead. The releasing steward proposes it. People operations confirms it. Snapping to the next pay-period start is off unless the organization turns it on. | Decided |
| BR-34 | A failed transition task remains open and does not cancel tasks that do not depend on it. The owner, people operations, the member, and the executive observer are told. | Decided |
| BR-35 | Material steps notify Section 9 in the portal and at the work email. A delegate receives the covered duty's notices. Executive observers receive immediate exceptions and a daily summary, not each routine approval. Email retries 3 times over 24 hours. A failed delivery does not undo the decision. No SMS and no personal mailbox. | Decided (Q-09) |
| BR-36 | Executive observation does not approve a case. Executive sign-off is a separate duty a movement type may add. If the only people-operations incumbent is the applicant, that sign-off step is inserted for that case. | Decided (Q-29) |
| BR-37 | People operations and executive observers see approvals, declines, and ageing. An attention item is raised when declines exceed 70% of at least 5 decisions in a rolling 90 days, or when there are 5 consecutive declines. The item does not remove the steward and does not approve the member. An administrator may change the numbers or turn the item off. Counts stay visible when the item is off. | Decided (Q-03) |
| BR-38 | Submissions, decisions, acknowledgements, delegations, escalations, waivers, rescinds, interventions, and named-list views stay on the case for 7 years after the case closes. | Decided (Q-23) |
| BR-39 | A member sees their own cases, profile, and notices. A member does not open another member's profile, application, or notices. | Decided |
| BR-40 | Mobility preferences and an application statement may be stored in the portal. Preferences can only narrow what policy already allows. | Decided |
| BR-41 | Service time uses Monday to Friday in the organization's headquarters timezone, excluding the organization's holiday list. | Decided (Q-23) |
| BR-42 | Compensation amounts are not collected, stored, or shown. A movement type may include a task whose only outcomes are done or not done: confirm compensation in the HRMS. | Decided (Q-13) |
| BR-43 | Secondment is available. It requires an expected end. People operations is reminded 30 days before that date. The return is a new movement and does not run by itself. | Decided (Q-17) |
| BR-44 | Completing a movement does not create an opening. The releasing steward receives one notice that the origin unit may have a vacancy. The notice is on unless the organization turns it off. | Decided (Q-33) |
| BR-45 | On a default movement type, fewer than 6 months in the current assignment is a hard rule. A type or an opening may set another number, including zero. | Decided |
| BR-46 | Default movement types allow the worker type Employee only. A type may also allow contingent, intern, or joint appointment. | Decided |
| BR-47 | After a decline, the member waits 90 days before applying again to that same opening. People operations may waive the wait with a reason. A different opening is allowed once no application is active. | Decided (Q-08) |
| BR-48 | If the snapshot has no skill data, a skill rule does not pass and does not read as "lacks the skill." Publishers see the excluding rule "skill data unavailable." | Decided |
| BR-49 | The experience meets WCAG 2.2 AA. One language ships, English unless the deployment chooses another. User-facing strings are externalized. | Decided (Q-24) |
| BR-50 | Every actor is a person in the workforce snapshot. The product has no directory of approvers the HRMS does not know. | Decided (Q-14) |
| BR-51 | Status events target 15 minutes from the HRMS event. Other workforce facts arrive within 24 hours. | Decided (Q-10) |
| BR-52 | The HRMS snapshot carries employee id, work email, name, status, worker type, unit, primary line, other lines, role, designation, skills, probation and its end, location, movement history, and employment start date. | Decided (Q-10) |
| BR-53 | The assignment request carries employee id, destination unit, role and designation when they change, effective date, movement type, and case id. It carries no pay. The movement stays in transition until the HRMS accepts. A rejection shows the HRMS reason. No answer within 1 business day opens a people-operations task. | Decided (Q-11) |
| BR-54 | List, detail, and decision writes return within 2 seconds for 95% of calls, excluding time spent in the HRMS and in email delivery. | Decided |
| BR-55 | The store is PostgreSQL. The data-access library is Prisma. The service is NestJS. Styling is Tailwind CSS. Tests use Jest. Deployment is Docker and is not tied to one cloud. | Decided (Q-12) |
| BR-56 | The HRMS exchange is HTTPS JSON, both for the workforce feed and for the assignment request. | Decided (Q-10, Q-11) |
| BR-57 | A publisher who is not the receiving steward of the destination, people operations, or an administrator cannot publish an opening. | Decided |
| BR-58 | When two cases would reserve the last seat, the first stored transition reserves it. The other stops as position filled. | Decided |
| BR-59 | The experience exposes only the pages and duty matrix in `.ai-context/brd/product-surface.md`. Deep links obey the same matrix. Hiding a control is not authorization. | Decided |
| BR-60 | Hierarchy is derived only from the accepted HRMS snapshot (unit, primary line, other lines). The portal does not invent reporting lines. Profile shows upward primary line and direct reports only. | Decided |
| BR-61 | HRMS integration uses webhook as primary and scheduled pull as fallback (status every 5 minutes, facts every 1 hour). Payloads are HTTPS JSON, authenticated, and idempotent on employee id + HRMS event id. | Decided |
| BR-62 | Status events (ended, suspended, do-not-move) are applied within a 15-minute target of the HRMS event. On accept: snapshot updates, open cases stop, session cannot act, sensitive writes re-check live status. | Decided |
| BR-63 | On apply, the case stores opening, movement type, eligibility result, warnings, statement, and snapshot time for audit. Unit, lines, role, designation, skills, probation, and status are always read live from the latest snapshot for routing and rechecks. | Decided |
| BR-64 | Before every decision the service reloads the snapshot, rechecks hard/warn rules, and re-resolves duties. Hard failure stops the case with member-safe reason `eligibility_changed` (or employment status reason when applicable). | Decided |
| BR-65 | If the member's unit changes before the releasing-steward task opens, releasing steward resolves to the new current unit; the prior unit is notified. An in-progress releasing task keeps its holder; later steps use live facts. | Decided |
| BR-66 | Database case data: backup with RPO ≤ 24 hours and RTO ≤ 8 hours. After restore, HRMS status and fact pulls refresh snapshots before mobility writes reopen. Workforce rows are never invented on restore. | Decided |
| BR-67 | Expedite may be set on an opening or case by people operations or the receiving steward, with a reason. It shortens reminder/escalation to 1 and 2 business days. It does not skip duties or auto-approve. | Decided |
| BR-68 | Mid-journey terminal and pause outcomes use the stop catalog and cleanup rules in `.ai-context/brd/resilience-urgency-and-closure.md`. If transition had started, people operations receives a cleanup checklist item; the portal does not mark the move complete and does not itself roll back external payroll or access systems. | Decided |

## 7. Question log

Every question is closed. The decision column is the rule.

| ID | Question | Decision | Rule |
| --- | --- | --- | --- |
| Q-01 | How old may a snapshot be before new applications and decisions pause? | 24 hours. Status events apply on receipt. | BR-06, BR-51 |
| Q-02 | How long before escalation? | Reminder at 2 business days. Escalate at 5. Then 3, then an executive exception at 5. Never auto-approve. A vacant line authority escalates immediately. | BR-11 |
| Q-03 | What decline pattern raises an attention item? | Over 70% of at least 5 decisions in 90 days, or 5 consecutive declines. | BR-37 |
| Q-04 | How many applications may be active? | One, through the decision chain and transition. | BR-25 |
| Q-05 | People operations and the receiving steward: order? | People operations, then receiving steward. A type may reorder or run them in parallel. | BR-21 |
| Q-06 | Must a panel be unanimous? | No. Majority. A tie declines. A named veto duty may still stop the case. | BR-23 |
| Q-07 | Must every reporting line approve? | No. The primary line approves. Others are notified unless the type requires them. | BR-13 |
| Q-08 | Cooling-off after a decline? | 90 days for the same opening. Waiver by people operations with a reason. | BR-47 |
| Q-09 | Which notice channels? | In the portal and the work email. No SMS. No personal mailbox. | BR-35 |
| Q-10 | What does the HRMS send, and how fast? | The fields in BR-52. Employee id is the key. Status within a 15-minute target. Other facts within 24 hours. HTTPS JSON. | BR-51, BR-52, BR-56 |
| Q-11 | How is the new assignment requested? | The fields in BR-53. Complete only after HRMS acceptance. Silence for 1 business day opens a people-operations task. | BR-53 |
| Q-12 | Database, data access, service framework, styling, deployment, tests? | PostgreSQL, Prisma, NestJS, Tailwind CSS, Docker, Jest. | BR-55, TD-02–TD-05 |
| Q-13 | Is compensation visible? | No amounts. A task may confirm that the HRMS was updated. | BR-42 |
| Q-14 | Approvers who are not in the HRMS? | No. | BR-50 |
| Q-15 | May a publisher name people on an opening? | No. | BR-19 |
| Q-16 | Is suspension a hard stop? | Yes. So is do-not-move. Neither can be configured off. | BR-28 |
| Q-17 | Secondments? | Yes. End date required. Reminder 30 days before. Return is a new movement. | BR-43 |
| Q-19 | Who may open the named match list? | People operations and the receiving steward of that opening. Audited. | BR-18 |
| Q-20 | What does the member see on a decline? | Stage, outcome, member-safe reason. Not internal notes. | BR-22 |
| Q-21 | What does a freeze do to in-flight cases? | Pause in place. Refuse new applications into the unit. Pause the clock. | BR-30 |
| Q-22 | Who keeps a task when the line changes? | The current holder. The new line is informed. Later tasks follow the new line. | BR-27 |
| Q-23 | Calendar and audit life? | Monday–Friday, headquarters timezone, organization holidays. Audit kept 7 years after closure. | BR-38, BR-41 |
| Q-24 | Accessibility and language? | WCAG 2.2 AA. One language, English by default, strings externalized. | BR-49 |
| Q-26 | Is probation a hard hide? | Yes on default types. A type may loosen it. | BR-16 |
| Q-27 | What matches Google? | Work email, trimmed, case-insensitive. Employee id keeps the row if the email changes. | BR-03 |
| Q-29 | Does the executive observer approve? | No. A separate sign-off duty does, and only when the type adds it, or when BR-36 inserts it. | BR-36 |
| Q-30 | May a delegate delegate again? | No. One hop. 30 days. Renewable. | BR-10 |
| Q-32 | Two duties, one person, in a row? | One task, one outcome per duty. | BR-12 |
| Q-33 | Backfill opening? | Not created. One notice to the releasing steward, on by default. | BR-44 |
| Q-34 | Who may reopen a decline? | People operations, within 10 business days, with a reason. | BR-29 |

There is no open assumption list. Earlier working assumptions are replaced by the decisions above.

## 8. Failure scenarios

| ID | Failure | Expected behaviour | Rule |
| --- | --- | --- | --- |
| FS-01 | HRMS sync fails. | Administrators see the failure and the age of the last accepted snapshot. New operational actions follow BR-06. | BR-01, BR-06 |
| FS-02 | Google identity matches no active row. | Sign-in is refused. No person is created. | BR-02, BR-03 |
| FS-03 | Employment ends while a case is open. | Open cases stop. The person can no longer act. | BR-04, BR-28 |
| FS-04 | Required approver is absent and has no delegate. | The task waits and then escalates. It is not approved and not skipped. | BR-11 |
| FS-05 | The applicant is the person who would approve. | That task goes to the delegate or escalates. | BR-10 |
| FS-06 | The same person is releasing steward and receiving steward. | Combined task records both duties. People operations remains a separate approver on the default chain. | BR-12, BR-21 |
| FS-07 | Member submits twice, or double-clicks apply. | One application for that opening. The second attempt reports the existing application. | BR-25 |
| FS-08 | Opening is filled while other applications are in review. | Those applications stop. Members are informed. No transition starts for them. | BR-26 |
| FS-09 | A hard rule starts failing after submission. | The case cannot advance. People already involved are told. | BR-20 |
| FS-10 | An approver records support while a warning is unacknowledged. | The decision is refused until the warning is acknowledged. | BR-14 |
| FS-11 | A transition task fails. | The movement stays in transition. The owner and people operations are informed. Unrelated tasks continue. | BR-34 |
| FS-12 | The HRMS rejects the assignment. | The movement stays in transition. It is not marked complete. | BR-31, BR-32 |
| FS-13 | A notice fails to deliver. | The failure is visible and retried. The decision stands. | BR-35 |
| FS-14 | Snapshot is older than the freshness limit. | New applications and new decisions pause. | BR-06 |
| FS-15 | Administrator tries to decide a case with no recorded intervention. | The product offers configuration and a recorded intervention, not a silent approve. | BR-30, BR-36 |

## 9. Notification matrix

| Event | Member | Current assignee | Delegate | People operations | Executive observer | Enabling owner |
| --- | --- | --- | --- | --- | --- | --- |
| Application submitted | Yes | Line authority | If covering | No | No | No |
| Decision recorded | Yes | Next assignee | If covering | On decline | No | No |
| Warning on the case | Yes, that approvers will see it | Yes | If covering | No | No | No |
| Escalation or stall past service time | Yes | Previous assignee | If covering | Yes | Exception | No |
| Withdrawal, opening closed, eligibility lost | Yes | Open assignees | If covering | Yes | No | No |
| Transition task assigned or failed | On failure and on completion | — | If covering | On failure | Exception on failure | The owner of that task |
| Movement complete or rescinded | Yes | — | — | Yes | Summary | Open owners on rescind |
| Rejection-pattern attention | No | No | No | Yes | Yes | No |
| Freeze | Affected members and assignees | Yes | If covering | Yes | Exception | No |

## 10. Functional requirements

| ID | The portal must | Rules |
| --- | --- | --- |
| FR-01 | Accept workforce create, update, and end from the HRMS. | BR-01 |
| FR-02 | Provide no way to add a person who is not in the snapshot. | BR-02 |
| FR-03 | Sign a person in with Google only on a work-identity match to an active row. | BR-03 |
| FR-04 | Remove the ability to act after accepted end of employment. | BR-04 |
| FR-05 | Show mastered fields without an edit path. | BR-05 |
| FR-06 | Show administrators sync health and snapshot age. | BR-01, FS-01 |
| FR-07 | Pause new applications and new decisions when the snapshot is past the freshness limit. | BR-06 |
| FR-08 | Store units with a customer-supplied label. | BR-07 |
| FR-09 | Execute the chain from duties on the movement type. | BR-07, BR-08 |
| FR-10 | Read reporting lines and unit membership from the snapshot. | BR-09 |
| FR-11 | Route a duty's open tasks to an active delegate, and refuse an applicant as their own delegate. | BR-10 |
| FR-12 | Escalate when no delegate exists, without approving by timeout. | BR-11 |
| FR-13 | Present consecutive same-person duties as one task with an outcome per duty. | BR-12 |
| FR-14 | Require the primary reporting line to support the move, and notify other lines unless the movement type requires them to support it. | BR-13 |
| FR-15 | Hide an opening on any hard-rule failure, and show warn-rule openings with the warning attached. | BR-14, BR-15, BR-16 |
| FR-16 | Explain a visible opening by rules and warnings, without a hidden score. | BR-17 |
| FR-17 | Show match counts by excluding rule, and audit named-list views. | BR-18 |
| FR-18 | Keep limited openings out of ordinary discovery and reject a hand-picked slate. | BR-19 |
| FR-19 | Re-check eligibility before each decision and stop on a new hard failure. | BR-20 |
| FR-20 | Accept an application only for a visible, eligible opening, and store the rule result and snapshot time. One application per member per opening. | BR-14, FS-07 |
| FR-21 | Run the default chain, or the chain the movement type defines. | BR-21, BR-23 |
| FR-22 | Stop on decline, require a reason code, and show the member-safe reason. | BR-22 |
| FR-23 | Accept withdrawal before transition and inform the people involved. | BR-24 |
| FR-24 | Enforce the concurrent-application limit. | BR-25 |
| FR-25 | Stop or pause applications when the opening changes state, as BR-26 states. | BR-26 |
| FR-26 | Refuse support or approval until warnings on the case are acknowledged. | BR-14 |
| FR-27 | Apply the mid-case reporting-line rule in BR-27. | BR-27 |
| FR-28 | Stop open cases on ended employment or suspension under BR-28. | BR-28 |
| FR-29 | Record a retain decision as a decline, and allow a reasoned reopen by people operations. | BR-29 |
| FR-30 | Freeze and pause as BR-30 states, and record any administrative unblock. | BR-30 |
| FR-31 | Start transition only after the chain is complete, with HRMS confirmation always on the checklist. | BR-31 |
| FR-32 | Complete a movement only when required tasks are done or waived and the HRMS has accepted the assignment. | BR-32 |
| FR-33 | Rescind an incomplete movement before the effective date, and treat a later reversal as a new movement. | BR-33 |
| FR-34 | Keep a failed transition task open without cancelling independent tasks. | BR-34 |
| FR-35 | Notify according to Section 9, including delegates, and retry a failed delivery without undoing the decision. | BR-35 |
| FR-36 | Give executive observers the pipeline, time in stage, stalls, and decline patterns, with no approve action on that duty. | BR-36 |
| FR-37 | Let administrators configure labels, movement types, rules, freshness, delegation policy, calendars, and reason codes. | BR-07, BR-08 |
| FR-38 | Show steward volumes and raise a threshold attention item that does not auto-decide. | BR-37 |
| FR-39 | Keep the audit events named in BR-38 on the case. | BR-38 |
| FR-40 | Show a member their own hierarchy slice, facts, and cases, and refuse another member's profile. | BR-39, BD-03 |
| FR-41 | Store mobility preferences that only narrow policy, plus an application statement. | BR-40 |
| FR-42 | Count service time on the organization's business calendar. | BR-41 |
| FR-43 | Expose only the pages and duty access in the product-surface matrix. | BR-59 |
| FR-44 | Derive hierarchy and duty holders from the HRMS snapshot fields only. | BR-60 |
| FR-45 | Ingest HRMS via webhook plus fallback pull; enforce idempotency and sync health visibility. | BR-61 |
| FR-46 | Apply status events on the fast path and refuse sensitive writes when status is not allowed. | BR-62 |
| FR-47 | Keep apply-time eligibility audit; use live snapshot for routing and rechecks; stop on new hard failure. | BR-63, BR-64 |
| FR-48 | Re-resolve releasing steward to the live unit when that task has not opened; notify the prior unit. | BR-65 |
| FR-49 | Meet backup RPO/RTO; after restore, refresh from HRMS before mobility writes. | BR-66 |
| FR-50 | Support Expedite with shortened SLA clocks without skipping duties. | BR-67 |
| FR-51 | Apply mid-journey stop catalog, notices, and people-operations cleanup when transition had started. | BR-68 |

## 11. Non-functional requirements

| ID | Requirement | Status |
| --- | --- | --- |
| NFR-01 | Workforce facts, application content, internal notes, and Google tokens are not written to general logs. | Required. Confirmed as a security posture. |
| NFR-02 | A member can see the current state of their own case without asking another team. | Required (FR-40). |
| NFR-03 | The experience and the service ship as one application (TD-01). | Confirmed. |
| NFR-04 | List, detail, and decision writes return within 2 seconds for 95% of calls, excluding HRMS and email time. | Decided (BR-54) |
| NFR-05 | Status-event accept target: 15 minutes from HRMS event. Fact-update accept target: 24 hours. Status poll at least every 5 minutes when webhooks are quiet. | Decided (BR-51, BR-61, BR-62) |
| NFR-06 | Case data backup RPO ≤ 24 hours; restore RTO ≤ 8 hours. | Decided (BR-66) |

## 12. Security considerations

| Risk | Control | Related |
| --- | --- | --- |
| A person who is not in the workforce gains a session. | Google match to an active work identity. No self-registration. | BR-02, BR-03, FS-02 |
| A leaver keeps approving or applying. | Accepted end-of-employment removes the ability to act. Freshness pause covers a stale snapshot. | BR-04, BR-06 |
| An applicant approves their own move. | Recuse, delegate, or escalate. | BR-10, FS-05 |
| Warnings are ignored. | Acknowledgement is required before support or approval. | BR-14, FS-10 |
| Named match lists become a browsing tool. | Duty limit plus an audit of each view. | BR-18 |
| Internal evaluation notes leak to the member or to logs. | Member-safe reason is separate. Logs exclude case content. | BR-22, NFR-01 |
| A silent administrative approve. | Intervention is recorded or it does not exist. | BR-30 |
| Delegation hides the real decision. | Both names stored. One hop, 30 days. Delegate cannot be the applicant. | BR-10 |

## 13. Scope

**In scope for the baseline:** workforce sync and Google entry; page/duty surface; hierarchy from HRMS; mid-journey fact changes; configurable units and duties; profile and duty-specific home; rule-based openings; the decision chain; delegation, escalation, and expedite; transition checklists; mid-journey closure and cleanup; notices; data-loss recovery posture; executive observation; administration of the policy above.

**Out of scope:**

- Public careers sites and people who are not in the HRMS.
- Hand-picked candidate slates and manual employee masters.
- Payroll calculation, granting system access, learning content, and performance reviews. Those systems may appear later as enabling-function tasks. This portal tracks completion; it does not replace them.
- A second directory for approvers the HRMS does not know (BR-50).
- SMS, push, and personal-mailbox notices (BR-35).
- Native mobile applications.
- Compensation amounts (BR-42).
- Opaque ranking or an automated decision that overrides a human duty.
- HTTP contracts and application code. Technology choices are decided in BR-55. The contract text is a later spec.

## 14. Dependencies

| Dependency | If it fails |
| --- | --- |
| HRMS workforce feed | FS-01, BR-06 |
| HRMS assignment acceptance | FS-12. The movement stays in transition. |
| Google identity provider | Sign-in is unavailable. Existing cases do not change. |
| Organization configuration (freshness, calendar, movement types) | Administrators see the gap. Operational actions that need the missing setting wait. |

## 15. Traceability

Acceptance criteria live in the module specs. API path contracts and tasks are still empty; behaviour for sync, pages, urgency, and closure is decided in the BRD companions. Journey: `.ai-context/brd/end-to-end-journey.md`.

| FR | Spec acceptance | API | Task | Test execution |
| --- | --- | --- | --- | --- |
| FR-01–FR-07, FR-44–FR-47 | `.ai-context/specs/identity/workforce-access.spec.md` + sync companion | Not designed | Not designed | Not run |
| FR-08–FR-14, FR-48 | `.ai-context/specs/organization/org-model.spec.md` | Not designed | Not designed | Not run |
| FR-40, FR-41, FR-43 | `.ai-context/specs/experience/member-home.spec.md` + product-surface | Not designed | Not designed | Not run |
| FR-15–FR-19 | `.ai-context/specs/opportunities/openings-and-matching.spec.md` | Not designed | Not designed | Not run |
| FR-20–FR-30, FR-50, FR-51 | `.ai-context/specs/mobility/application-decision-chain.spec.md` + closure companion | Not designed | Not designed | Not run |
| FR-31–FR-34, FR-51 | `.ai-context/specs/transition/movement-execution.spec.md` | Not designed | Not designed | Not run |
| FR-35 | `.ai-context/specs/notifications/journey-notifications.spec.md` | Not designed | Not designed | Not run |
| FR-36–FR-39, FR-42, FR-49 | `.ai-context/specs/governance/observation-and-control.spec.md` | Not designed | Not designed | Not run |

The full matrix, including the empty-cell reason, is `.ai-context/traceability.md`.

## 16. Gate 1 posture

This baseline is **not** a Gate 1 submission. Section 7 is closed. Journey, product surface, HRMS sync, hierarchy, urgency, data loss, and mid-journey closure are written. A reviewer still cannot see HTTP path contracts, an executed test suite, or a task plan. Those are the next documents when a build is requested.
