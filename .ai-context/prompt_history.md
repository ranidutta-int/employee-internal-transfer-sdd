# Prompt history

Append-only. Do not rewrite earlier entries.

## 2026-11-16 — Initial baseline

**Request:** Create an Internal Job Portal end to end in SDD form. Study the reference repository's folder structure and recreate the agent, BRD, and spec layout with the same kind of rules. Do not carry that other product's name or domain into this application. Do not write application code yet. Focus on the BRD. The product is a Next.js and Node.js monolith. The BRD must stay usable for any employer, not only a technology firm.

**Brief included in the request:** HRMS is the only source of people. Google OAuth after onboarding. Profile shows hierarchy and HRMS facts. Openings are matched on skills, role, probation, and past movement. Frequent movement can hide an opening or warn approvers. The default story is next reporting line, evaluation, current-unit sign-off, then people operations and the destination unit, then the enabling work that employer actually has. Cover absence, heavy decline patterns, executive observation, administration, and notices. Brainstorm edge cases.

**Follow-up:** The SDD participant guide was attached and the work resumed.

**Outcome:** `.ai-context/BRD.md` v0.1, Pending Gate 1. Module BRDs and draft specs. No `src/`.

## 2026-11-16 — Close questions and write the journey

**Request:** Add a markdown file under the BRD that tracks a user's end-to-end journey, including edge cases and the real-world outcome for each. Close every open question and every working assumption with an industry-level decision.

**Outcome:** `.ai-context/brd/end-to-end-journey.md`. Baseline moved to v0.2. Section 7 is closed. No application code.

## 2026-11-17 — Operational gaps into the BRD

**Request:** Put the missing detail into the BRD at the same how-it-works level as the 15-minute sync explanation: pages and who sees them, HRMS automatic sync, data loss, long chain/urgency, non-response, mid-journey close, mid-journey department/role/hierarchy changes, and how hierarchy is known.

**Outcome:** Baseline v0.3. Companions `product-surface.md`, `hrms-sync-and-hierarchy.md`, `resilience-urgency-and-closure.md`. BR-59–BR-68. Journey EC-71–EC-80. No application code.

## 2026-11-17 — Pitch demo narrative

**Request:** Present the whole idea as a start-to-finish demo narrative for a pitch (without naming storyboard techniques).

**Outcome:** `.ai-context/brd/pitch-demo-journey.md` with 12 beats, cast, timeline, alternate endings, and a demo checklist.
