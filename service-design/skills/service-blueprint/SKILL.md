---
name: service-blueprint
description: Use when mapping how a service actually works — frontstage/backstage/support lanes from code, docs, and operational knowledge
---

# Service Blueprint

**Core principle:** A blueprint connects what users experience to what the organization produces. Touchpoints are commitments, not screens.

## Build the Skeleton from Code

Read routes, controllers, models, mailers, and jobs to extract the operational backbone: customer actions, frontstage responses, backstage processes, support systems. Code reveals *what happens*; it cannot reveal *what it feels like* or *why the org chose this shape*. Ask targeted questions for those layers. Accept docs, transcripts, and URLs as supplementary sources — ingest them, don't summarize them.

## Set Boundaries First

Pick one scenario and one actor. Define start and end conditions from the actor's POV. Combine three lenses to find the right boundary:

| Lens | What it prevents |
|------|-----------------|
| **Lifecycle sequencing** (pre/during/post) | Optimizing one moment while degrading the surrounding system |
| **Context of use** (environment, stakes, frequency) | Designing for a user who doesn't exist |
| **JTBD forces** (functional, social, emotional) | Reducing the service to task completion |

## The Three Lines

| Line | What it reveals |
|------|----------------|
| **Interaction** | Where the user touches the org — every crossing is a moment of truth |
| **Visibility** | What's hidden vs. shown — a design choice, not just operational fact |
| **Internal interaction** | Where bottlenecks and systemic failures hide |

## Confidence Discipline

Tag every claim: `[confirmed]` (verified in code or docs), `[hypothesis]` (inferred from patterns), or `[gap]` (unknown). Separate observation from inference. For each hypothesis, note what would confirm it — a stakeholder interview, a log query, a support ticket search.

## Output

Write to `docs/service-design/<slice>/blueprint.md` using the template at `service-design/templates/blueprint.md`. Populate the Service Slice table, Blueprint Lanes, Moments of Truth, and Failure Modes sections.

## Related Artifacts

`service-design:empathy-analysis` produces the emotional/informational layer this blueprint references. `service-design:journey-map` sequences the same service across time and channels. Use all three for a complete picture.
