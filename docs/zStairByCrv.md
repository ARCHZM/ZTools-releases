# zStairByCrv

`zStairByCrv` builds a stair that climbs along a curve you have already drawn. It suits stairs whose path is not a simple rectangular region and must follow a specific line.

Good for:

- A stair path that is a free curve or a polyline, not a rectangular region.
- A stair that must follow an existing building outline or ramp path exactly.
- Single-storey or continuous multi-storey stairs.

Not for:

- A known rectangular or regular region where you want the tool to plan the layout: use `zStairByRegion`.
- Spiral or winding stairs: use `zSpiralStair`.

## Quick start

1. Run `zStairByCrv`.
2. Pick an **open (not closed), horizontal curve** as the stair path (see Good to know).
3. Click a point to decide which side of the curve the stair grows to.
4. The dialog opens with a live preview. Adjust the groups below.
5. **Bake** creates the real geometry. **Cancel** discards the preview.
6. To change a baked stair, select it and run `zStairByCrvEdit` (see Good to know for the limits).

## LAYOUT

- **Format**: the display unit for every length field. Only the display changes. Values are stored in model units.
- **Align**: whether the picked curve is the left edge, the center line or the right edge of the stair.
  - **Left**: the curve is the left edge, and the width grows to the right.
  - **Center**: the curve is the center line, and the width grows equally to both sides.
  - **Right**: the curve is the right edge, and the width grows to the left.
- **Story** (Single / Multiple): one storey or several continuous storeys.
  - **Single**: one flight climbing to the Height you set.
  - **Multiple**: stairs for several storeys in a row. The Height row is replaced by a table of **Typical / Ground / Basement** rows, each with a floor-to-floor height and a count.
- **Type**: how the stair is built.
  - **Plank**: separate horizontal treads and vertical risers. DIMENSIONS then also shows TREAD **Thickness**, RISER **Thickness** and **Nosing**.
  - **Monolithic**: one solid sloping slab, like a cast-in-place concrete stair. DIMENSIONS then shows **Slab Thickness**.
  - **Ramp**: no steps, just one continuous sloping ramp. DIMENSIONS then shows **Ramp Thickness**, and the RISER **Height** and RISER **Thickness** rows are hidden because a ramp has no discrete steps.
- **Reverse Path Direction** (formerly Flip): puts the stair on the other side of the path curve, so you do not have to pick again.
- **Retain Path Curvature** (formerly True Arc): when the path has arc segments, whether the tread, riser and landing edges are built from true arcs.
  - On: the edges at a bend are smooth true arcs.
  - Off: arcs are approximated by line segments.
- **Trim Underground Geo** (formerly Trim Ground): cuts away the part of the stringers or slab that is below the stair's own ground level, so the underside sits on the ground. With it off you may see extra solid hanging under the stair.
- **Rise Mode**: decides which one gives way, the path length or the **Going** (tread depth).
  - **Fixed Going** (default): Going is the value you type and is never adjusted. The path is only a direction. The number of steps comes from Height and Riser, and the steps are laid along the path at the fixed Going. If the path is longer than needed, the extra length is simply not used (no error, no message). If the path is too short, the stair continues in a straight line from the end of the last used path segment, along its tangent, until all steps are placed.
  - **Fit Curve**: Going becomes a read-only derived value (usable path length / number of steps), and the real length of the path is followed exactly, with no early stop and no extension. This is the original behavior and is kept as an option.

## DIMENSIONS > GENERAL

- **Height**: the total height to climb in Single mode. A larger value gives more steps and a longer stair. A very small value may give only one or two steps.
- **Width**: the stair width, constant along the whole stair. About 36 in is common for houses and 48 in or more for public buildings.

## DIMENSIONS > RISER

- RISER **Height** (target riser height): the step height you want. The tool uses it to work out how many steps are needed. It is a target, not an exact final value. The real height of each step is adjusted slightly by rounding, usually by a small amount. A sensible range for houses is about 6.5 in to 7.5 in. The row is hidden for Ramp.
- RISER **Thickness**: the thickness of the riser board. Shown only for Plank.

## DIMENSIONS > TREAD

- **Going** (formerly Tread D): the horizontal depth of each step, measured along the path. It depends on Rise Mode. In Fixed Going it is the value you type and it applies as is. In Fit Curve it is grayed out and shows the derived value. A sensible range is about 10 in to 12 in (default 11 in).
- **Nosing**: how far the front edge of the tread overhangs the riser. Default 1 in. Shown only for Plank.
- TREAD **Thickness**: the tread thickness, from 0.5 in to 2 in, default 1 in. Shown only for Plank.

## DIMENSIONS > MONOLITHIC

- **Slab Thickness**: the thickness of the monolithic slab, measured down from the nosing line. It is also the thickness of landings.

## DIMENSIONS > RAMP

- **Ramp Thickness**: the thickness of the ramp slab. It is separate from Slab Thickness.

## DIMENSIONS > LANDING (collapsible)

The heading is a switch that controls whether landings are made at corners or when the code limit on steps per flight is reached. When open:

