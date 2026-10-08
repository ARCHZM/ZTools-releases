# zPackObjects

`zPackObjects` brings the selected objects down to Z = 0 and packs their footprints in two dimensions inside a rectangle you pick. It can also straighten the objects and flip them over automatically.

## Quick start

1. Run `zPackObjects`.
2. Select the objects to pack.
3. Click two points on the world XY plane to draw a rectangle as the boundary. This step is **required** and cannot be done in the dialog.
4. The dialog opens. Adjust **SETTING**. You can drag the boundary rectangle live.
5. **Bake** creates the result. **Cancel** discards it.

## SETTING

- **Gap**: the minimum gap between neighboring objects.
- **Algorithm**: **Skyline** or **MaxRects** (default).
  - **Skyline**: keeps a horizontal outline and each time picks the position where the piece fits and the resulting height is lowest.
  - **MaxRects**: keeps the full set of maximal free rectangles and picks the placement that wastes the least area. It usually packs tighter than Skyline.
- **Straighten** (on by default): aligns the longest edge to the X axis before packing (the same algorithm as `zStraighten`).
- **Auto Vertical Flip** (on by default): decides automatically which side is larger and turns the larger face down.

## How the settings work together

- **Straighten runs before Auto Vertical Flip**, and it uses the true tight bounding box of the rotated geometry (not the corners of the bounding box from before the rotation) to work out the footprint, so the area is not overstated.

## Input requirements

You **must select the objects first and then draw the boundary rectangle by hand.** Canceling the rectangle cancels the whole command.

## Good to know

- **A boundary that is too small does not give an error.** Some objects overflow the boundary, and the boundary line turns red to warn you. The tool always tries to place every object, and never refuses or clips anything for lack of space. A red boundary means it cannot hold all the objects, so make it larger by hand.
- **When even a wide boundary cannot hold everything, the remaining objects are stacked outside the left side of the boundary.** This is handled as a fallback and does not fail.
