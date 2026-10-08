# zScatterToSrf

`zScatterToSrf` scatters copies of the selected objects over a target surface, randomly or in sequence. It supports collision avoidance and edge trimming.

## Quick start

1. Run `zScatterToSrf`.
2. Select the source objects.
3. Pick the target (a surface, Brep, mesh or extrusion).
4. Adjust **DISTRIBUTION** and **TRANSFORMATION**.
5. **Bake** creates the copies. **Cancel** discards them.

## DISTRIBUTION

- **Placement Mode**: **Random** or **Sequential**.
- **Count**: how many copies to scatter. A plain number, with no re-roll button, because it involves no randomness.
- **Seed**: controls both the sampling of hit points and the pick from the pool of source objects (the two are managed together, because both are always used whether or not Scale and Rotate are on). It has its own re-roll button.
- **Avoid Collision**: on by default.
- **Edge Trimming**: on by default. Prevents objects from hanging over the edge of the target surface.
- **Align**: **Normal** or **World Z**.

## TRANSFORMATION

- **Scale** (a checkbox plus a two-handle range, off by default): it has its own seed and re-roll button.
- **Rotate** (a checkbox plus a two-handle angle range, off by default): it has its own seed and re-roll button, independent of the Scale seed.

## How the settings work together

- **The Distribution Seed, Scale and Rotate each have their own random seed.** Re-rolling one does not change the other two.

## Good to know

- **The sampling budget is fixed (count x 20) and does not grow.** If the target surface is too small or the collision limit is too strict, the requested count may not fit. To place more, reduce the Count or use a larger target.
- **Edge Trimming strictly rejects any placement where a bottom corner does not land on the ground.**
