---
slice: <!-- scenario + actor -->
artifact: blueprint
last_updated:
sources: []
confidence: hypothesis
---

## Service Slice

| Field | Value |
|-------|-------|
| Scenario + Actor | `[confirmed]` |
| Start condition | <!-- actor's trigger --> |
| End condition | <!-- actor's "done" --> |
| Service promise | <!-- what "good" looks like --> |
| Non-goals | <!-- explicit exclusions --> |

## Blueprint Lanes

<!-- Read top-to-bottom as time flows left-to-right -->

| Step | 1 | 2 | 3 | 4 | 5 |
|------|---|---|---|---|---|
| **Physical evidence** | <!-- UI, email, artifact --> | | | | |
| **Customer actions** | | | | | |
| --- LINE OF INTERACTION --- | | | | | |
| **Frontstage actions** | <!-- visible to customer --> | | | | |
| --- LINE OF VISIBILITY --- | | | | | |
| **Backstage actions** | <!-- hidden work --> | | | | |
| --- LINE OF INTERNAL INTERACTION --- | | | | | |
| **Support processes** | <!-- systems, policies --> | | | | |

## Moments of Truth `[hypothesis]`

<!-- Each line-of-interaction crossing where trust is won/lost -->

| Crossing | Customer expectation | What actually happens | Failure mode | Consequence |
|----------|---------------------|-----------------------|--------------|-------------|
| | <!-- what they predict --> | | <!-- how it breaks --> | <!-- trust/abandonment/workaround --> |
| | | | | |
| | | | | |

## Failure Modes `[gap]`

| Moment of truth | Root category | Description | Severity |
|-----------------|---------------|-------------|----------|
| | policy / training / system / handoff / incentive | | high / med / low |
| | | | |
