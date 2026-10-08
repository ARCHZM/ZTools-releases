# zSiteModifier

`zSiteModifier` reshapes a site surface so that it follows the top of other objects. It rebuilds the control grid of one or more **original site surfaces**, then drops each control point straight down onto the highest hit on a set of **target objects** (buildings, a terrain mesh, any solids). The result is a smooth new ground that fits the existing buildings or terrain, without editing control points by hand. Control points that miss the targets are filled in from their nearest hit neighbor, so the result is always a complete surface with no holes.

## Quick start

1. Run `zSiteModifier`.
2. Select one or more original site surfaces. Only single surfaces are accepted. Polysurfaces are skipped, with a message on the command line.
3. Press Enter, then select one or more target objects: surfaces, polysurfaces, meshes, solids, extrusions, in any mix.
4. Press Enter. The dialog opens and the original surfaces are hidden while it is open. They are shown again when it closes, and they are never modified.
5. In **SAMPLING**, change **Min UV Length**. The preview updates live.
6. Tick **Show Grid** and **Show Control Pts** if you want to see the control grid.
7. In **SMOOTHING**, raise **Iteration** to smooth the result (the default 0 means no smoothing).
8. Tick **Show Contour** for a contour reference.
9. **Bake** creates one new fitted surface per original surface on the `zSiteModifier` layer. **Cancel** discards the preview.

## SAMPLING

- **Min UV Length**: the smallest cell length of the new control grid, in model units (default 30, no upper limit). The number of control points in U and V is worked out separately from the real length of the surface in each direction, so a non-square surface still gets near-square cells. A smaller value gives a denser grid and a finer fit, but is slower. A larger value gives a coarser grid.
- **Show Grid**: shows the isocurves of the rebuilt grid as white lines. On by default.
- **Show Control Pts**: shows each control point as a red dot. Off by default.

## SMOOTHING

- **Iteration**: how many times the projected heights are smoothed, from 0 to 100 (default 0). The amount of smoothing at each control point depends on the largest height difference to its four neighbors: larger differences are smoothed more, and flat areas are left alone. More iterations give a stronger effect that gradually levels off.
- **Show Contour**: a contour preview for reference, not baked. The spacing is fixed at 1 ft in an imperial document and 1 m in a metric one, and cannot be changed. Every fifth contour is drawn thicker and labeled with its elevation.

## Status bar

On the left, **Control Points** is the total number of control points of all original surfaces after rebuilding. On the right, **Deviation** appears only when Iteration is above 0. It is the average absolute change in height caused by smoothing.

## How things work together

- The preview uses a coarse mesh of the target objects for speed. **Bake** re-meshes the targets with the document's finer quality setting and recomputes, so the baked result is more accurate than the preview.
- Changing Min UV Length rebuilds the grid and reprojects in one step. There is no separate Rebuild button.
- The contours follow the smoothed result, so changing Iteration also changes the contours when Show Contour is on.
- With several original surfaces, all SAMPLING and SMOOTHING values are shared. Bake creates a separate new surface for each original, all on the `zSiteModifier` layer.

## Good to know

- Original surfaces must be single surfaces. Polysurfaces are skipped when you pick.
- Smoothing has no extra strength or radius setting. Smoothing weights are limited to between 0 and 1, so the result cannot overshoot or spike.
- The contour spacing is fixed (1 ft or 1 m).
- The original surfaces are hidden while the dialog is open. If you delete or change an original surface by other means while the dialog is open, that surface's result will fail on Bake. It does not crash, it just produces nothing.
