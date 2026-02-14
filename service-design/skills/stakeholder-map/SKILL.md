---
name: stakeholder-map
description: Use when mapping all actors in a service ecosystem — who serves whom, dependencies, interest alignment and conflict, power dynamics
---

# Stakeholder Map

**Core principle:** The blueprint shows one actor's path; the stakeholder map shows how all actors connect. Staff experience drives customer experience — the service-profit chain is not optional.

## Identify All Actors

Scan code, org charts, interviews, business docs. Code reveals roles/permissions. Org charts reveal reporting. Interviews reveal informal structure.

| Type | What to look for |
|------|-----------------|
| **Customer** | User roles, signup flows, segments |
| **Frontline** | Customer-facing staff, support agents, account managers |
| **Backstage** | Operations, fulfillment, moderation, internal tooling users |
| **Support/Ops** | Engineering, finance, legal, HR — enabling systems |
| **External** | Partners, vendors, payment processors, regulators |

## Map Relationships

For every actor pair, capture four dimensions:

| Dimension | Question |
|-----------|----------|
| **Serves** | Who delivers value to whom? |
| **Depends on** | Who blocks whom when they fail? |
| **Information flow** | What data/signals pass between them? |
| **Handoff risk** | Where do things drop? |

Trace from code (API calls, queues, mailers, webhooks) and operations (escalations, emails). Code shows designed flows; interviews reveal workarounds.

## Surface Interest Alignment and Conflict

Identify where interests align and where they conflict (metrics rewarding one actor at another's expense). Misalignment is where failures originate — a frontline team measured on handle time cuts corners, creating backstage rework and customer frustration.

## Map Power Dynamics

Document decision authority, veto power, and affected-but-voiceless actors. Services optimize for the most powerful actor by default. Name who benefits and who bears costs.

## Connect the Service-Profit Chain

Map actors to each link: internal service quality (tools, training, autonomy) → employee satisfaction → service delivery quality → customer satisfaction → revenue/growth. If internal quality is poor, no customer-facing design fixes it. Tag each link.

## Build from Mixed Inputs

Accept code, org charts, transcripts, support tickets, business docs. Cross-reference — a code role absent from the org chart is a finding; an interview actor absent from code is a gap.

## Confidence Discipline

Tag every claim `[confirmed]`, `[hypothesis]`, or `[gap]`. For each hypothesis, note what would confirm it.

## Output

Write to `docs/service-design/<slice>/stakeholder-map.md` using the template at `service-design/templates/stakeholder-map.md`. Use `project-wide` when mapping the full ecosystem.

## Sequencing

Run AFTER `service-design:actor-profile` (needs actors defined). Run BEFORE `service-design:service-blueprint` (informs backstage/support lanes) and `service-design:service-diagnosis` (misalignment causes failures).