- **Pick / Unpick**: click a segment of the path in the viewport to force a landing there, or remove a picked point. Available only when the path has more than one segment.
- **At Height**: the cumulative heights at which landings are forced, separated by commas. Each height must fall strictly inside a single flight, otherwise the tool refuses to build and shows a warning instead of ignoring it silently.
- **Even Tread Split** (formerly Even): spreads the steps as evenly as possible between landings. When off, each flight climbs as far as the code allows before a landing is added.

## DIMENSIONS > STRINGER (collapsible)

The heading is a switch for solid sloping stringers on both sides of the stair. With Type = Ramp this switch is forced off and cannot be turned on. When open:

- STRINGER **Thickness**: the stringer thickness, 0.1 in to 4 in.
- STRINGER **Depth**: how deep the stringer reaches below the nosing line, 9 in to 24 in, default 12 in. In Monolithic mode it is capped automatically (never more than one riser plus Slab Thickness), so the stringer does not poke through the underside of the slab.

## DIMENSIONS > RAILING PROFILE (collapsible)

The heading is a switch that additionally outputs a pair of guide curves following the stair. When open:

- RAILING PROFILE **Inset**: how far the rail guide is pulled in from the step edge. It can be adjusted only while Stringer is off. If Stringer is on, it is disabled.

## How the settings work together

- **Rise Mode decides who follows whom.** In Fixed Going, Going is your fixed value and the path is only a direction (a long path is cut short, a short path is extended). In Fit Curve, Going is a read-only derived value and the path length is followed exactly. In both modes Riser is only a target, and the number of steps always comes from Height and Riser.
- **In Fit Curve, if the derived Going falls below the code minimum, the tool refuses to build.** The preview goes empty, the HUD shows a red error, and closing the dialog bakes nothing. Lengthen the path, reduce the number of steps, or switch to Fixed Going.
- **Story = Multiple replaces the Height row** with the Typical / Ground / Basement table.
- **Type shows or hides a whole set of rows.** Plank shows TREAD Thickness, RISER Thickness and Nosing. Monolithic shows only Slab Thickness. Ramp shows only Ramp Thickness, hides Riser and RISER Thickness, and forces Stringer off. Switching back turns Stringer on again.
- **With Landing off, Pick / Unpick, Even and At Height are all disabled (grayed).** They only describe how landings are inserted.
- **Stringer and Railing Profile:** when Stringer is on, RAILING PROFILE Inset is disabled, because the rail guide then follows the center of the stringer. RAILING PROFILE Inset can be set only when Stringer is off.
- **Code checks only warn, they never stop you.** If Riser or Going is outside the built-in building code range, a yellow note appears in the viewport HUD, but you can still bake.

## Good to know

- **The picked curve must be open (not closed) and horizontal.** A closed curve, or a curve with clear ups and downs (not a flat 2D curve), is rejected and the command is cancelled. A curve that is too short is rejected too. A self-intersecting curve only gives a warning, but steps may overlap at the crossing.
- **There is no radius input.** All sizes come from the real length of the curve, Width and Going. If the proportions are not what you expected, check Going first. (This applies to Fit Curve. In Fixed Going, Going is the value you typed.)
- **Any warning stops Bake.** A path that is too short, corners or landings that use the whole path, or in Fit Curve a Going below the code minimum, all give a red warning in the HUD and an empty preview, and closing the dialog bakes nothing. This is a safeguard, not a bug.
- **A custom landing height (At Height) must be strictly inside a single flight, or the build is refused.** Unlike `zStairByRegion`, which ignores invalid settings silently, this tool stops and warns. If the stair does not appear at all, check whether an At Height value sits exactly on a corner or a landing edge.
- **Exploding part of a baked stair means that part can never be edited again.** Exploded fragments lose the identity information, so `zStairByCrvEdit` refuses them. If other parts of the group are still intact, you can select those and keep editing the whole stair, but the exploded fragments are not cleaned up, and you have to delete them by hand.
- **Deleting the hidden path reference curve makes a moved stair jump back to its original place.** A baked stair carries an invisible reference that remembers whether you moved it. If it is deleted, or edited so it is no longer a straight line, the next edit builds the stair at the original position without any message.
- **A manual deformation of a single part is thrown away when you run Edit.** Edit rebuilds the whole stair from the original settings and can only recognize a rigid move or rotation of the whole stair. A part you stretched or scaled is replaced without any warning.
- **Copying and pasting a baked stair is safe.** The tool recognizes a copy as independent, and editing one does not affect the other.
- **If a stored setting is deleted or damaged, Edit silently replaces it with the default** and rebuilds. The result may differ from what you set, with no message telling you which setting went back to default.
- **Repeated flights are baked as Rhino blocks.** In Plank construction, the treads, risers and nosings of flights that are identical (for example every middle storey of a Multiple stair) share one block definition, which keeps the file small. Landings, stringers and the railing guide are ordinary objects. Monolithic and Ramp are not instanced. To change one flight, select it and use Edit as usual. Do not use Block Edit on these blocks, because every identical flight changes with it and Edit rebuilds them anyway.
