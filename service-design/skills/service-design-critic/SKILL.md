---
name: service-design-critic
description: Use when you have existing service design artifacts and want to stress-test them — finds contradictions, blind spots, missing coverage, and unsupported claims across the artifact suite
---

# Service Design Critic

**Core principle:** Artifacts that agree aren't necessarily right. Artifacts that contradict reveal shallow understanding. Make implicit assumptions explicit. Find what's missing.

## Cross-Reference Checks

Read every artifact in the slice. For each pair:

| Artifact A | Artifact B | What to look for |
|------------|------------|-----------------|
| Blueprint | Empathy map | Confidence claimed at a step with no backstage support? Contradiction. Arc peaks where no moment of truth exists? Misalignment. |
| Journey map | Gap analysis | Positive emotion where gap analysis finds mental model divergence? Investigate. Pain points in one but not the other? Incomplete. |
| Blueprint | Diagnosis | Moments of truth with no fail point analysis? Blind spot. Fail points with no blueprint lane? Orphaned. |
| Empathy map | Journey map | Emotional arcs align? Peaks and endings consistent? Says/thinks contradict journey thoughts? One is wrong. |
| Gap analysis | Diagnosis | Priority gaps with no PDSA test? No path to resolution. PDSA tests on non-priority gaps? Misallocated effort. |
| **All artifacts** | — | `[gap]` tags piling up in one area? That's where you need research, not inference. |

## Confidence Audit

Flag any `[hypothesis]` treated as `[confirmed]` downstream. Flag sections with no confidence tag. Every `[hypothesis]` must state what would confirm it — if it doesn't, the hypothesis is unfalsifiable.

## Coverage Check

Map every stage and touchpoint across all artifacts. Which have thin coverage (one artifact only)? Which have none? Single-perspective findings can't be triangulated.

## Suggested Improvements

For each finding, prescribe a next action:

| Finding type | Action |
|-------------|--------|
| Contradiction | Identify which artifact has weaker evidence. Gather data to resolve. |
| Blind spot | Specify which artifact section to populate. |
| Unsupported claim | Downgrade confidence tag. Note what would support it — interview, code audit, log query, ticket search. |
| Coverage gap | Run the missing skill against that stage. |
| Confidence drift | Correct the downstream tag. Trace the chain of inference. |

## Output

Produce a findings list, not a new artifact. Write to `docs/service-design/<slice>/critic-findings.md` or deliver conversationally. Order by severity: contradictions, blind spots, coverage gaps.
