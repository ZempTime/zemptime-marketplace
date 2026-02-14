---
name: swiss-design
description: Use when generating any output a human will read — interfaces, documentation, data visualizations, CLI output, tables, diagrams
---

# Swiss Design for Software

**Core principle:** Clarity through reduction. Every element must earn its place. Remove until removing more would harm understanding.

## The Four Principles

| Principle | Rule | Test |
|-----------|------|------|
| **Reduction** | If it doesn't serve comprehension, it's noise. Remove it. | Can I remove this without losing meaning? |
| **Grid** | Mathematical structure creates visual order. Alignment implies relationship. | Does alignment create rhythm and relationships? |
| **Hierarchy** | Control attention through contrast in size, weight, position. Three levels max. | Is there a clear reading order at a glance? |
| **Typography** | Type carries content and creates structure. One typeface, two weights. | Is type doing the structural work, not color or decoration? |

Deep reference for each: [reduction.md], [grid.md], [hierarchy.md], [typography.md]

## Apply to Every Output

Before finalizing, run each test above. If any answer is "no," revise. This is not optional.

**Verification — the grayscale test:** If the design doesn't work in grayscale, the structure is weak. Color supplements hierarchy; it never creates it.

**Verification — the squint test:** Blur your vision. What do you see first? That's your primary element. If nothing stands out, hierarchy is broken.

## Common Failures

| Failure | Symptom | Fix |
|---------|---------|-----|
| Decoration creep | Gradients, shadows, icons that don't encode meaning | Remove. Does comprehension survive? |
| Hierarchy collapse | Everything bold, 12 font sizes, rainbow colors | Three levels max. One lever per level. |
| Grid abandonment | "Just this once" alignment exceptions | Fix the system, don't patch around it |
| Color as crutch | Hierarchy exists only in color, not structure | Make it work in grayscale first |
| Filling empty space | Adding elements because "it looks empty" | Whitespace is breathing room, not a problem |

## Red Flags

- Adding decoration after the design "works" but "looks plain"
- More than 5 type sizes in one view
- Elements at more than 3 alignment points
- Color distinguishing things that should differ in size or weight
- "Users expect it" as justification for an element (they expect to accomplish their goal)

**All of these mean: you're decorating, not designing. Strip back.**
