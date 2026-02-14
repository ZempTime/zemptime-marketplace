---
name: gap-analysis
description: Use when identifying where user mental models diverge from system reality, where moments of truth occur, and where information scent misleads — finds the structural causes of confusion
---

# Gap Analysis

**Core principle:** Every gap between what users believe is happening and what's actually happening is a design failure waiting to surface. Map the gaps, find the structural causes.

## Mental Model Gaps

For each touchpoint or step, document:

| Column | What it captures |
|--------|-----------------|
| **User belief** | What users predict will happen, based on cues and prior experience |
| **System reality** | What actually happens — read routes, controllers, views, mailers |
| **Consequence** | Confusion, error, churn, support load — the cost of the gap |
| **What would close it** | The design change that aligns model to reality |

Code analysis reveals system reality directly. User beliefs require inference from labels, copy, affordances, and known interaction patterns. Separate confirmed from inferred.

## Moments of Truth

Every crossing of the line of interaction is a moment of truth — an encounter with outsized power to shape trust. For each crossing, capture: expected mental model, likely emotional state, failure modes, and visible evidence shaping perception. Cross-reference the blueprint's interaction line to find these crossings.

## Information Scent

Assess whether labels, cues, and surrounding context help users estimate the value of each path. Weak scent causes confusion and backtracking even when the right answer exists in the system. Misleading scent is worse — it builds false confidence. Rate each location: strong, weak, or misleading.

## Progressive Disclosure Check

Evaluate complexity timing across three phases. Early (onboarding, first contact): overwhelmed or appropriate? Mid (core task): sufficient or noisy? Late (completion, edge cases): starved or adequate? Flag phase mismatches — too much too early, too little too late.

## Sequencing

This skill is most powerful after `service-design:service-blueprint` and `service-design:empathy-analysis` exist for the slice. It cross-references both: the blueprint provides the interaction line and backstage structure, the empathy analysis provides the emotional and informational layer. Can run standalone but yields less precise results.

## Confidence Discipline

Tag every claim: `[confirmed]` (verified in code, docs, or data), `[hypothesis]` (inferred from patterns or analogous systems), or `[gap]` (unknown). Mental model gaps derived from code analysis are `[confirmed]` on the system-reality side; the user-belief side is almost always `[hypothesis]` without direct research. Note what would confirm each hypothesis.

## Output

Write to `docs/service-design/<slice>/gap-analysis.md` using the template at `service-design/templates/gap-analysis.md`. Populate Mental Model Gaps, Moments of Truth, Information Scent, Progressive Disclosure Check, and Priority Gaps sections.
