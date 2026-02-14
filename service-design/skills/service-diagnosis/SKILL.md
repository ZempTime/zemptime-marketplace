---
name: service-diagnosis
description: Use when identifying bottlenecks, waste, and failure modes in a service — designs PDSA improvement experiments with lane-anchored measures
---

# Service Diagnosis

**Core principle:** Diagnosis without measurement is opinion. Measurement without aim is vanity. Every improvement must state what gets better, for whom, and how you'd know.

## Prerequisite

This skill reads an existing blueprint. Run `service-design:service-blueprint` first. The blueprint's lanes, moments of truth, and failure modes are the raw material for diagnosis.

## Identify Fail Points

Mark fail points on the blueprint. For each, trace root cause across five categories:

| Category | What it catches |
|----------|----------------|
| **Policy** | Rules that force bad outcomes — approval gates, eligibility traps, rigid scripts |
| **Training** | Staff lack knowledge or confidence to handle the scenario |
| **System** | Technical failures — downtime, latency, missing integrations |
| **Handoff** | Drops between teams, channels, or stages — no owner, no signal |
| **Incentive** | Metrics that reward behavior misaligned with customer outcomes |

Surface redundancies (duplicate work, conflicting ownership) and capacity constraints (queues, wait times, batching). These are structural — they won't appear in individual complaint data.

## Anchor Measures to Lanes

Map measures directly onto blueprint lanes. Measures that float free of lanes measure nothing actionable.

| Lane | Measures |
|------|----------|
| **Customer outcomes** | Task success rate, time-to-value, perceived clarity |
| **Frontline outcomes** | Handle time, rework rate, escalation rate |
| **System outcomes** | Latency, defect rate, policy exception frequency |

State current and target for each. If current is unknown, tag `[gap]` — that gap is itself a finding.

## Design PDSA Tests

For each top fail point, design a test: aim (what this proves), change (smallest safe experiment), measure (signal of success), expected outcome (prediction), risk/guardrail (stop condition), decision rule (scale if X, revert if Y). Small scale — one segment, one workflow, one week. Avoid big-bang rollouts.

## Value Stream Mapping

When operational bottlenecks dominate — queues, rework, long lead times — diagram material and information flows end to end. Separate value-creating steps from waste (waiting, transport, overprocessing, duplication). The blueprint provides structure; the value stream map provides timing.

## SERVQUAL Lens

Optional but powerful. Score the service against five dimensions:

| Dimension | Question |
|-----------|----------|
| **Tangibles** | Does the physical/digital evidence signal quality? |
| **Reliability** | Does the service deliver what it promised? |
| **Responsiveness** | Does the org act willing and fast? |
| **Assurance** | Do actors trust the service's competence? |
| **Empathy** | Does the service treat the person as individual? |

Map gaps to blueprint lanes. A reliability gap traced to a backstage handoff is actionable. A reliability gap floating in the abstract is not.

## Confidence Discipline

Tag every claim: `[confirmed]` (verified in code, data, or docs), `[hypothesis]` (inferred from patterns), or `[gap]` (unknown). For each hypothesis, note what would confirm or refute it.

## Output

Write to `docs/service-design/<slice>/diagnosis.md` using the template at `service-design/templates/diagnosis.md`. Populate the Aim Statement, Fail Points, Redundancies, Capacity Constraints, Lane-Anchored Measures, PDSA Tests, and SERVQUAL Assessment sections.
