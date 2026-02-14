---
name: journey-map
description: Use when mapping what a user experiences over time — actions, thoughts, emotions, touchpoints across pre-service, during-service, and post-service stages
---

# Journey Map

**Core principle:** A journey map is a user-centered narrative. It answers "what is this person going through?" not "how does our system work?" — that's the blueprint's job.

## Build from Evidence

Read code (routes, controllers, mailers, views) to extract touchpoints and channels. Read transcripts and support tickets to extract thoughts and emotions. Code reveals *what the user encounters*; it cannot reveal *what they think or feel*. Accept docs, pasted content, URLs — ingest, don't summarize.

## Set Boundaries First

One scenario, one actor. Define start and end from the actor's POV. Three lenses:

| Lens | What it prevents |
|------|-----------------|
| **Lifecycle sequencing** (pre/during/post) | Optimizing one moment while degrading surroundings |
| **Context of use** (environment, stakes, frequency) | Designing for a user who doesn't exist |
| **JTBD forces** (functional, social, emotional) | Reducing the journey to task completion |

## Stage Structure

Three stages: **pre-service -> during-service -> post-service**. Making before-and-after visible prevents local optimization. Per stage, capture:

| Dimension | Guidance |
|-----------|---------|
| **Actions** | Observable behavior |
| **Touchpoints + channels** | Extract from code |
| **Thoughts** | Verbatim quotes when available; mark inferred |
| **Emotions** | Grounded in research, not invented — name + valence |
| **Pain points** | Friction, confusion, anxiety |
| **Opportunities** | Unmet needs |

## Confidence Discipline

Tag every claim: `[confirmed]` (verified in code, transcript, or docs), `[hypothesis]` (inferred), `[gap]` (unknown). Verbatim quotes = `[confirmed]`. Inferred emotions = `[hypothesis]`. Missing stages = `[gap]`. Note what would confirm each hypothesis.

## Relationship to Blueprint

Journey map = user side. Blueprint = org side. Both needed. Two failure modes to prevent:

1. A compelling story that collapses under operational reality
2. Efficient operations users experience as cold or hostile

## Output

Write to `docs/service-design/<slice>/journey-map.md` using the template at `service-design/templates/journey-map.md`.

## Related Artifacts

`service-design:service-blueprint` for the org-side view. `service-design:empathy-analysis` for the deeper emotional/informational layer underlying thoughts and emotions here.
