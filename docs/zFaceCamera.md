# zFaceCamera

`zFaceCamera` rotates the selected faces about the world Z axis so that their average normal points toward the camera of the current view. There is no dialog. It computes and rotates the moment you confirm the selection.

Good for:

- Quickly turning a batch of signs, billboards or flat panels toward the current view.

Not for:

- A layout that must follow the camera live. The command computes once at the moment it runs. It is not a live tracking tool.

## Quick start

1. Turn the view to the angle you want the objects to face.
2. Run `zFaceCamera`, select one or more faces, polysurfaces, meshes or extrusions, and press Enter.
3. The command calculates and rotates at once. There is no confirmation and no preview.

## How it works

There are no settings. Everything is automatic.

- **Axis and pivot:** the rotation is always about the world Z axis, and the pivot is the center of each object's own bounding box. With several objects selected, each rotates about its own center, not about the center of the whole selection.
- **How the angle is worked out:** for each object, the tool finds the area-weighted average normal and projects it onto the XY plane. It then takes the direction from the object's pivot to the current camera position, also projected onto the XY plane. The difference of the two directions in the XY plane is the rotation. The camera position is read once when the command starts and does not follow later changes of the view.
- **Supported geometry:** Brep, extrusion, mesh and single surface. The normal is sampled differently for each type (at each face center for Brep and extrusion, per face for a mesh, one normal for a surface), but all of them end up as one area-weighted average direction.

## How the settings work together

- **With several objects selected, each one works out its own angle and rotates about its own bounding-box center.** The group is not treated as a whole, so the relative positions of several faces change after the rotation.

## Good to know

- **Turn the view first.** The camera position is read only at the moment the command runs, so turning the view afterwards does not adjust anything.
- **There is no preview.** The rotation happens as soon as you press Enter. If you do not like it, use Undo.
- **The rotation is in the horizontal plane only (about Z).** It does not change the pitch or height of the objects. If you need the face to look straight at the camera including the tilt, this tool cannot do that.
