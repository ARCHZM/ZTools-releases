# zDoor

`zDoor` builds a door frame and door leaf on an existing wall face. It is modeled on `zWindow` but much simpler. You pick a wall face, click one base point on it (the bottom center of the opening), and then adjust the width, height, frame, panel thickness, hinge side and swing in a floating dialog with a live preview. When you are happy, **Bake** creates it.

Differences from `zWindow`:

- `zWindow` needs a closed curve drawn on the wall for the opening. `zDoor` does not. The rectangular opening is made from the width and height at the point you click.
- `zDoor` cuts the door opening in the host wall automatically on Bake (a Boolean difference), so you do not have to cut the wall with Rhino's own tools. `zWindow` previews with the wall hidden and cuts it only on Bake, with its own wall-cutting rules (see its manual).

## Quick start

1. Run `zDoor`.
2. Pick a wall face (a face of a surface, Brep or extrusion; use Ctrl+Shift+click to pick a single face of a multi-face Brep).
3. Click a base point on the wall face. This is the lowest point of the opening, centered left to right.
4. The floating dialog opens. The wall is hidden automatically, and a door of the default size (32 in wide by 80 in high) previews at your point.
5. Adjust **Type** (single or double), **Hinge** (single only), **Swing** (in or out) and **Open Amount** (preview angle). The viewport updates live. In **DIMENSIONS**, the collapsible **GENERAL** section has Width, Height and GENERAL Thickness, **GLASS** is on by default (turn it off and its five fields are grayed out but still visible), and **HANDLE** picks the handle style. In **FRAME**, set FRAME Width and Depth at the top, and the **TRANSOM** and **SIDELIGHT** sections each have a real on/off switch. When a switch is off, its own values (Height, Width) stay visible but grayed out.
6. **Bake** cuts the door opening in the wall (a Boolean difference, with the size of the whole opening including the frame area), creates the Frame and Panel geometry and groups it. **Cancel** restores the wall as it was and leaves no door and no cut.
7. To change a baked door, run `zDoorEdit` and select any part of it. The same dialog opens with the saved settings, and Bake replaces the old geometry (keeping the same stable ID).

## LAYOUT

- **Unit**: the display unit for every length (mm or inch). Values are converted to model units on bake.
- **Type**: how the door opens.
  - **Single**: one leaf, hinged on the side given by **Hinge**.
  - **Double**: two equal leaves, each Width / 2, with no center post. Each leaf is hinged on its own outer jamb, so there is no hinge choice.
- **Hinge**: Left or Right (a button group). Visible only when Type = Single.
- **Swing**: which side of the wall the door opens to, In or Out (a button group).
- **Open Amount**: only for the preview and renders. It sets the angle of the leaf around the hinge (0 = closed, 100 = 90 degrees). It does not change the baked geometry, which is always modeled closed.

## DIMENSIONS

### GENERAL

- **Width**: the total opening width. For Double, each leaf is Width / 2.
- **Height**: the opening height, measured up from the base point you clicked.
- GENERAL **Thickness**: the thickness of the door leaf (a solid slab) along the wall normal.

### GLASS (collapsible, with a real switch, on by default)

When on, a rectangular glass window is cut out of each leaf (a Boolean cut from the solid slab, not added on). When off, the five fields stay visible but are grayed out. Collapsing is separate and is controlled only by the small icon next to the heading.

