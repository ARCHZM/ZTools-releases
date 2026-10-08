# zProjectToGround

`zProjectToGround` drops the selected objects down along the world -Z direction onto the ground you pick. It also has an optional random scale and rotation.

## Quick start

1. Run `zProjectToGround`.
2. Select the objects to project.
3. Pick the ground (a Brep, mesh or extrusion). If you pick nothing, the world XY plane at Z = 0 is used.
4. Adjust **PROJECTION** and **TRANSFORMATION**.
5. **Bake** applies it. **Cancel** discards it.

## PROJECTION

- **Mode**: **Center** or **Corners**.
  - **Center**: one ray down from the center of the bottom of the bounding box. A pure Z move.
  - **Corners**: a ray down from each of the 4 bottom corners of the bounding box. The one with the largest drop is used, so the lowest corner lands on the ground. Good for sloping ground.
- **Avoid Collision** (on by default): if the new position collides with an object that has already been placed, the object falls back to projection only (the random scale and rotation are discarded).

## TRANSFORMATION

After the projection you can add a random scale and rotation to each object. They happen about the projected center of the object's bottom, so the object does not lift away from the ground.

- **Scale** (a checkbox plus a random range, off by default): a two-handle range, with its own seed and re-roll button.
- **Rotate** (a checkbox plus a random angle range, off by default): a two-handle angle range, with its own seed and re-roll button, independent of the Scale seed.

## How the settings work together

- **In Corners mode, Scale and Rotate can change which bottom corner actually lands on the ground.** The tool corrects the landing position once more using the transformed bounding box, with no switch needed. In Center mode the base point is the landing point itself, so Scale and Rotate never lift the object off the ground.
- **Avoid Collision and the random transforms:** on a collision, Avoid Collision keeps the landing position but discards this object's random scale and rotation. With it off, the transform is applied as normal and the collision remains.

## Good to know

- **You can press Enter without picking a ground.** The objects are projected onto the world XY plane at Z = 0.
- **An object whose bounding-box center and corners all miss the ground is marked as failed and does not move.** It does not stop the other objects.
