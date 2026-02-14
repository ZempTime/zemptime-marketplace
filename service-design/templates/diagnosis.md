---
slice: <!-- scenario + actor -->
artifact: diagnosis
last_updated:
sources: []
confidence: hypothesis
---

## Aim Statement `[confirmed]`

<!-- What gets better, for whom, by when -->

| Field | Value |
|-------|-------|
| What improves | <!-- measurable outcome --> |
| For whom | <!-- actor/segment --> |
| By when | <!-- target date --> |
| From/To | <!-- current state -> target state --> |

## Fail Points + Root Causes `[hypothesis]`

| Fail point | Root category | Description | Impact |
|------------|---------------|-------------|--------|
| | policy / training / system / handoff / incentive | | <!-- frequency x severity --> |
| | | | |
| | | | |

## Redundancies `[gap]`

| Duplicate work | Owners | Conflict |
|----------------|--------|----------|
| <!-- what's done twice --> | <!-- who each thinks owns it --> | <!-- consequence --> |
| | | |

## Capacity Constraints `[gap]`

| Constraint | Queue/wait | Cause | Downstream effect |
|------------|------------|-------|-------------------|
| <!-- bottleneck --> | <!-- time --> | <!-- batching, staffing, system --> | <!-- what it delays --> |
| | | | |

## Lane-Anchored Measures `[hypothesis]`

| Lane | Measure | Current | Target |
|------|---------|---------|--------|
| Customer outcomes | <!-- satisfaction, completion, time --> | | |
| Frontline outcomes | <!-- effort, error rate, rework --> | | |
| System outcomes | <!-- throughput, uptime, cost --> | | |

## PDSA Tests `[hypothesis]`

<!-- One per top fail point -->

| Field | Test 1 | Test 2 |
|-------|--------|--------|
| Aim | <!-- what this test proves --> | |
| Change | <!-- smallest safe experiment --> | |
| Measure | <!-- signal of success --> | |
| Expected outcome | <!-- prediction --> | |
| Risk / guardrail | <!-- what could go wrong, stop condition --> | |
| Decision rule | <!-- scale if X, revert if Y --> | |

## SERVQUAL Assessment `[gap]`

| Dimension | Current state | Gap | Priority |
|-----------|---------------|-----|----------|
| Tangibles | <!-- physical/digital evidence quality --> | | high / med / low |
| Reliability | <!-- deliver promised service? --> | | |
| Responsiveness | <!-- willingness, speed --> | | |
| Assurance | <!-- competence, credibility --> | | |
| Empathy | <!-- individualized care --> | | |
