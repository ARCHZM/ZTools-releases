# zLouver

`zLouver` builds a louver panel on an existing flat face: an outer frame plus a stack of slanted blades. It is typically used for mechanical screens, vents and ventilation openings.

Good for:

- Quickly making louver panels on a flat face such as a wall opening, an equipment band or a screen opening.

Not for:

- A multi-face Brep when you want every face processed. Only the first face is used.

## Quick start

1. Run `zLouver`.
2. Pick one or more planar surfaces, Breps or extrusions to mount the louvers on.
3. The dialog opens with a live preview. Adjust **FRAME** and **BLADE**.
4. **Bake** creates the real geometry. **Cancel** discards the preview.
5. To change a baked louver, select it and run `zLouverEdit`. See Good to know for the limits.

## FRAME

- **Format**: the display unit for every length field.
- **Width**: how wide the outer frame is, measured inward in the plane of the face.
- **Depth**: how far the frame is extruded along the face normal.
- **Offset**: pulls the outline of the mounting face inward before modeling, so neighboring panels have a gap between them.

## BLADE

- **Spacing**: the center-to-center spacing of the blades, top to bottom.
- **Angle**: the tilt of the blades from horizontal, from -89 to 89 degrees. A larger angle makes the blades closer to vertical. Near 0 they are almost horizontal.
- **Thickness**: the thickness of each blade.
- **Depth** (read-only, no checkbox): always equal to the frame depth divided by the absolute cosine of the angle (`FRAME Depth / |cos(Angle)|`), so the blades exactly fit the frame depth. It cannot be typed.
- **Extension**: extra length added to the blades beyond the automatic Depth. Always editable. To make the blades longer, change Extension.

## How the settings work together

- **Depth always follows FRAME Depth and Angle** and cannot be typed. Extension is added on top of this automatic value, and the two together are the real blade length.
- **Angle affects both the tilt and the automatic Depth.** Changing it changes both, but Extension stays as it is.

## Good to know

- **The mounting face must be flat and must not be horizontal.** A non-planar face is skipped with the message that it is not planar. A horizontal face is skipped because it cannot define a vertical direction for the blades. The face must also have a closed outer boundary.
- **Only the first face of a multi-face Brep is used.** The other faces are ignored. To make louvers on several faces, split the object into single faces and pick them separately.
- **You do not need to flip the face.** The tool matches the real face normal, and the frame always grows toward the inside of the real normal.
- **If the mounting face is deleted, the louver can no longer be edited with `zLouverEdit`.** The louver is always rebuilt from its mounting face. If all mounting faces are gone, the edit command refuses and says the source face no longer exists in the document.
- **If the mounting face is moved or scaled, a baked louver does not follow.** It updates only when you run the edit command and bake again.
- **Manual changes to a single blade (moved, recolored, edited) are discarded when you run the edit command**, which deletes the whole set of old blades and builds them again.
