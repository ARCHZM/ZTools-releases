# zStairByRegion

`zStairByRegion` builds a stair from one **closed rectangular outline**. You draw the plan of the stair hall, and the tool folds a stair into it. You can choose Straight, L, U, Z or C layouts.

Good for:

- A known stair hall or stair well in plan, when you want a stair that reaches the total height quickly.
- Landings that are added automatically when a flight has more steps than the code allows.
- Single-storey or continuous multi-storey stairs (including basements).

Not for:

- A stair path that is not a rectangle or regular region, for example along a free curve: use `zStairByCrv`.
- Spiral or winding stairs: use `zSpiralStair`.

## Quick start

1. Run `zStairByRegion`.
2. Pick a **closed rectangular outline** for the stair area (see Good to know).
3. You are asked to click an edge of the outline as the **start edge**, which decides where the stair begins to climb. If you press Esc to skip this step, the first edge of the outline is used, with no error.
4. The dialog opens with a live preview. Adjust the groups below.
5. **Bake** creates the real geometry. **Cancel** discards the preview.
6. To change a baked stair, select it and run `zStairByRegionEdit` (see Good to know for the limits).

## LAYOUT

- **Format**: the display unit for every length field. Only the display changes. Values are stored in model units.
- **Align**: which way the stair width grows from the start edge you picked.
  - **Left**: the width grows to the left of the start edge.
  - **Center**: the start edge is the center, and the width grows to both sides.
  - **Right**: the width grows to the right of the start edge.
  This only places the stair sideways inside the region. It does not change the direction of climb.
- **Story** (Single / Multiple): one storey or several continuous storeys.
  - **Single**: one stair climbing to the Height you set.
  - **Multiple**: continuous stairs for several storeys. The Height row becomes a table of **Typical / Ground / Basement** rows (floor-to-floor height and count). The Style choices narrow to U and C only, because the other styles cannot be stacked vertically.
- **Style** (shown below Type): how the stair is folded into the region in plan.
  - **Straight**: one straight run, no turn. The simplest, and the longest.
  - **L**: turns 90 degrees once, good for a corner.
  - **U**: turns 180 degrees once, fits more climb in a shorter length, with a landing in the middle.
  - **Z**: like U but with a different turn, climbing in a zigzag.
  - **C**: several turns, usually for a stair well enclosed by walls on three or more sides.
- **Type**: how the stair is built.
  - **Plank**: separate horizontal treads and vertical risers, for timber or steel stair looks. DIMENSIONS then also shows **Tread T**, **Riser T** and **Nosing**.
  - **Monolithic**: one solid sloping slab, like a cast-in-place concrete stair. DIMENSIONS then shows **Slab T**, and with **Egress** on also **Tilt**.
  - **Ramp**: no steps, just one continuous ramp, for accessible ramps and service access. DIMENSIONS then shows **Ramp T**, and **Stringer is forced off** even if you had turned it on. Switching back to Plank or Monolithic turns Stringer on again.
- **Mirror Stair Flight** (formerly Mirror): flips the whole stair left to right about the perpendicular bisector of the start edge (this changes the turn direction of L, U and Z). Use it to change the hand of the stair.
- **Add Entry Landing** (formerly Egress, available only for multi-storey stairs): builds the stair by the rules for an enclosed exit stair (IBC). The stair hugs the walls, a full-width landing is made at the entrance, and the landings at turns are larger. **It can be turned on only when Story = Multiple.** In Single mode the switch is disabled and forced off.
- **Trim Underground Geo** (formerly Trim Ground): cuts away the part of the stringers or slab below the stair's own ground level, so the underside sits on the ground.

## DIMENSIONS > GENERAL

- **Height**: the total height to climb in Single mode, about 120 in to 240 in (default 180 in). A larger value gives more steps and a longer stair. A very small value may give only one or two steps.
- **Width**: the stair width, constant along the stair. About 36 in is common for houses and 48 in or more for public buildings. A very small value can make the stringer and rail inset look cramped.

## DIMENSIONS > RISER

- **Riser** (target riser height): the step height you want. The tool divides the total height by this value and rounds to get the number of steps, then divides the total height by that number to get the real riser height. So **the final height of each step is not exactly the number you typed**, it is adjusted by rounding, usually only a little. A sensible range for houses is about 6.5 in to 7.5 in. Too high gives steep steps, too low gives more steps and a longer stair.
- **Riser T**: the thickness of the riser board, 0.1 in to 4 in. Shown only for Plank.

## DIMENSIONS > TREAD

- **Going** (formerly Tread D): the horizontal depth of each step. A sensible range is about 10 in to 12 in (default 11 in). Too small feels cramped and may trigger a code warning. Too large uses more floor space.
- **Nosing**: how far the front edge of the tread overhangs the riser. Default 1 in. Shown only for Plank.
- **Tread T**: the tread thickness, 0.5 in to 2 in, default 1 in. Shown only for Plank.

## DIMENSIONS > MONOLITHIC

- **Slab T**: the thickness of the monolithic slab, measured down from the nosing line.
- **Tilt**: shown only when Type = Monolithic and Egress is on. The backward lean of each riser from vertical (0 to 45 degrees), for the common cast-in-place detail of a slightly slanted riser with a level tread.

## DIMENSIONS > RAMP

- **Ramp T**: the thickness of the ramp slab. It is separate from Slab T.

