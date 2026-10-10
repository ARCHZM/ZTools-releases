# zDimArea

`zDimArea` dimensions the area of closed planar curves, faces, Breps and meshes. It draws a 45 degree hatch at the center of each object to mark it.

## Quick start

1. Run `zDimArea`.
2. Pick **closed and planar** curves, or any face, Brep or mesh.
3. Adjust the **TEXT** group.
4. **Mark** commits the dimensions. **Cancel** discards them.

## TEXT

A row at the top has three presets, **Tiny**, **Small** and **Large Object**. The preset affects only TEXT Size. This tool has no arrows, so the only group box is **TEXT**.

- TEXT **Unit**: the area unit format.
- TEXT **Size**: the height of the dimension text.
- **Decimal Places**: the number of decimals in the area value.

## How the settings work together

- **A change to any of the three fields recalculates everything.** A preset switch affects only TEXT Size.
- **The text direction no longer follows the world XY axes.** The tool works out the smallest-area bounding box of each object and aligns the text to the long side of the shape itself, so it is no longer always horizontal.
- **Circles are the exception.** If the picked object is a circle (or a closed curve that reduces to a circle), the text still follows the world axes, because a circle has no clear long side and aligning to it would make the text direction jitter between nearly identical circles.

## Input requirements

**Curves must be closed and planar.** Curves that are not cannot be selected, because the picker filters them out. Faces, Breps and meshes have no extra limits.

## Good to know

- **The HUD at the top right shows the total area live**, so you do not have to add it up.
- **Every area label comes with a 45 degree hatch**, shown together with the area text.
- **If you close the dialog without pressing Mark, the tool removes its empty layer.** After Mark, all the dimensions (hatch included) are put in one group.
- **This tool also closes its dialog with Ctrl+Z** (the same as `zDimVolume`. The other zDim tools do not have this shortcut).
