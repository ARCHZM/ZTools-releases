# zRailingSide

`zRailingSide` builds a railing along a curve, usually a slab or balcony edge, where the posts are **not** grown up from the plane of the curve but stick out sideways from a vertical face on brackets or clamps. It also supports a completely frameless glass wall.

Good for:

- A slab or balcony edge that needs a railing whose posts are fixed to the side instead of the top.
- A frameless glass guard wall fixed with point clamps or continuous clamps.

Not for:

- A normal railing whose posts grow up from the floor or from the path itself: use `zRailing`.

## Quick start

1. Run `zRailingSide`.
2. Pick one or more curves as the reference along the slab edge (see Good to know).
3. The dialog opens with a live preview. Choose the **System** first, then adjust the groups below.
4. **Bake** creates the real geometry. **Cancel** discards the preview.
5. To change a baked railing, select it and run `zRailingSideEdit` (see Good to know for the limits).

## GENERAL

- **Format**: the display unit for every length field. Only the display changes. Values are stored in model units.
- **System**: the infill. Baluster, Cable, Glass or Metal. Only Glass and Metal can be switched to frameless (by turning the post switch in DIVISION > POST off).
- **Height**: the total railing height, measured from the reference curve to the top of the top rail.
- **Below Slab**: how far the posts or the frameless glass wall reach down below the reference curve to cover the slab edge. It affects only the part below the curve. The top rail height, handrail and everything above the curve stay as they are.
- **Flip In/Out**: which side of the reference curve the railing is built on.

## DIVISION > POST

- **Post** (switch): whether solid posts are made. It can be turned off only when System is Glass or Metal (frameless). Otherwise it is forced on and cannot be turned off.
- **Section**: a round or rectangular post section and its size.
- **Corner**: **Single** (one shared post at a corner) or **Split** (no shared post, one post set back about 10 in on each side). With frameless glass (posts off) this is forced to Single and locked, because there are no posts to split.
- **Distribution**: **Even** (spread evenly with a spacing no larger than Spacing) or **Fixed** (fixed spacing, the remainder collected at the end).
- **Spacing**: the maximum or fixed post spacing.
- **Fitting**: available only when the Intermediate Rail is on. It makes a saddle fitting at the post top that connects the intermediate rail to the top rail.

## DIVISION > INFILL

The content depends on System, and the fields mean the same as in `zRailing`. Baluster adjusts Section and Gap. Cable adjusts Section, Mode (Spacing or Count), Top Margin and Bottom Margin. Glass adjusts Thick, Gap and Clear (Clear only for framed glass with Bottom Rail off). Metal adjusts Gap, Clear, Inset Top/Bottom and Inset Left/Right.

## DIVISION > MOUNT PLATE

The content depends on whether posts are on and on the clamp shape.

**Framed (posts on):**

- MOUNT PLATE **Thickness**: the thickness of the mount plate. This is the only plate thickness you set. The outward offset of the posts is worked out from it plus half the post thickness, and this is true even when you turn the mount plate display off.
- **Plate W / MOUNT PLATE Diameter**: for a rectangular post the label is MOUNT PLATE Width (the width along the curve). For a round post it becomes MOUNT PLATE Diameter (the diameter of the round flange).
- MOUNT PLATE **Height** (rectangular posts only): the vertical height of the mount plate. A round flange has no separate height, so it is not shown.

**Frameless glass (System = Glass and posts off):**

- **Clamp**: the clamp shape.
  - **Rect**: a flat block clamp, holding the glass on one side and the wall on the other.
  - **Cylinder**: a round clamp, the usual real point clamp.
- CLAMP **Near Thickness**: the thickness of the clamp on the wall side. This side takes the main load.
- CLAMP **Far Thickness**: the thickness of the clamp on the glass side. This side mostly acts as a stop and is usually thinner than the wall side.
- **Clamp W / CLAMP Height** (Rect only): the width and height of a rectangular clamp.
- CLAMP **Depth** (Cylinder only): the diameter of a round clamp. It is independent of CLAMP Width and H.

