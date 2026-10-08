# zScatterToCrv

`zScatterToCrv` scatters copies of the selected objects along a path curve. It supports random scale and rotation, an offset to the sides of the curve, and collision avoidance.

## Quick start

1. Run `zScatterToCrv`.
2. Select the objects to scatter.
3. Pick the path curve.
4. Adjust **DISTRIBUTION** and **TRANSFORMATION**.
5. **Bake** creates the copies. **Cancel** discards them.

## DISTRIBUTION

- **Alignment**: **Normal** (follows the curve normal) or **World Z** (always stays upright).
- **Mode**: **By Spacing** (fixed spacing) or **By Count** (fixed number).
- **Value**: the spacing or the count, depending on Mode.
- **Seed**: controls only which object each position takes from the pool of source objects (the pool pick). It has its own re-roll button and is independent of the Scale, Rotate and Offset seeds below.
- **Avoid Collision**: on by default. A position that would collide is skipped.

## TRANSFORMATION

The tool always scatters to both sides of the curve. There is no one-sided or two-sided mode to choose.

- **Offset** (a checkbox plus a maximum distance, off by default): when on, each copy picks a random side of the curve, up to that maximum distance. It has its own seed and re-roll button.
- **Scale** (a checkbox plus a two-handle range, off by default): it has its own seed and re-roll button.
- **Rotate** (a checkbox plus a two-handle angle range, off by default): it has its own seed and re-roll button, independent of the Scale seed.

## How the settings work together

- **Seed, Scale, Rotate and Offset each have their own random seed.** Re-rolling one does not change the result of the other three.

## Good to know

- **Without Avoid Collision, no placement is rejected for collisions.** With it on, a position that hits something is skipped.
- **A curve that fails to parameterize is skipped silently** and does not stop the other curves from scattering.
