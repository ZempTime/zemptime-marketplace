---
name: empathy-analysis
description: Use when you need to understand what users believe, feel, and predict — builds empathy maps with emotional/informational layers from research, transcripts, and code
---

# Empathy Analysis

**Core principle:** True empathy = cognitive model of the user's emotional state + what they believe is happening. The test: "Does this make sense for someone who doesn't know what's coming next?"

## Two Layers — Both Required

| Layer | What it captures | Key question |
|-------|-----------------|--------------|
| **Informational** | What users believe is happening vs. what's actually happening. What they need to predict next. Whether labels/cues help them estimate a path's value (information scent). | Where will the mental model diverge from system reality? |
| **Emotional** | Baseline emotion before contact, peak intensity during, ending impression (peak-end rule). Which backstage conditions create negative peaks. | What will they *remember* feeling? |

## Build from Evidence

Accept transcripts, interviews, pasted content, files, and URLs as input — ingest them, don't summarize them. Code reveals what the system actually does; contrast that with what users believe. Populate says/thinks/does/feels quadrants from observed evidence. Never invent quotes or emotions.

## Confidence Discipline

Tag every claim: `[confirmed]` (direct quote or observed behavior), `[hypothesis]` (inferred from patterns), or `[gap]` (unknown). Flag inferred emotions explicitly — "User likely feels frustrated [hypothesis]" not "User feels frustrated." For each hypothesis, note what evidence would confirm or refute it.

## The Actionable Test

Cognitive empathy predicts confusion, fear, and skepticism. If your empathy map cannot predict where users will be confused, it is decorative. Walk each row of the Informational Empathy table and ask: given what they believe, what will they expect next? If the system does something different, that is a failure point.

## Output

Write to `docs/service-design/<slice>/empathy-map.md` using the template at `service-design/templates/empathy-map.md`. Populate the Actor table, Quadrants, Emotional Arc, Informational Empathy, and Information Scent sections.

## Related Artifacts

`service-design:service-blueprint` maps the backstage conditions that create the peaks this analysis identifies. `service-design:journey-map` sequences the same experience across time and channels.
