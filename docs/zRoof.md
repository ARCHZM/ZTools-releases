# zRoof

`zRoof` builds a roof from one planar outline curve. It supports four basic styles, **Gable**, **Hip**, **Gambrel** and **Mansard**, recognizes L-shaped, T-shaped and cross-shaped plans for multi-wing hip roofs, and lets you add **dormers** right in the dialog.

Good for:

- A building outline already drawn in plan, when you need a roof quickly.
- Adding dormers to a roof (gable, hip or shed dormers).
- L, T and cross-shaped plans that should become a hip roof automatically.

Not for:

- Irregular or non-axis-aligned outlines. The tool rejects them as Irregular and builds nothing.

## Quick start

1. Run `zRoof`.
2. Pick a **closed planar polyline** as the roof outline (see Good to know below).
3. The longest edge is used as the ridge reference automatically. You do not choose it.
4. The dialog opens with a live preview in the viewport. Adjust **LAYOUT** and **DIMENSIONS**, and add dormers in **DORMERS** if you need them.
5. **Bake** creates the real geometry. **Cancel** discards the preview.
6. To change a baked roof, select it and run `zRoofEdit`.

## LAYOUT

- **Format**: the display unit for every length field. It only changes the display; values are stored in model units.
- **Style**: the choices depend on your outline.
  - Rectangle: Gable, Hip, Gambrel (barn roof) and Mansard.
  - L, T and cross-shaped plans: Hip only (shown as L-Shape Hip, T-Shape Hip or Cross-Shape Hip).
- **Height Mode**: decides whether the ridge height comes from the slope or is set directly.
  - **Pitch**: the ridge height follows the Pitch value.
  - **Fixed**: you type the ridge height, and the slope is no longer used. This row is hidden for Mansard, which has no single ridge height (the lower and upper slopes control it).
- **Flip Ridge**: Gable and Gambrel only. Turns the automatically chosen longest edge to the other direction, which rotates the ridge by 90 degrees.
- **Force Pyramid**: rectangle outlines with Hip only. When the outline is not square, forces a single pyramid peak instead of a hip roof with a ridge.
- **Match Main**: T and cross-shaped plans with Hip only. Controls whether the ridge of a secondary wing follows the ridge height of the main wing.
  - On: the secondary ridge always equals the main ridge height.
  - Off: the secondary wing uses its own pitch, and the valley at the junction stays parallel to the true hip line.

## DIMENSIONS

- **Pitch**: the general slope for Gable and Hip. Mansard and Gambrel do not use it and use Lower Pitch and Upper Pitch instead. Typical house roofs are about 15 to 45 degrees.
- **Height** (only when Height Mode is Fixed): the ridge height. Not used by Mansard.
- **Overhang**: the horizontal overhang at the eaves.
- **End Overhang**: Gable and Gambrel only. The overhang at the gable ends. Hip, Mansard and multi-wing plans have no gable ends, so the row is removed.
- **Thickness**: the roof slab thickness. At the default of 0 the roof is a zero-thickness surface, which previews fastest. Above 0, the roof is thickened downward along the surface normal. Gable end caps always stay zero thickness.
  While you drag the Thickness slider, the preview shows only the thin zero-thickness panels, not the thickened shape (a speed optimization). **Bake** gives the real thickness.

### Gambrel: SLOPE BREAK

- **Break Frac**: where along the half span the slope changes.
- **Lower Pitch**: the steep lower slope.
- **Upper Pitch**: the shallow upper slope. It is grayed out and ignored when Height Mode is Fixed, because the height is then set directly.

### Mansard: SLOPE BREAK

- **Break Inset**: how far the upper part is pulled in from the outline.
- **Lower Pitch**: the steep lower slope.
- **Upper Pitch**: the shallow upper slope. Set it to 0 degrees for a flat top.

## DORMERS

A table with an editor below it. Each row is a dormer. Click **+ Add Dormer** under the list, then click on the previewed roof to place one. Tick one or more rows and the editor shows their parameters. With several rows ticked, a change applies to all of them.

- **Style**: the roof form of the dormer.
  - **Gable**: a ridge, two slopes and a gable end wall.
  - **Hip**: a hipped front slope, no gable.
  - **Shed**: a single sloping roof with its own pitch.
- **Width**: the dormer width, eave to eave.
- **Height**: the height of the front wall, from the main roof up to the dormer's own eave.
- **Up Offset**: how far to move the dormer up the slope from the point you picked.
- **Pitch** (Shed only): the dormer's own slope, which must be shallower than the main roof. Gable and Hip dormers do not show it, because their ridge follows the main roof pitch.

## How the settings work together

- Height Mode shows either the Pitch row or the Height row, never both.
- For Gambrel and Mansard the general Pitch and Height Mode do not apply. Mansard hides the Height Mode row altogether. Both use the SLOPE BREAK Lower and Upper Pitch.
- The Style choices follow the outline: all four styles for a rectangle, Hip only for L, T and cross-shaped plans. An irregular outline empties and disables the Style list, and no roof can be made.
- Flip Ridge and End Overhang appear only for Gable and Gambrel. Force Pyramid appears only for a rectangle with Hip. Match Main appears only for T or cross-shaped plans with Hip.
- Ticking or unticking dormer rows shows or hides the editor. Changes in the editor are written to all ticked rows.
- Switching a dormer to Shed adds a Pitch row. Switching back removes it.

## Good to know

- **The outline must be exactly a rectangle, L, T or cross shape, not any polygon.** A rectangle has 4 vertices with near right angles, an L has exactly 6 vertices with exactly one 270 degree corner, a T has exactly 8 vertices and a cross has exactly 12. Anything else is Irregular: the Style list is empty and disabled and no roof is made. If the Style list is empty after picking, your outline is not one of these four shapes.
- **The outline must be a closed planar polyline.** An outline with arc fillets cannot be used directly.
- **Dormers are placed on the live preview.** You cannot add one until the current settings produce a valid preview roof.
- **A dormer outside the roof or overlapping another dormer only gives a warning.** It does not stop Bake.
- **If a setting change removes the roof face a dormer sits on, that dormer is skipped silently, but its settings are kept.** Each refresh matches every dormer to the nearest roof face. If you change Style, Pitch or Overhang so that the host face disappears or moves too far, the dormer is not shown for now and a note appears in the viewport. Change the roof settings back and it returns.
- **Toggling Flip Ridge clears all dormers.** Flipping rotates the roof orientation by 90 degrees, so the dormer positions no longer match any roof face. They are removed from the list and you have to add them again with **+ Add Dormer**.
- **There is no value panel in the viewport.** A small red square appears at the top right only when there is a warning (for example a missing dormer host), without the warning text.
- **Deleting the outline curve during creation interrupts the preview** and you are asked to undo or run the command again.
- **Exploding a baked roof group loses the ability to edit it as a whole.** The roof parts are put in one Rhino group. If you explode the group, the parts keep their identity, but without the group, running the edit command again may treat them as new, and the old parts are not cleaned up automatically. Delete the leftovers by hand.
- **Deleting the hidden position reference of a roof makes an edited roof jump back to its original place.** A baked roof carries a hidden reference that remembers whether you moved it. If that reference is deleted, an edited roof appears at the original location instead of where you moved it, with no message.
