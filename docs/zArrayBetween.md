# zArrayBetween

`zArrayBetween` arrays the selected objects between two picked points, along a plane.

## Quick start

1. Run `zArrayBetween`.
2. Select the objects to array.
3. Define the array plane. With a single point or a single straight line selected, the world XY plane is used automatically. In other cases you click three points in turn: the origin, the X axis and the Y side.
4. Pick the start point and the end point.
5. The dialog opens. Adjust **DISTRIBUTION** and **PLANE**.
6. **Bake** creates the array. **Cancel** discards it.

## DISTRIBUTION

- **Mode**: **Fixed Count** or **Fixed Spacing**.
- **Count / Spacing**: shows the one that matches the Mode.
- **Even Fit**: when on, in Fixed Spacing mode the count is rounded to a whole number and the remainder is shared evenly. When off, the copies are placed at the fixed spacing until the distance is used up.

## PLANE

- **Shrink Length** (read-only): how far the two ends are pulled in. It can only be set by picking two points with **Pick Edge**. It cannot be typed.

## Good to know

- **A single point or a single straight line skips the three-point plane step** and uses the world XY plane automatically.
- **A start and an end point that are too close fail.** If the two points coincide or are very close, the command fails.
