# Reduction

**For each element, ask: if I remove this, what do users lose?**

- "Nothing" → remove
- "Beauty" or "polish" → remove
- "They won't understand X" → keep

## What to Remove

| Context | Remove | Keep |
|---------|--------|------|
| Interfaces | Decorative borders/shadows when whitespace suffices. Icons duplicating adjacent labels. Color not encoding meaning. Helper text restating the obvious. | Separation creating semantic grouping. Color encoding state/severity. Motion showing causation. |
| Data viz | Grid lines when density implies scale. Legends when direct labeling works. 3D effects, gradients. Redundant encoding (position AND color for same variable). | Axis labels with units. Labels when precision matters. Reference lines at meaningful thresholds. |
| Documentation | Introductory paragraphs delaying content. Hedging language. Obvious statements. Section headers for single paragraphs. | Context preventing misuse. Examples that demonstrate. Warnings preventing real errors. |
| CLI | ASCII art, decorative banners. Progress for sub-second ops. Confirmation of obvious success. | Errors with actionable context. Progress for ops over 2s. Color for severity only. |

## Before / After

```html
<!-- Before: card, shadow, icon, divider, helper text -->
<div class="card shadow-lg rounded-xl border p-6">
  <div class="flex items-center gap-2 mb-4">
    <UserIcon class="w-5 h-5 text-blue-500" />
    <h3 class="text-lg font-semibold">User Profile</h3>
  </div>
  <div class="divider border-t my-4"></div>
  <p class="text-sm">View and edit your profile information below.</p>
</div>

<!-- After: the heading does the work -->
<section>
  <h3>User Profile</h3>
</section>
```

## Reduction Traps

| Trap | Reality |
|------|---------|
| "Users expect it" | They expect to accomplish their goal. Not specific decoration. |
| "It looks empty" | Empty space is breathing room, not a problem to solve. |
| "It's just a small addition" | Each element adds cognitive load. Small additions compound. |
| "Competitors have it" | Competitors may be wrong. |

## Process

1. Build with all elements you think you need
2. Remove one element
3. Can users still accomplish the goal?
4. If yes, repeat from 2
5. If no, restore and stop

You've found the minimum when removing anything breaks comprehension.
