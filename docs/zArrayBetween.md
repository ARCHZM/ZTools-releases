# zArrayBetween

`zArrayBetween` arrays the selected objects along a path of picked points. The path can be one straight segment (a start and an end point) or a polyline with as many points as you like. All copies are placed in one group.

## Quick start

1. Run `zArrayBetween`.
2. Select the objects to array.
3. Define the array plane. With a single point or a single straight line selected, the world XY plane is used automatically. In other cases you click the origin, then the X axis, then the Y side, and press Enter.
4. Pick the start point, then the end point of the first segment. To make a polyline, keep picking points. Press Enter when you are done. Type `Undo` to remove the last point.
5. The dialog opens. Adjust **DISTRIBUTION** and **END**.
6. **OK** creates the array. **Cancel** discards it.

## DISTRIBUTION

The settings apply to every segment of the path on its own. A polyline is treated as its separate segments, not as one long curve.

- **Mode**: **Fixed Count** or **Fixed Spacing**.
  - **Fixed Count**: that many copies on every segment.
  - **Fixed Spacing**: the spacing restarts at the start of every segment.
- **Count / Spacing**: shows the one that matches the Mode.
- **Even Fit**: when on, in Fixed Spacing mode the count on each segment is rounded to a whole number and the remainder is shared evenly. The actual spacing of every segment is shown next to the check box, for example `actual: 6.66 / 5.40 / 6.10`. When off, the copies are placed at the fixed spacing until the segment is used up.

## END

- **Trim Segment Ends**: pick two points to measure a distance. Every segment of the path is then shortened by that distance at its end, so the last copy of each segment stops short of the corner. Use it when the copies have a width and would overlap at the corners.

## Good to know

- **Copies follow the path.** On a polyline, each copy turns so that its X direction follows the direction of its segment. A copy that sits exactly on a corner turns to the middle direction between the two segments.
- **A corner gets one copy.** Two segments that meet share the copy at their corner. With **Trim Segment Ends** the previous segment stops before the corner and the next segment starts there.
- **A single point or a single straight line skips the plane step** and uses the world XY plane automatically.
- **A start and an end point that are too close fail.** If the two points coincide or are very close, the command fails.
