---
name: service-design-ingest
description: Use when you have new source material (transcripts, interviews, support logs, docs, code changes) and want to reason about what existing service design artifacts need updating
---

# Service Design Ingest

**Core principle:** New evidence updates understanding. Process source material and propose specific, traceable updates to existing artifacts.

## Accept and Classify

Accept pasted content, file paths, or URLs. Classify before processing:

| Source type | What it typically reveals |
|-------------|-------------------------|
| **Transcript / interview** | User mental models, emotions, language, unmet expectations |
| **Support log** | Failure points, recovery paths, frequency signals |
| **Ops runbook / code change** | Backstage process shifts, new constraints |
| **Business doc** | Strategic intent, success metrics, org priorities |
| **User feedback** | Peak moments, information gaps |

## Process — Sequential

1. Read the source material in full — ingest, don't summarize.
2. Read existing artifacts for the relevant slice(s) (`docs/service-design/<slice>/`).
3. Extract claims: what does this tell us about customer actions, emotions, backstage processes, fail points, or mental models?
4. Classify each claim against existing artifacts:

| Classification | Meaning | Action |
|---------------|---------|--------|
| **Confirm** | Evidence supports an existing `[hypothesis]` | Upgrade to `[confirmed]`, cite source |
| **Contradict** | Evidence conflicts with an existing entry | Downgrade or flag `[gap]`, explain conflict |
| **Extend** | Elaborates on an existing entry | Propose addition with confidence tag |
| **Novel** | No match in existing artifacts | Propose where it belongs |

5. For each update, state: artifact file, section, current text, proposed text, why (with source reference).
6. Optionally save source material to `docs/service-design/<slice>/sources/` for traceability.

## Confidence Is the Job

The most important output is confidence movement: `[hypothesis]` → `[confirmed]`, or `[confirmed]` → `[gap]` when evidence contradicts. Every ingest should strengthen or challenge what's already written.

## Do Not Auto-Apply

Present proposed updates for human review. The human decides what to accept.

## Related Artifacts

Every sibling skill produces artifacts this skill updates: `service-design:service-blueprint`, `service-design:empathy-analysis`, `service-design:journey-map`. Run ingest first, then the relevant skill to incorporate accepted changes.