## DIMENSIONS > LANDING (collapsible)

The heading is a switch.

- **On**: follows the code limit on steps in one flight, and inserts a landing when it is exceeded. When open:
  - **Even Tread Split** (formerly Even): spreads the steps as evenly as possible between landings. When off, each flight climbs as far as the code allows before a landing is added, which can leave very few steps in the last flight and look uneven.
  - **At Height**: the heights at which landings are forced, separated by commas (for example `48, 110`). **It works only when Style is Straight, or L in single-storey mode.** For other styles or multi-storey mode, what you type here is ignored, without any error or message.
- **Off**: the stair climbs all the way up with no landings and ignores the code limit. Turn it off only if you do not need code compliance.

## DIMENSIONS > STRINGER (collapsible)

The heading is a switch for solid sloping stringers (the structural beams under the steps) on both sides. With Type = Ramp it is forced off and cannot be turned on. It can be turned on again after switching to Plank or Monolithic. When open:

- **Stringer T**: the stringer thickness, 0.1 in to 4 in.
- **Stringer D**: how deep the stringer reaches below the nosing line, 9 in to 24 in, default 12 in. In Monolithic mode it is capped automatically (never more than one riser plus Slab T), so the stringer does not poke through the underside of the slab.

## DIMENSIONS > RAILING PROFILE (collapsible)

The heading is a switch that additionally outputs a pair of guide curves (inner and outer) following the stair, so you can place rails by hand or use `zRailing` to build real railings along them. These are **curves**, not solid rails. **With Stringer on, this switch can still be turned on**, and only Rail Inset below is grayed out, so you can turn it on now and it takes effect when you turn Stringer off later. When open:

- **Rail Inset**: how far the rail guide is pulled in from the step edge. It can be adjusted only while Stringer is off. If Stringer is on, it is disabled because the rail guide then follows the center of the stringer.

## How the settings work together

- **Height, Riser and the final number of steps work as one system.** Riser is only a wish. The number of steps is the total height divided by it and rounded, and the real riser is the total height divided by that number. In multi-storey mode each storey can have a different height, so the real riser can differ slightly between storeys. This is normal.
- **Story = Multiple changes the available styles and the layout.** Style narrows to U and C (L, Straight and Z cannot be stacked), and if you had chosen another style it changes to U. The Height row becomes the Typical / Ground / Basement table.
- **Type shows or hides a whole set of rows.** Plank shows Tread T, Riser T and Nosing. Monolithic shows Slab T (and Tilt if Egress is on). Ramp shows Ramp T and forces Stringer off, and switching back turns it on again.
- **With Landing off, Even and At Height are disabled (grayed).** They only describe how landings are inserted.
- **Stringer and Railing Profile:** the RAILING PROFILE switch itself is never forced off by Stringer. When Stringer is on, only Rail Inset is grayed out, because the rail guide follows the center of the stringer.
- **Code checks only warn, they never stop you.** If Riser or Going is outside the built-in building code range (IBC 2018), a yellow note appears in the viewport HUD, but you can still bake. Whether to follow the code is your decision.

## Good to know

- **The region must be a closed rectangular polyline, not an arc or a free curve.** The command stops with an error unless the curve is closed, is a polyline made only of straight segments (any arc edge fails this), has at least 4 vertices, and all vertices are coplanar. A rectangle with arc fillets cannot be used directly.
- **The start-edge step can be skipped.** If you press Esc at that prompt, the first edge of the region is used silently. If the stair faces the wrong way, run the command again and click the correct edge.
- **A region that is too small or too narrow produces nothing.** If Going or Width is too large for the region (for example the region is narrower than the stair width), or the region is degenerate (close to zero area), a warning appears in the viewport, and the preview and the bake are empty. There is no crash, but nothing is built. Check that the region size matches your Width and Going.
- **The red "outside the region" box ignores the nosing.** Only the real outline of treads, risers, landings and stringers is tested, not the part where the nosing overhangs. A nosing that only slightly pokes out does not trigger the warning.
- **At Height works only for some styles.** See LANDING above. If a custom landing does not appear at the height you gave, first check whether the current Style and Story support it.
- **Exploding part of a baked stair means that part can never be edited again.** A baked stair is a group of several solids (treads, risers, stringers and so on). If you explode one, the fragments lose the stored identity, and `zStairByRegionEdit` refuses them with a message that they are not a zStairByRegion object. If other parts of the group are still intact, you can select those and keep editing the whole stair, but the exploded fragments are not cleaned up, and the new stair will sit next to them until you delete them by hand.
- **Deleting the hidden outline reference makes a moved stair jump back to its original place.** A baked stair leaves an invisible reference at the original region, which remembers whether you moved the stair. If it is deleted, any later edit (run `zStairByRegionEdit` and bake again) builds the stair at the original position instead of where you moved it, with no message.
- **A manual deformation of a single part (a Gumball stretch, a CageEdit) is thrown away when you run Edit.** Edit rebuilds the whole stair from the original settings and can only recognize a rigid move or rotation of the whole stair, not a local stretch. The old parts, with your change, are deleted and replaced, with no warning.
- **Copying and pasting a baked stair is safe.** The tool recognizes a copy as independent, and editing one does not affect the other.
- **If a stored setting (user text) of a stair is deleted or changed, Edit silently replaces it with the default** and rebuilds. The result may differ from what you set, with no message telling you which setting went back to default.
