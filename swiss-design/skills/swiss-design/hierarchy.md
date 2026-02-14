# Hierarchy

**If everything is emphasized, nothing is. Hierarchy converts a collection of elements into a sequence.**

## Three Levers

| Lever | How | Rule |
|-------|-----|------|
| **Size** | Larger attracts first | |
| **Weight** | Bolder stands out | |
| **Position** | Top-left scanned first; isolation draws focus | |

Use one lever strongly per level. Using all three on every element flattens hierarchy.

## Three Levels Maximum

| Level | Purpose | Treatment |
|-------|---------|-----------|
| Primary | Main action or message | Large, bold, prominent position |
| Secondary | Supporting actions, context | Regular size, normal weight |
| Tertiary | Metadata, auxiliary | Smaller, lighter, peripheral |

One primary action per view. If two things are equally important, neither is important.

## Before / After

```html
<!-- Broken: everything competes -->
<h1 class="text-xl font-bold text-blue-600">Title</h1>
<p class="text-lg font-medium">Content</p>
<span class="text-base font-semibold">Jan 15</span>

<!-- Clear: each level is distinct -->
<h1 class="text-2xl font-bold">Title</h1>
<p class="text-base">Content</p>
<span class="text-sm text-gray-500">Jan 15</span>
```

## Application

| Context | Primary | Secondary | Tertiary |
|---------|---------|-----------|----------|
| Interfaces | Page title, main CTA | Body, supporting actions | Metadata, captions |
| Data viz | Data values | Axis labels | Grid lines, legends |
| Documentation | Code examples, headings | Body text | Notes, caveats |
| CLI | Errors, main output | Context, details | Suggestions, help |

## Hierarchy Failures

| Failure | Fix |
|---------|-----|
| Everything bold | Reserve bold for true primary elements |
| Rainbow colors | Color is one lever — use it sparingly |
| Size escalation | Set sizes once. Don't inflate for each new feature. |
| Position chaos | Anchor important elements in high-attention zones |

## Verification

1. Rank elements — what's most important?
2. Assign levels — primary, secondary, tertiary. No more.
3. Choose one lever per level
4. Create obvious contrast between levels (subtle gaps fail)
5. Squint test — can you see the hierarchy at a glance?

Hierarchy is sacrifice. To make something important, make other things less important.
