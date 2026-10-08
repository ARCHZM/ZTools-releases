# zWindowByCrv

`zWindowByCrv` builds a whole row of windows along a polyline baseline on an existing wall in one go. Each straight segment of the baseline becomes one window, and neighboring segments meet directly at the corners. It is made for corner windows and curved facades (approximated by a polyline), which would be tedious to build one window at a time with `zWindow`. Each window consists of a **Frame**, a **Sash** (glass bead), **Glass** and dividing muntins, and baking also cuts the opening in the wall.

Use `zWindow` instead when you have a single window: it supports casement and sliding types. `zWindowByCrv` only makes fixed windows.

## Quick start

1. Run `zWindowByCrv`.
2. Pick the **wall** (surface, Brep or extrusion).
3. Pick the **baseline curve**, a polyline where each straight segment is one window. The height of the baseline is the sill height.
4. The view zooms to the baseline and the dialog opens. The picked wall is hidden while the dialog is open, and the preview shows only the windows.
5. Adjust **LAYOUT**, **DIVISION** and **DIMENSIONS**. The preview updates live.
6. **Bake** creates the windows and cuts the opening in the wall. **Cancel** discards everything and the wall is restored.

## Parameters

All lengths are entered in the **Unit** chosen at the top (mm or inch). Switching the unit converts the values so the model stays the same.

### LAYOUT

- **Unit**: mm or inch, for every length field below.
- **Window Offset** (default 2 in): pushes the whole window (frame, bead, glass and muntins) into the wall along the inward wall normal. 0 means the outer face of the frame is flush with the wall face.
- **Window Height** (default 48 in): the window height, measured up from the Z of the baseline. The sill and the margins at both ends are fixed at 0, so the glass runs all the way down to the baseline and out to every free end.

### DIVISION

**Glass Split** switch (off by default). Off gives one pane of glass per segment. On divides the glass using the two values below.

- **Column Width** (default 24 in): the target width of each glass column. Each segment works out its own number of columns from its visible glass length: columns = round(visible length / Column Width), at least 1, all equal in width within the segment.
- **Row Height** (default 24 in): the target height of each glass row. Rows = round(window height / Row Height), at least 1, all equal in height.

### DIMENSIONS

The three headings can be clicked to collapse or expand. This only changes what you see, not the values.

- **Frame**: Frame Width (default 2.5 in) and Frame Depth (along the wall normal, default 4 in).
- **Sash**: Sash Width and Sash Depth (both 1 in by default). Here the sash is the glass bead around each pane, since a fixed window has no real sash. Sash Depth can be at most half of Frame Depth, and larger values are cut back.
- **Glass**: Glass Thick (default 0.5 in).

## How corners are handled

- **Glass meets glass at the corner.** Neighboring panes touch directly, with no post and no vertical frame at the corner.
- **Head and sill bars are mitered.** The horizontal frame bars above and below are cut along the bisector of the corner, so there is no overlap or gap at any angle.
- **Vertical frames only at the free ends.** Only the far left and far right ends of the row, not the corners, have a vertical frame.
- **Muntins and beads** are trimmed back to the corner and never run into the neighboring segment.

## How the settings work together

- With Glass Split off, Column Width and Row Height are grayed out and keep their values.
- The position of the glass at a corner changes with Window Offset, Frame Depth and the sash values. This is normal: when the whole window moves into the wall, the corner joint moves with it.

## Good to know

- **The baseline must be a polyline.** Each straight segment makes one window, and arcs or curved segments are skipped. If no straight segment is usable, the command line says `baseline curve has no usable straight segments`. For a curved facade, approximate it with enough short segments first.
- **Every segment needs a wall.** Each baseline segment must be close to a face of the picked wall (within about 6 in). A segment with no matching face is skipped with a message on the command line, and the other segments are built normally.
- **The wall can be a single L-shaped or cornered object.** You do not need one wall object per segment.
- **The opening in the wall** is cut once along the whole baseline when you bake. It extends about 2 ft on each side of the baseline, which goes through ordinary wall thicknesses.
- **What you get after baking:** Frame, Glass and Muntins (bead and muntin bars) each go on their own sublayer under the zWindow root layer, and all parts are put in one group. The frame profiles are joined into one piece when possible, and kept as separate pieces if joining fails.
- **There is no companion Edit command.** To change a baked row, undo and run the command again, or use Rhino's own editing tools.
- **If you close the dialog without pressing Bake,** the wall is restored and every preview is removed. Nothing is left behind.