## RAIL > TOP RAIL

The heading is a switch for the top rail. When open:

- **Section**: the top rail cross-section.

## RAIL > INTERMEDIATE RAIL

Available only when Top Rail is on. The heading is a switch. When open:

- **Section**: the intermediate rail cross-section.
- **Drop**: how far the intermediate rail is below the top rail.

## RAIL > BOTTOM RAIL

The heading is a switch. In frameless glass wall mode it is forced off and cannot be turned on, because the glass wall already reaches down to the Below Slab depth and takes the place of the bottom rail. When open:

- **Section**: the bottom rail cross-section.
- **Clearance** (clearance above the floor): meaningful only for framed railings with the bottom rail on.

## RAIL > HANDRAIL

The heading is a switch for a separate accessible handrail. When open:

- **Section**: the handrail cross-section.
- **Connection**: **Standoff** or **Bracket**.
- **Height**: the handrail height above the floor.
- **Lateral**: the horizontal offset of the handrail from the main body of the railing.

**ADA EXTENSION**: the heading is a switch. This heading cannot be collapsed. With the switch off the values below are grayed out but stay visible. They are:

- **Return**: **Down**, **Wall** or **Loop** (a loop back).
- ADA EXTENSION **Length**: the length of the extension at each end.
- **Corner Radius**: the corner radius at the bend of the extension.

## How the settings work together

- **The thickness of the mount plate or clamp decides where the posts really stand, even if the plate is not shown.** The horizontal offset of the posts from the reference curve is worked out from the plate thickness (or the clamp thickness in frameless mode) plus half of the post or glass thickness. Turning the Mount Plate display off does not move the posts. They are still placed as if the plate were there.
- **Below Slab affects only the part below the curve.** It stretches or shortens only the part of the posts or glass wall below the reference curve. The top rail height, handrail height and everything above the curve are unchanged.
- **Glass with the posts off triggers a chain of forced settings.** Bottom Rail is forced off and cannot be turned on, Corner is forced to Single, and the Mount Plate group switches from the plate fields to the glass clamp fields. These come from the frameless glass wall construction itself and are not errors.
- **Rectangular and round posts change the Mount Plate fields and labels.** Round posts use MOUNT PLATE Diameter (flange diameter) and hide MOUNT PLATE Height. Rectangular posts show MOUNT PLATE Width and MOUNT PLATE Height.
- **Turning Top Rail off disables Intermediate Rail.**
- **Fitting depends on the Intermediate Rail.**

## Good to know

- **Very short or invalid curves are skipped silently.** If you pick several curves, the short or invalid ones are skipped and the rest are built. If all of them are invalid, the command is cancelled with a message that no usable curve was picked.
- **The reference curve stands for the slab edge and does not have to be horizontal**, but the tool assumes the curve lies at the real height of the slab edge. A curve that is clearly away from the real edge gives a result that is offset in the same way.
- **Exploding part of a baked railing means that part can never be edited again.** Most parts (posts, infill, mount plates, glass or metal clamps, handrail connectors) are block instances. If you explode one, it loses the stored identity, so the edit command does not recognize it, and it is not cleaned up. Delete the leftovers by hand.
- **Deleting the hidden reference line makes a moved railing jump back to its original place.** If it is deleted, any later edit builds the railing at the original position instead of where you moved it, with no message.
- **A manual change to a single part is thrown away when you run Edit.** Edit deletes and rebuilds the whole railing. It does not patch parts. Any hand-edited part is replaced the next time you edit and bake, with no warning.
- **Copying and pasting a baked railing is safe.** The tool recognizes a copy as independent.
- **If a stored setting is deleted or changed by hand, Edit silently replaces it with the default** and rebuilds, with no message telling you which setting went back to default.
