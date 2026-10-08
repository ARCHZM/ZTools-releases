# zSection (section tools)

The section tools are three closely related commands:

- **`zSection`**: a live-preview section that does not change the original geometry.
- **`zSectionCut`**: a real, irreversible section that replaces geometry.
- **`zSectionManager`**: the central dialog that manages both.

**`zSectionCut` has no command of its own.** It can only be started with the **Cut** button of `zSectionManager`.

Choose by what you need:

- You want to see the inside of a building or model live in the viewport, and be able to switch it on and off, flip it or delete it at any time without changing the original model: use `zSection` (Clip mode).
- You want the section to be really visible in renders and in every display mode (Raytraced included), and you accept that this cannot be undone: use `zSectionCut` (Cut mode).

## Create a section line (zSection)

1. Run `zSection`.
2. Pick one or more target objects (Brep, mesh, extrusion or SubD. Curves and block instances are not supported).
3. Draw a polyline point by point on the construction plane of the current view as the section line (it can have several segments with bends). Press Enter to finish.
4. When the command line says "Pick the side to KEEP", click once on the side you want to keep.
5. When the section line is created, the `zSectionManager` dialog opens automatically.

## Manage section lines (zSectionManager)

1. Run `zSectionManager`, or **double-click** an existing section line in the viewport.
2. Each row of the list is one section line (named like "A-A'"). You can change its color and visibility, flip it, edit it, add or remove target objects, or run Cut.

## The manager dialog

A non-modal dialog with a fixed width and an adjustable height. The main part is a scrollable table. From left to right, each row has:

- **Drag handle**: drag to change the row order (kept in memory for this session only, not saved).
- **Section Line name column**: for example "A-A'", bold when selected. Double-click to rename (this changes only the name shown in the list, for this Rhino session. After a restart it goes back to the automatic name, and the A / A' marks in the viewport do not change). Right-click the name for a menu: Rename, Delete, and for Clip rows also Flip, Add Objects, Remove Objects and Cut (Cut rows do not have these). The Del key deletes the selected section line, and Esc clears the selection.
- **Status column**: a summary such as `[Clip] (9 obj)` or `[Cut] (9 obj) (hidden)`.
- **Color swatch**: left-click selects the row, double-click opens the color picker, right-click shows the menu Change Color...
- **Visibility and Flip**: a pair of icons.
- **Edit**: an icon button.
- **Add and remove target objects**: a pair of icons.
- **Cut**: an icon button.

## How it works

### Clip (zSection, live preview, original geometry unchanged)

The tool takes the render mesh of each target object and builds a "blade" by extruding the polyline along its normal. It splits every target mesh with it and filters by the side you keep. **This is only a viewport overlay.** It does not change the real objects in the document. The original objects are really hidden (not by layer), so in any display mode the old and the new geometry never show at the same time.

### Cut (zSectionCut, geometry replacement, irreversible)

This works only on an existing Clip section line (it converts it to Cut). There is no entry to create a new Cut on its own. After it runs:

- Objects entirely on the kept side stay as they are.
- Objects entirely on the cut-away side are **deleted**.
- Objects that cross the section line get a real geometry operation (a Boolean difference first, falling back to splitting and filling the cut faces by hand if that fails).
- **Only the objects chosen as targets are processed.** An object that was not chosen as a target (even one entirely on the cut-away side) is not deleted and stays as it is.
- An extrusion that crosses the section line becomes an ordinary Brep after the cut. A target that is not at the same height as the section line (for example the line is drawn on the ground and the target is raised a long way) can still be cut.
- Targets that cannot be cut (for example block instances) stay as they are. They are neither deleted nor cut.

How each geometry type is handled: Brep and extrusion try a Boolean difference first, and fall back to splitting plus filling the faces by hand. A mesh is split triangle by triangle, with no cap (the cut is open). A SubD is converted to a Brep first, and **it is no longer a SubD after the cut**.

## How the settings work together

- **Once Cut has run, the Edit, visibility, flip and add / remove target actions of that row are all disabled for good.** Only the color swatch and delete still work. The reason is that the "parameter driven preview" no longer applies: the geometry has really been replaced and cannot be edited back.
- **Visibility works differently for Clip and Cut.** Turning visibility off for a Clip really hides the original objects that it previewed. Turning visibility off for a Cut restores the replaced geometry to the original full shape, and turning it on again applies the cut again.
- **While you edit, all other objects are locked.** After you press Edit, every other object in the document is temporarily locked so you do not select or change them by mistake. They are unlocked automatically when editing ends.

## Input geometry

- **Target objects** can only be Brep, mesh, extrusion or SubD. **Curves and block instances are not supported.**
- **The section line** is drawn on the construction plane of the current view. It can be an open polyline with several bends, and after drawing it is extended outward automatically to cover the whole bounding box of the targets, so you do not have to draw it past the model.
- **Double-clicking to open the manager works only on section line objects that carry the tool's mark.** You need two clicks on the same object within 500 ms.

## Good to know

- **A section line is locked in the viewport and cannot be dragged directly.** Any move or transform of a section line done directly in the viewport is **silently reverted** in the next idle cycle, unless you are inside an Edit session of the manager. You must use the **Edit button** of the manager to really drag the control points. Dragging it directly in the scene has no effect and snaps back.
- **Cut is a real geometry operation and is irreversible. A confirmation appears first, suggesting you back up the file.** The replacement itself is wrapped in one undo record (so Ctrl+Z can in principle bring the original geometry back). After converting to Cut, all the editing, flip and add / remove target abilities of the Clip are gone, and the only way to change it is to delete it and start again.
- **The Cut result is not written into the .3dm file itself. It is restored temporarily when you save.** When you save the document, the tool temporarily restores all geometry replaced by Cut to the original full shape, writes the file, and then applies the cut again. So if you open the same .3dm in other software, you see the uncut original geometry. This is a limit of how Cut works, not a bug.
- **If a section line object is deleted outside `zSectionManager` (for example selecting it in the scene and pressing Delete), the list removes it automatically** and no dead row is left. This is normal.
- **When the viewport display mode has Shadows ticked (in General settings) and the lighting method is Default, Scene or Custom (not Ambient Occlusion), the section preview shows only the shaded faces, without wireframe edges and the red section outline.** (This replaced an earlier problem in which a gray ghost shadow moved with the view.) To see the full wireframe and outline again, switch the lighting method back to Ambient Occlusion, or turn the Shadows checkbox off. This does not affect the visibility and isolate icons or the A / A' marks at the ends of the section line.
