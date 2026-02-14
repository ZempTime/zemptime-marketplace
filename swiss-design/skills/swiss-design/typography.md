# Typography

**Type carries content and creates structure. It's the only ornament you need.**

Swiss designers chose sans-serif not because it was trendy, but because it was neutral — the type disappears, the content remains.

## One Typeface, Two Weights

```css
--font-family: Inter, -apple-system, sans-serif;
--font-weight-normal: 400;
--font-weight-bold: 600;
```

One family because variety is decoration. Two weights because regular and bold create sufficient hierarchy. Light reduces readability. Extra-bold shouts.

## Type Scale

4-5 sizes max, following a ratio (1.25× / major third):

```css
--text-sm: 0.8rem;    /* 13px */
--text-base: 1rem;    /* 16px */
--text-lg: 1.25rem;   /* 20px */
--text-xl: 1.563rem;  /* 25px */
--text-2xl: 1.953rem; /* 31px */
```

More sizes = weaker system.

## Line Height and Measure

- **Body:** line-height 1.5, max-width 65ch
- **Headings:** line-height 1.2, max-width 25ch
- **Measure:** 45-75 characters per line. Wider loses tracking. Narrower feels choppy.

## Five Treatments — Enough for Most Interfaces

| Element | Size | Weight | Color |
|---------|------|--------|-------|
| Page title | text-2xl | bold | primary |
| Section head | text-lg | bold | primary |
| Body | text-base | normal | primary |
| Caption | text-sm | normal | secondary |
| Label | text-sm | normal | secondary |

## Application

| Context | Rule |
|---------|------|
| Interfaces | Don't use type for decoration. `<h1 class="text-2xl font-bold">` not gradient-text-uppercase-tracking-widest. |
| Data viz | Tabular figures for numbers (`font-variant-numeric: tabular-nums`). 11px minimum for data labels. Axis labels smaller than data labels. |
| Documentation | Monospace for all code. Body at 16px minimum. Headings signal structure, not importance. |
| CLI | Monospace by default. Hierarchy through whitespace and caps, not box-drawing characters. |

## Typography Failures

| Failure | Fix |
|---------|-----|
| Too many fonts | One family + monospace for code. That's it. |
| Too many sizes | If 12, 13, 14, 15, 16px exist in one view, there's no system. |
| Weight as emphasis | Don't bold paragraphs. Bold specific terms. Weight is scarce. |
| Tight line height | Body below 1.4 strains reading. Don't sacrifice readability for density. |

## Choosing a Typeface

- Sans-serif (Inter, SF Pro, Roboto, system stack)
- Neutral (no distinctive personality)
- Wide character set (internationalization)
- Readable at small sizes

The best typeface is one nobody notices.
