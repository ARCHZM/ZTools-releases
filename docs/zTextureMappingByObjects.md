# zTextureMappingByObjects

`zTextureMappingByObjects` applies a native Rhino **box texture mapping** (channel 1) to each selected object using that object's **own** bounding box, instead of one box around the whole selection. Objects of different sizes and orientations each get a mapping box that fits their own geometry, so textures are not stretched. Optional random rotation and random offset make a batch of objects look less uniform.

## Quick start

1. Run `zTextureMappingByObjects`.
2. Select the objects to map (surfaces, Breps or meshes).
3. The dialog opens with a live preview of the mapping. Adjust **MAPPING** and **RANDOM TRANSFORMATION**.
4. While the dialog is open, Rhino's own mapping gizmo (the interactive handles) is shown. You can use it to look at the current mapping box.
5. **Apply** confirms and keeps the mapping. **Cancel** discards it. If the gizmo is still in the viewport afterwards, press **Esc** to cancel it.

## MAPPING

- **Alignment**: the orientation of the mapping box.
  - **CPlane**: the box axes follow the current construction plane exactly, with no rotation search.
  - **Min BBox** (default): the box Z axis always stays world Z (it does not tilt), and the tool searches in the X/Y plane for the rotation that gives the smallest plan-view area for that object.
  - **Wrap**: the box wraps each object's own minimum bounding box (the same orientation as Min BBox, with the size taken from the object's own box), so the texture stretches with the object instead of using a common size. With Wrap, **Scaling**, **Axes** and **Size X/Y/Z** below are all grayed out and cannot be changed.
- **Scaling**: **Uniform** or **Non-Uniform**.
  - **Uniform**: editing any one of Size X, Y or Z changes the other enabled axes with it.
  - **Non-Uniform**: Size X, Y and Z each take their own value.
- **Axes**: three separate switch chips X, Y and Z that decide which axes take part in the Size settings. A switched-off axis is locked at 1 and grayed out.
- **Size X / Size Y / Size Z**: the size of the mapping box. Always in effect (there is no separate enable switch, except that with Wrap they are grayed out). They override the box size each object would get from its own fit, so objects of different sizes share one texture scale instead of each being stretched. You can type text with a unit suffix (for example `20cm`) and it is converted to the document unit.

## RANDOM TRANSFORMATION

- **Rotation** (a checkbox plus a maximum angle, off by default): when on, each object's mapping box is rotated randomly about the world Z axis through the center of that object's bottom face, up to the angle you set. It has its own seed and re-roll button.
- **Offset** (a checkbox, off by default): when on, each object's mapping box is moved randomly along X, Y and Z.
- **Offset X / Offset Y / Offset Z** (the maximum offset per axis): 5.0 by default. Each axis has its own value, its own seed and its own re-roll button, independent of the others.

## How the settings work together

- **With Scaling = Uniform, axes switched off in Axes do not take part.** For example, if only Z is on, editing Size Z does not change the switched-off Size X and Y (they stay at 1).
- **Random rotation is always about the world Z axis, whatever the Alignment.** Even when the Min BBox box is tilted in plan, the rotation axis does not tilt with it, so the rotation stays predictable.
- **The gizmo is shown once, when the dialog opens, and does not refresh when other settings change.** After Apply or Cancel it may still be in the viewport. Press **Esc** to cancel it.
- **Wrap and Min BBox differ:** with Min BBox the box size comes from Size X/Y/Z (one size for all objects, so one texture scale). With Wrap the box size comes from each object itself (the texture stretches with the object).

## Good to know

- **The gizmo shown when the dialog opens is Rhino's own interactive mapping control.** It is only for viewing and fine-tuning the current box, and it does not follow later setting changes.
- **If you close the dialog without pressing Apply, all mapping changes are undone** and nothing is kept on the objects.
- **The mapping is always worked out from each object's own bounding box.** Even if several objects belong to one group, each object gets its own fitted mapping box, not one large box around the whole group.
