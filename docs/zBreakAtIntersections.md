# zBreakAtIntersections

`zBreakAtIntersections` is a one-shot command with no dialog. It breaks curves at the places where they cross each other, and where each curve crosses itself.

## Quick start

1. Run `zBreakAtIntersections`.
2. Select one or more curves and press Enter.

## Parameters

There are no settings. Every curve is checked for self-intersections, and every pair of curves is checked for crossings.

## Good to know

- **Two curves that only meet end to end are not treated as crossing.** If the intersection is exactly an end point of both curves, it counts as touching ends, not a real crossing, and nothing is broken there.
- **A curve with no break points is left as it is.** It is not deleted and rebuilt.
- **Overlaps do not produce break points.** If two curves partly lie on top of each other, only real point crossings count.
- **The command line reports the number of breaks, the number of pieces and the number of intersections.**
