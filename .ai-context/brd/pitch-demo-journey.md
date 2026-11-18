# Pitch demo — one move, start to finish

**Baseline:** `.ai-context/BRD.md` version 0.3  
**Status:** Decided  
**Audience:** Pitch and walkthrough  
**Companion detail:** `.ai-context/brd/end-to-end-journey.md` (rules and edge cases)

This is the demo script for the Internal Job Portal. Read it top to bottom. Each beat is one moment you can show or narrate. Titles are the employer’s words; the product runs on duties.

---

## Cast for the demo

| Person | In the org | Duty in the portal |
| --- | --- | --- |
| Alex | Already employed; wants an internal move | Member |
| Sam | Alex’s reporting manager | Line authority |
| Jordan | Assesses fit for the opening | Evaluator |
| Riley | Leads Alex’s current unit | Releasing steward |
| Casey | People operations | People operations |
| Morgan | Leads the destination unit | Receiving steward |
| Quinn | Watches the pipeline | Executive observer |

The HRMS is off-stage but always present: it already knows Alex. The portal never types Alex in by hand.

---

## Arc in one breath

Alex appears from the HRMS → signs in with Google → sees only openings that fit → applies → Sam supports → Jordan passes → Riley releases → Casey clears policy → Morgan accepts → enabling work finishes and the HRMS takes the new assignment → Alex’s case shows **complete**. Quinn can watch the whole path without approving it.

---

## Beat 1 — Before anyone opens the portal

**You say:** “Alex was hired in the HRMS. Nobody creates Alex again here.”

**What happens:** The HRMS sends Alex’s employee id, work email, unit, manager (Sam), role, designation, skills, probation, and reporting lines. The portal stores that snapshot.

**Screen:** Optional admin sync health — last accepted sync is fresh. No “Add employee” button anywhere.

**Demo tip:** Start here so the room understands this is not an ATS for strangers.

---

## Beat 2 — Alex signs in

**Screen:** Sign-in (Google only).

**Alex does:** Chooses Google with the work email.

**System does:** Matches that email to an active workforce row. Personal Gmail that is not on the row is refused.

**Lands on:** Home — relevant openings, my applications, tasks waiting on me.

---

## Beat 3 — Alex checks identity

**Screen:** Profile.

**Alex sees:** Designation, role, skills, unit, probation, movement history. Hierarchy: Sam above; anyone who reports to Alex below. No edit on HRMS fields.

**You say:** “If the manager is wrong, it is fixed in the HRMS and syncs here. We do not keep a second personnel file.”

---

## Beat 4 — Morgan publishes a seat

**Screen:** Manage openings (Morgan or Casey).

**Morgan does:** Publishes an opening — destination unit, movement type, seat count 1, hard and warn rules (skills, probation, time in assignment, frequency).

**System does:** Opening becomes discoverable only to people who pass every hard rule.

**Demo tip:** Show that a colleague with different skills does **not** see this opening. Same portal, different list.

---

## Beat 5 — Alex discovers a fit

**Screen:** Openings → opening detail.

**Alex sees:** This opening (hard rules passed). A warning may show if Alex has moved units often.

**Alex does:** Opens “Why am I seeing this?” — passed rules and warnings, no score, no rank. Applies with an optional statement.

**System does:** Stores eligibility snapshot and warnings. Notices go to Alex and Sam (portal + work email).

**State:** Application **submitted**.

---

## Beat 6 — Sam supports

**Screen:** Sam’s tasks → this case.

**Sam sees:** Alex’s statement, warnings, current facts from HRMS.

**Sam does:** Acknowledges warnings, then supports.

**System does:** Opens evaluation. Riley is not asked yet.

**State:** In review — evaluation.

**If you need drama:** Show that Sam cannot approve if a warning is still unchecked.

---

## Beat 7 — Jordan evaluates

**Screen:** Jordan’s task (or a panel of three).

**Jordan does:** Passes. On a panel, majority passes; a tie would stop the case.

**State:** In review — release.

---

## Beat 8 — Riley releases

