---
name: service-design-coordinator
description: Use when starting or continuing service design analysis for a project — checks what artifacts exist, identifies gaps, recommends which analytical skill to run next
---

# Service Design Coordinator

**Core principle:** A complete analysis for a service slice requires all 7 artifact types. The coordinator ensures coverage, not perfection.

**When invoked, execute this workflow. Do not skip phases.**

---

## Phase 1: Inventory

Scan `docs/service-design/` for existing slices. For the target slice, list every artifact with its `last_updated` date. If no slice exists, ask the user to name one before proceeding.

**Say:** "Slice `<name>` has N/7 artifacts: [list present], missing: [list absent]."

Flag any artifact older than 30 days as stale — treat stale the same as missing for recommendation purposes.

---

## Phase 2: Recommend Next Artifact

Use this default order. Earlier artifacts feed later ones — but any order works if the user has context to supply.

| # | Skill | Rationale | Prerequisites |
|---|-------|-----------|---------------|
| 1 | `service-design:actor-profile` | Shared foundation; every artifact references actors | None |
| 2 | `service-design:stakeholder-map` | How all actors connect; staff experience drives customer experience | Actor profiles |
| 3 | `service-design:service-blueprint` | Operational backbone; other artifacts reference it | Stakeholder map enriches it |
| 4 | `service-design:empathy-analysis` | User's inner world (emotions, information needs) | Actor profiles (blueprint enriches it) |
| 5 | `service-design:journey-map` | User's experience sequenced over time and channels | Actor profiles (blueprint + empathy enrich it) |
| 6 | `service-design:gap-analysis` | Where understanding diverges from reality | Empathy + journey recommended |
| 7 | `service-design:service-diagnosis` | Bottlenecks + improvement design | Blueprint + stakeholder map recommended |
| 8 | `service-design:service-design-critic` | Stress-test the whole suite | 3+ artifacts required |

Pick the first missing artifact in this order. If all 7 exist, recommend the critic.

**Say:** "Recommend running `<skill>` next because [rationale]. Invoke now?"

---

## Phase 3: Invoke

If the user confirms (or didn't object), invoke the recommended skill immediately. Pass the slice name as context.

After the skill completes, return to Phase 1 — re-inventory and recommend again until the suite is complete or the user stops.

---

**Remember:** This is a command. Execute each phase in order. Inventory before recommending, recommend before invoking.
