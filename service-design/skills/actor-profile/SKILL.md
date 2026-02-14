---
name: actor-profile
description: Use when defining who the actors in a service are — segments, JTBD forces, context, constraints, and relationships between actors
---

# Actor Profile

**Core principle:** Every artifact references actors. If actors aren't defined, every artifact invents its own definition. Actor profiles are the shared foundation — run this skill before all others.

## JTBD Forces — Why They Switch

Map four forces per actor. These explain why someone adopts or abandons a service, distinct from what they experience once using it:

| Force | Question |
|-------|----------|
| **Push** | What's broken about their current solution? |
| **Pull** | What about this service looks better? |
| **Anxiety** | What makes them hesitate? |
| **Habit** | What keeps them doing it the old way? |

Extend to functional, social, and emotional goals. Task completion alone misses half the switching calculus.

## Context of Use

Environment, stakes, frequency, constraints. A user on a phone in a noisy shop with 30 seconds is not the same actor as one at a desk with an hour. Make these explicit.

## Relationships Between Actors

Customer ↔ frontline staff ↔ back-office ↔ external partners. Map who serves whom, who depends on whom, who sets terms. One actor's constraint is often another's backstage action.

## Build from Mixed Inputs

Code reveals what the system assumes about users — roles, permissions, workflows. Transcripts reveal what they actually think and struggle with. Business docs reveal intended segments. Accept all three; triangulate. Never invent quotes or emotions.

One profile per actor type. Multiple actors = multiple files in the same slice.

## Confidence Discipline

Tag every claim: `[confirmed]` (verified in code, analytics, or direct quote), `[hypothesis]` (inferred from patterns), or `[gap]` (unknown). Segments from analytics = `[confirmed]`. Inferred motivations = `[hypothesis]`. For each hypothesis, note what would confirm.

## Output

Write to `docs/service-design/<slice>/actor-profile-<name>.md` using the template at `service-design/templates/actor-profile.md`. One file per actor type.

## Related Artifacts

`service-design:empathy-analysis` deepens the emotional layer for a single actor this profile defines. `service-design:service-blueprint` maps the service structure actors move through. `service-design:journey-map` sequences their experience across time.
