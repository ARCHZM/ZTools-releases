# zWindow

`zWindow` builds a window in an existing wall from one closed boundary curve. It supports four opening types, **Fixed**, **Hung**, **Casement** and **Sliding**, and vertical or horizontal divisions.

Good for:

- A wall with an opening outline already drawn, when you need a window model quickly.
- A combination window with a transom.

Not for:

- Odd boundaries that are not a rectangle or a rectangle with one arched top, when you want operable types. Only **Fixed** is available for those.

## Quick start

1. Run `zWindow`.
2. Pick the wall first (you can pick a single face of a multi-face Brep), then pick a **closed** curve for the opening.
3. The dialog opens with a live preview. Adjust **LAYOUT**, **DIVISION** and **DIMENSIONS**. The FRAME, SASH and GLASS headings in DIMENSIONS can be clicked to collapse or expand. They have no switch and are only for tidying: their values always apply.
4. **Bake** cuts the opening in the wall (a Boolean difference with the actual shape of your curve) and then creates the window geometry. **Cancel** discards the preview and the wall is not cut.
5. To change a baked window, select it and run `zWindowEdit`. See Good to know for the limits.

## LAYOUT

- **Unit**: the display unit for every length field.
- **Type**: how the window opens.
  - **Fixed**: does not open, and works with any outline. The related fields in DIMENSIONS are labeled **Bead** instead of **Sash**.
  - **Hung**: the lower sash slides up.
  - **Casement**: swings out on a hinge.
  - **Sliding**: the moving sash slides sideways.
  - Any outline other than a rectangle or a rectangle with one arched top is locked to Fixed and the list is disabled.
- **Hinge / Slide**: Casement and Sliding only. For Casement it is the side of the hinge. For Sliding it is the direction the moving sash slides. Not used by Hung or Fixed.
- **Open Amount**: non-Fixed types only. It shows the open state in the preview and in renders. It does **not** change the baked structure.

## DIVISION

### Horizontal Split (collapsible)

The heading is a switch that divides the window into several side-by-side windows (adding vertical mullions). When open:

- **Amount**: 2 to 8 windows. If Transom is also on, the split applies only to the main window below the transom bar.

### Transom (collapsible)

The heading is a switch that adds a horizontal bar with a fixed transom window above it. If the outline is Sliding with a curved edge, this switch is turned on and locked, because a sliding sash cannot slide in an arched opening, so the arched part must be separated off as a fixed transom. When open:

- **Split At**: shown only when the outline is straight-sided (no arc). The split height measured down from the top of the outline.
- **Spring Offset**: shown only when the outline contains an arc. The offset from the true spring line of the arc, measured down toward the sill. 0 splits exactly at the spring line.
- **Arc Style**: shown only with Transom on and a true semicircular arc.
  - **Bead**: ordinary flat glazing.
  - **Fan**: radiating bars.
- **Ray Count**: shown only for Fan. 1 to 12 bars, spread evenly over the angle of the arc.
- **Hub Ring** (checkbox, on by default): Fan only. Whether the bars meet at a small ring or directly at the center.

## DIMENSIONS

- **Frame Width**: how far the outer frame is offset inward in the plane of the wall.
- **Frame Depth**: how far the frame is extruded along the wall normal.
- **Sash Width / Bead Width**: the sash width for non-Fixed types. For Fixed it becomes the bead width (the same field with a different label).
- **Sash Depth / Bead Depth**: the depth along the wall normal. Always at most half of the Frame Depth.
- **Glass Thick**: the thickness of the glass.
- **Rows**: the number of horizontal cells the glass is divided into. 1 means no division, 2 means one horizontal bar splits the glass in two, and so on. The default is 2. (Since 2026-09-06 this is a cell count. Before that it counted the bars.)
- **Cols**: the number of vertical cells, with the same meaning as Rows. The default is 2.

## How the settings work together

- **If the outline does not support operable types, Type is locked to Fixed.** Hung, Casement, Sliding, divisions and transoms work only when the outline is exactly 4 straight edges (a rectangle) or 3 straight edges plus 1 arc (an arched rectangle). Circles, ellipses, triangles and other shapes can only be Fixed, but still get a frame and glass.
- **Sliding with an arched outline forces Transom on.** See above.
- **Switching Type changes the Sash or Bead label and default values in DIMENSIONS.** Switching to Fixed resets the bead defaults, and switching to any other type resets the sash defaults.
- **The Transom options depend on the outline.** A straight-sided outline shows Split At, an outline with an arc shows Spring Offset, only a true semicircle shows Arc Style, and only Arc Style = Fan shows Ray Count and Hub Ring.

## Good to know

- **The outline curve must be closed.** An open curve makes the command stop.
- **The wall face must be planar and not horizontal.** A non-planar wall face, or a perfectly horizontal one (no defined "up"), is skipped with a message on the command line. The outline must also lie in the same plane as the wall face (coplanar or parallel), otherwise it is skipped.
- **Bake cuts the opening in the wall automatically**, with a Boolean difference using the real shape of your curve (rectangle, arch or any closed shape). The result is written back to the same wall object. The wall must be a solid (a solid Brep or polysurface). If it is not, for example a single open surface, the cut fails, a warning is printed on the command line, the wall stays unchanged, and the window geometry is still created. If the cut splits the wall into several pieces, the tool tries to join them back. The wall is cut only while it is hidden or unlocked during the dialog, and if the cut fails the tool falls back to deleting and re-creating the wall while keeping its layer and other properties. Please check the wall after baking.
- **`zWindowEdit` never cuts the wall again.** Editing recomputes only the frame, sash, glass and bars. The opening already cut in the wall is left alone.
- **If the wall or the outline curve you picked is deleted later, the window can no longer be edited with `zWindowEdit`.** The edit command checks that the sources still exist and refuses if they do not. Delete the window by hand and create it again with `zWindow`.
- **Manual changes to the frame, sash, glass or bars are discarded when you run Edit.** The edit command rebuilds the whole window from the original settings and deletes the old parts. It does not read your hand-edited geometry.
- **Exploding or splitting parts and then editing may leave leftovers or remove the wrong parts.** Exploding does not lose the identity mark of each part (the mark is stored on each part, not on the group), but if some pieces were deleted earlier, the rebuild replaces an incomplete list of old members, which can leave orphaned geometry or remove the wrong pieces.
