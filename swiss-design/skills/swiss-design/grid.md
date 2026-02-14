# Grid

**Fewer alignment points = stronger grid. Every unique alignment point adds cognitive load.**

## Base Unit

Choose a base unit. Derive all spacing from multiples of it. 8px is safe — divides evenly at common viewports, works with line heights.

```
xs: 4px (0.5×)  sm: 8px (1×)  md: 16px (2×)
lg: 24px (3×)   xl: 32px (4×) 2xl: 48px (6×)
```

## Alignment Points

**Weak grid:** Elements start at 12px, 16px, 24px, 32px, 40px (5 points)

**Strong grid:** Elements start at 16px or 48px only (2 points)

## Application

| Context | Grid type | Key rule |
|---------|-----------|----------|
| Page layout | Column grid (12-col) | `gap: 24px` (3× base) |
| Vertical rhythm | Baseline grid | Line heights at multiples of base unit |
| Components | Internal grid | Same base unit as page — spacing is fractal |
| Data viz | Axis alignment | Data points align to grid lines. Labels at fixed positions, not floating. |
| Documentation | Margin consistency | All content blocks share the same left margin. Heading levels = structural tiers, no skipping. |
| CLI | Column alignment | Related data shares columns. Indent by consistent amounts (2 or 4 spaces, not mixed). |
| Tables | Data type alignment | Numbers right-align. Text left-aligns. Headers match their column. |

## Grid Violations

**Allowed:** Full-bleed images (intentional break). Pull quotes (offset for emphasis). Optical adjustments (icons at mathematical center appearing misaligned).

**Red flags:** "Just this once" positioning. Pixel nudges to "make it look right." Different spacing in similar components.

If you're nudging pixels, either your grid is wrong or your content doesn't fit. Fix the system, don't patch around it.

## Building a Grid

1. Choose base unit (8px)
2. Define spacing scale (multiples of base)
3. Set column grid (12-col)
4. Apply consistently — no exceptions for 2 weeks
5. Evaluate and adjust (grid serves the content)

The grid is an aid, not a prison. But master it before you break it.