- GLASS **Thickness**: the glass thickness, 0.50 in by default and adjustable from 0.25 in to 1.00 in. It is independent of GENERAL Thickness (the glass is centered in the leaf's thickness band and is not necessarily as thick as the slab).
- **Inset Left / Right / Top / Bottom**: the margin from the glass edge to the matching edge of the leaf. The four values are independent. For Double, Left and Right of the right leaf are mirrored automatically, so both windows are symmetric about the center joint.

### HANDLE (collapsible, no switch)

Every door always has a handle, front and back of each leaf. There is no "off" option.

- HANDLE **Style**: five styles.
  - **Lever**: a 1.5 in round rose with a 4 in stepped grip.
  - **Round Pull**: one continuous bent tube in a D shape, with a default 9 in mounting spacing.
  - **C-Shape Pull**: a round-tube pull with a 90 degree offset in the plane, default 8 in mounting spacing.
  - **Ladder Pull**: one main vertical tube and two short posts that fix it to the door. Length defaults to 66 in (filled in automatically when you switch to this style, maximum 66 in).
  - **L-Shape Pull**: one L-shaped main tube (a vertical run and a horizontal foot with a rounded bend) and four short posts. Length defaults to 30 in and has a maximum of 36 in (the only style with a different limit). The horizontal foot length is worked out from the door width automatically (from the bend to the opposite jamb with 2.5 in left over), so you do not set it.
- **Height**: the height of the handle center, measured from the floor.
- **Length**: shown only for Round Pull, C-Shape Pull, Ladder Pull and L-Shape Pull. The overall length of the handle in the vertical direction (the center to center mounting spacing).

## FRAME

- FRAME **Width**: the width of the frame jamb, offset inward in the plane of the wall from the edge of the opening.
- FRAME **Depth**: the depth of the frame, extruded along the wall normal.

### TRANSOM (collapsible, with a real switch, off by default)

When on, adds a fixed frame and glass strip above the door head, covering the whole opening width (including any sidelight).

- **Height**: the height of the transom panel, measured up from the door head. Always visible, grayed out when the switch is off.

### SIDELIGHT (collapsible, with a real switch, off by default)

When on, adds fixed glass side windows next to the door. With the switch off, there are no sidelights.

- **Side**: Left, Right or Both (default Left). There is no "None", because the switch itself means none.
- **Width**: the width of the sidelight panel, measured outward from the door's own jamb. Always visible, grayed out when the switch is off.

The transom and sidelight frames share one continuous system of vertical and horizontal frame bars with the door frame itself (using the same FRAME Width and FRAME Depth), instead of each being a separate closed frame. This leaves no extra seam lines where the sidelight and the transom meet.

## How the settings work together

- When Type is **Double**, the Hinge row is hidden, because both leaves are hinged at their own outer jamb and there is no hinge choice.
- **Open Amount** changes only the preview. The baked leaf is always the closed position, centered within the FRAME Depth.
- The wall is hidden automatically when the dialog opens. **Cancel** shows it again unchanged. **Bake** cuts the door opening in the wall and then shows it. The wall keeps the same object ID (the geometry of the same object is replaced, not deleted and recreated), so other tools and user text that refer to the wall stay valid.
- The opening is a pure rectangle. Arched or odd-shaped openings are not supported (unlike `zWindow`, which accepts any closed curve).
- If FRAME Width is too large (Width / 2 or more), the offset fails and falls back, with a warning on the command line. You may get only the outer frame with no inner opening.

## Good to know

- **The wall must be a solid (a solid Brep or polysurface) for the cut to work.** If the host wall is not a solid, for example a single open surface, the cut fails, a warning is printed on the command line, the wall stays unchanged and the door geometry is still created.
- If the wall is split into several pieces by the opening, `zDoor` tries to join them back into one object. If joining fails, they stay as separate objects. No geometry is lost silently.
- Automatic wall cutting is the part most likely to leave something unexpected. `zDoor` only cuts while the wall is hidden or unlocked during the dialog, falls back to deleting and re-creating the wall (keeping its layer and other properties) if replacing fails, and always prints a message on the command line when something fails. Even so, please check the wall geometry after baking.
- If the host wall has been deleted by the time you run `zDoorEdit`, it refuses to reopen and says the wall no longer exists.
- By default the opening is a standard size (32 in x 80 in) at the point you click. You do not draw a boundary curve first, as you do for `zWindow`.
- If the door is exploded, or its identity mark (user text) is otherwise destroyed, `zDoorEdit` can no longer recognize it and refuses to edit. This is the same as the edit commands of `zWindow` and `zLouver`.
- The glass window, transom and sidelight are all fixed and cannot open. Unlike `zWindow`, there are no operable sashes or louver panels.
- A transom height or sidelight width of 0 or very small silently produces nothing, with no error.
