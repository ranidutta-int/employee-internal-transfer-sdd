# Internal Job Portal — client brief

**Status:** Source note only. This file is not the requirement baseline.  
**Date:** 2026-11-16  
**Authoritative baseline:** `.ai-context/BRD.md` version 0.1

No separate client document was attached with this request. This note records the brief that was ingested.

## Intent

Build an Internal Job Portal for people who already work in the organization. The human-resource system of record (HRMS) is the source of workforce identity. Once a person is onboarded there, they can sign in here with Google OAuth and find their designation, role, skills, reporting line, and related employment facts already present.

The portal is for any kind of organization. A technology company, a college, a hospital, a plant, and a professional practice must all be able to use the same product by configuring labels and duties. The workflow binds to duties, and the employer supplies their own titles.

## Journey the brief described

1. The HRMS sends people into the portal. Staff do not type employees in, and the portal is not an applicant-tracking desk for unknown candidates.
2. An onboarded person signs in with Google OAuth.
3. Home and profile show the person's hierarchy and the employment facts supplied by the HRMS.
4. Openings are matched to the person. Skill, role, probation, and past movement behaviour decide what they see. Two people can see two different sets of openings.
5. When past movement is too frequent, the organization may hide the opening or allow an application that carries a warning to the people who will approve it.
6. An application goes to the next person in the reporting line, then through evaluation. On a pass, the current unit signs the person off, and the people-operations function plus the destination unit approve.
7. After approval, the functions that must make the move real — payroll effect, access, facilities, or whatever that organization actually has — complete their part.
8. If someone in the chain is absent, the case needs a delegation path.
9. If a steward declines unusually often, that pattern must be visible.
10. A chief executive and an administrator can observe the whole journey.
11. Each step notifies the people who need to know.

## Stack constraint from the same request

The product will be a Next.js and Node.js monolith. This brief does not choose a database, an object-relational mapper, a Node HTTP framework, a styling system, or a deployment target. Those stay open until a later architecture decision.

## What this pass asked for

Specification-driven folders and rules, with the business baseline written first. Application code was explicitly deferred.
