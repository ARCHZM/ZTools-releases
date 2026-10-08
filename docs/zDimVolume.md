# zDimVolume

`zDimVolume` dimensions the volume of closed solids, with no hatch. The text is placed above the object, aligned to the center of the top face of the object's own minimum bounding box.

## Quick start

1. Run `zDimVolume`.
2. Pick **closed solids** (Brep, mesh or extrusion).
3. Adjust the **TEXT** group.
4. **Mark** commits the dimensions. **Cancel** discards them.

## TEXT

A row at the top has three presets, **Tiny**, **Small** and **Large Object**. The preset affects only Text Size. This tool has no arrows, so the only group box is **TEXT**.

- **Unit Format**: the volume unit format.
- **Text Size**: the height of the dimension text.
- **Decimal Places**: the number of decimals in the volume value.

## How the settings work together

- **A change to any of the three fields recalculates everything.** A preset switch affects only Text Size.
- **The text direction no longer follows the world XY axes.** The tool works out the smallest-area bounding box of each object (the same rotating-calipers method as `zDimArea` and `zTextureMappingByObjects`) and aligns the text to the long side of the shape itself.
- **The label sits at the center of the top face**, not at the centroid. The text always floats above the top of the solid, so it is never buried inside where you cannot see it.

## Input requirements

**The object must be a closed solid.** A Brep must be solid, a mesh must be closed, and an extrusion must be capped. Open shapes cannot be selected.

## Good to know

- **The HUD at the top right shows the total volume live.**
- **Unlike `zDimArea`, the volume dimension has no hatch**, only the text.
- **If you close the dialog without pressing Mark, the tool removes its empty layer.**
- **This tool also closes its dialog with Ctrl+Z** (the same as `zDimArea`).
