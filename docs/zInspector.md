# zInspector

`zInspector` counts the geometry in the document by type, live, and gives quick actions to select, show, hide, lock or delete by type. It also includes a built-in backface selection tool (`zSelectBackfaces`). A separate command, `zInspectorToggle`, switches the live HUD at the top left on or off without opening the dialog.

## Quick start

1. Run `zInspector`. The panel scans the current document automatically.
2. In the **ITEM** group, tick the geometry types you want to select. The ticks are synchronized to the selection in the viewport.
3. Or use the shortcuts in **QUICK SELECTION** (by shape family, by open or closed, or backfaces).
4. Use the buttons in **ACTIONS** to isolate, lock, delete, show or hide.

## zInspectorToggle

1. Run `zInspectorToggle` directly. You do not need to open the `zInspector` dialog first.
2. Each run flips the current state of the HUD: on becomes off, off becomes on. The command line says "Inspector HUD on" or "Inspector HUD off".

## The dialog

- **Scope** (a segmented control): **Document** (the whole model space) or **Selection** (a snapshot of the objects currently selected in the viewport, not live tracking).
- **ITEM group, 19 categories** in 3 columns:
  - Curves and surfaces: Open / Closed Curve, Open Surface, Open / Closed Polysurface, Open / Closed Extrusion.
  - Meshes, SubDs and other: Open / Closed Mesh, Open / Closed SubD, Point, Block, Group, Bad Geometry.
  - Other: Annotation, Hatch, Clipping Plane, Light.
  - An empty category cannot be ticked.
- **QUICK SELECTION**:
  - **SelCrv / SelSrf / SelMesh**: by shape family.
  - **SelOpen / SelClosed / SelNonGeometry**: open, closed or non-geometry.
  - **SelBackfaces**: a one-shot action. Pick a single-face object and it detects the faces that point away from the current camera and are not hidden behind other faces.
- **ACTIONS**:
  - Row 1 (depends on the ticks): **Isolate** / **Isolock** / **Delete**.
  - Row 2 (visibility for the whole document, independent of the ticks): **Show** / **Hide** / **Swap Hidden**.
  - Row 3 (locking for the whole document): **Unlock** / **Lock** / **Swap Lock**.

## How the settings work together

- **The ticks are synchronized to the selection in the viewport, one way.** Any change of a tick clears the whole selection, selects everything in the ticked categories again and zooms to the selected objects.
- **Isolate and Isolock are available only when something is ticked.**
- **Hide and Lock use the selection if there is one, and otherwise everything.** If objects are selected in the viewport, only they are affected. With nothing selected, the whole document is affected.

## Good to know

- **The HUD exists independently of the dialog.** Closing the dialog does not stop the live HUD at the top left. You have to run `zInspectorToggle` to turn it off.
- **The HUD starts automatically the first time you open the `zInspector` dialog in a session.** It stays on after you close the dialog, and you turn it off with `zInspectorToggle`. After you restart Rhino, this automatic start resets.
- **SelBackfaces is a one-shot pick and does not correspond to any tick.** It depends on the current camera angle, so running it again from another angle gives a different result.
- **SelBackfaces selects only whole single-face objects** (a single-face surface, or a Brep or mesh with exactly 1 face). It does not support picking a sub-face of a multi-face Brep.
- **Scope = Selection is a snapshot, not live tracking.** Pick again to refresh it.
- **Delete asks for no confirmation.** You can only Undo it.