**Screen:** Riley’s task.

**Riley does:** Releases Alex from the current unit and proposes an effective date (today or later, within 90 days).

**You say:** “This is the current unit signing off — not the destination hiring yet.”

**State:** In review — people operations.

---

## Beat 9 — Casey clears policy

**Screen:** Casey’s task.

**Casey does:** Checks policy, confirms the effective date, acknowledges warnings, approves.

**You say:** “Policy sits before the destination commits, so the org does not promise a seat it cannot honour.”

**State:** In review — destination.

---

## Beat 10 — Morgan accepts

**Screen:** Morgan’s task.

**Morgan does:** Accepts Alex into the destination unit.

**System does:** Reserves the seat. Builds the transition checklist. Always includes “HRMS accepts the new assignment.” Other tasks only if this employer turned them on.

**State:** **In transition.**

**Demo tip:** If Morgan was also the evaluator earlier, show two separate questions on the timeline — fit vs seat acceptance.

---

## Beat 11 — The move becomes real

**Screen:** Transition tasks (Casey and any enabling owners).

**They do:** Mark checklist items done. Optional tasks may be waived with a reason. HRMS confirmation **cannot** be waived.

**System does:** Sends the assignment request to the HRMS (employee id, destination unit, role/designation if changed, effective date, case id — no pay figures). Waits for accept.

**State:** Still in transition until HRMS accepts and required tasks are done.

---

## Beat 12 — The journey ends

**Screen:** Alex’s application detail; Casey’s view; Quinn’s pipeline.

**System does:** Marks the movement **complete**. Alex sees completed. Casey is notified. Riley gets a vacancy notice (no automatic backfill opening). Quinn sees the finished journey in the pipeline — still no approve button on the observer duty.

**You say:** “Start: HRMS already knew Alex. End: HRMS holds the new assignment, and the portal holds the governed story of how we got there.”

---

## One-slide timeline (for the pitch deck)

| # | Moment | Who | Outcome |
| --- | --- | --- | --- |
| 1 | Workforce sync | HRMS → portal | Alex exists here |
| 2 | Sign-in | Alex | Home |
| 3 | Profile | Alex | Facts + hierarchy |
| 4 | Publish opening | Morgan | Seat is live |
| 5 | Discover + apply | Alex | Submitted |
| 6 | Support | Sam | Evaluation opens |
| 7 | Evaluate | Jordan | Release opens |
| 8 | Release | Riley | Policy opens |
| 9 | Policy | Casey | Destination opens |
| 10 | Accept | Morgan | Transition starts |
| 11 | Checklist + HRMS | Owners + HRMS | Assignment accepted |
| 12 | Complete | Alex / Quinn | Journey ends |

---

## Alternate endings (30-second pitch answers)

Use these only if someone asks. Full tables live in the end-to-end journey companion.

| If this happens… | Demo ending |
| --- | --- |
| Sam is on leave | Delegate gets the task; if none, escalate — time never auto-approves. |
| Sam ignores the task | Reminder, then escalate (faster if Expedite). Still no silent yes. |
| Jordan declines | Case stops with a member-safe reason. Alex sees why, not private notes. |
| Seat taken by someone else | Alex’s in-review case stops: position no longer available. |
| Alex’s unit changes mid-case | Later release uses the new unit’s steward; eligibility is rechecked. |
| Alex leaves the company mid-case | Status sync stops all cases; Alex can no longer act. |
| Quinn wants to force-approve | Observer has no approve action. Sign-off is a separate step only if configured. |

---

## Demo order checklist

1. Sync / “no add employee”  
2. Alex Google sign-in → home  
3. Profile hierarchy  
4. Colleague does **not** see Alex’s opening  
5. Alex sees opening → why listed → apply  
6. Sam task → support  
7. Jordan → pass  
8. Riley → release + date  
9. Casey → approve  
10. Morgan → accept → transition  
11. HRMS confirmation → complete  
12. Quinn pipeline (watch only)

That is the full pitch path: **HRMS in → governed decisions → HRMS assignment out → complete.**
