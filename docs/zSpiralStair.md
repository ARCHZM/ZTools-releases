# zSpiralStair

`zSpiralStair` builds a stair that spirals around a center axis. You pick a straight edge as the radius reference, and the tool works out a suitable spiral radius and builds the whole stair.

Good for:

- Tight spaces where you need a lot of climb in a small footprint.
- Decorative stairs or stairs for industrial equipment.
- Single-storey or continuous multi-storey spiral stairs.

Not for:

- An ordinary rectangular stair hall: use `zStairByRegion` or `zStairByCrv`.

## Quick start

1. Run `zSpiralStair`.
2. Pick a **straight edge** as the radius reference (see Good to know).
3. Click a point to decide which side of the edge the stair is on.
4. The dialog opens with a live preview. Adjust the groups below.
5. **Bake** creates the real geometry. **Cancel** discards the preview.
6. To change a baked stair, select it and run `zSpiralStairEdit` (see Good to know for the limits).

## LAYOUT

- **Format**: the display unit for every length field. Only the display changes. Values are stored in model units.
- **Story** (Single / Multiple): one storey or several continuous storeys.
  - **Single**: one stair climbing to the Height you set.
  - **Multiple**: continuous stairs for several storeys. The Height row becomes a table of **Typical / Ground / Basement** rows (floor-to-floor height and count).
- **Type**: how the stair is built.
  - **Plank**: separate horizontal treads and vertical risers. Shows **Tread T** and **Nosing**.
  - **Monolithic**: one solid stepped block, like a cast-in-place concrete stair. Shows **Slab T**. Nosing is ignored in this mode, with a note.
  - **Ramp**: each flight is one continuous spiral slab with no steps. Shows **Ramp T**, and **Stringer is forced off** even if you had turned it on.
- **Clockwise**: whether the stair climbs clockwise or counter-clockwise. The geometry is always built counter-clockwise first and then mirrored when this is ticked. It changes no dimensions, it is only a direction choice.
- **Trim Platform**: controls how the opening edge of the landing platform is built, trimming away the part of the platform that goes beyond a sensible radial range, so the edge fits the real usable space.

## DIMENSIONS > GENERAL

- **Height**: the total height to climb in Single mode, about 120 in to 240 in (default 180 in).
- **Width**: the radial width of the stair, from the inner to the outer circle. A very small value can make the stringer and rail inset look cramped.

## DIMENSIONS > RISER

- **Riser** (target riser height): the step height you want. The tool works out the real number of steps and the real riser from it. A sensible range is about 6.5 in to 7.5 in.
- **Riser T**: the thickness of the riser board. It also affects the angle between steps. Unlike the other two stair tools, Riser T stays editable even in Monolithic mode, because it really takes part in the angular spacing of the spiral.

## DIMENSIONS > TREAD

- **Going** (formerly Tread D): the horizontal depth of each step, measured at the inner circle. **This is the only input that drives the final radius.** There is no separate radius setting. The tool solves for a radius that gives your Going. If no radius can reach the Going you asked for with the current number of steps, the radius is pinned at its lower limit and a note is shown (it does not stop with an error).
- **Nosing**: how far the front edge of the tread overhangs the riser. Default 1 in. Ignored in Monolithic mode.
- **Tread T**: the tread thickness, 0.5 in to 2 in, default 1 in.

## DIMENSIONS > MONOLITHIC

- **Slab T**: the thickness of the stepped block, measured down from the nosing line. It has a dynamic lower limit: when the riser height is small, the smallest allowed Slab T rises automatically so the slab does not cut into the neighboring steps. The default is 6 in, the same as the other stair tools.

## DIMENSIONS > RAMP

- **Ramp T**: the thickness of the ramp slab.

## DIMENSIONS > LANDING (collapsible)

The heading is a switch that controls whether landings are made when the code limit on steps per flight is reached. When open:

- **Even Tread Split** (formerly Even): spreads the steps as evenly as possible between landings. When off, each flight climbs as far as the limit allows before a landing is added.
- **At Height**: the cumulative heights at which landings are forced, separated by commas. Each height must fall strictly inside a single storey, otherwise the whole build is stopped with a warning.

## DIMENSIONS > STRINGER (collapsible)

The heading is a switch for solid stringers on the inner and outer sides. With Type = Ramp it is forced off and cannot be turned on. When open:

- **Stringer T**: the stringer thickness, 0.01 in to 4 in. It also shifts the real radius of the rail position (with a stringer, the rail follows the stringer).
- **Stringer D**: the structural depth of the stringer, measured down from the nosing line, 9 in to 24 in, default 12 in. A spiral stringer lies outside the inner and outer radius and does not overlap the stepped block in plan, so unlike the other two stair tools it needs no cap. Any depth is safe.

## DIMENSIONS > RAILING PROFILE (collapsible)

The heading is a switch that additionally outputs an inner and outer guide curve following the stair. When open:

- **Rail Inset**: how far the rail guide is pulled in from the inner and outer edge. It can be adjusted only while Stringer is off. If Stringer is on, it is disabled because the rail follows the stringer.

## How the settings work together

- **Riser T stays editable in Monolithic mode.** The other two stair tools hide it there because it is only a decorative riser board thickness, but here it takes part in the angular spacing, so it applies for every Type.
- **Going is the only input that drives the radius.** To get a larger or smaller spiral, change Going, Riser or Width, not a radius (there is none). If no radius can satisfy the Going, the tool pins the radius at its minimum and shows a note, and the real tread depth is then smaller than you set.
- **Type = Ramp forces Stringer off.** Switching back to Plank or Monolithic lets you turn it on again. A ramp also has no discrete steps.
- **With Landing off, Even and At Height are disabled (grayed).**
- **With Stringer on, Rail Inset is disabled**, because the rail follows the stringer.
- **Slab T has a dynamic lower limit** that follows the riser height. Changing Riser or Height can change the smallest allowed Slab T.
- **Code checks only warn, they never stop you.** Values outside the built-in code range only produce a note in the viewport HUD, and you can still bake.

## Good to know

- **The picked edge must be straight.** If you pick an arc or a free curve as the radius reference, the tool refuses with the message `pick a straight (linear) edge`.
- **The platform width must be smaller than the solved radius.** If Platform W is too large for the current geometry (equal to or larger than the inner radius), the tool stops with an error that gives the radius and platform width, so you can reduce the platform width or change other settings.
- **A failed radius solve does not stop the tool, it pins the radius at its limit and shows a note.** When Going, Width and the other requirements cannot all be met, the real tread depth differs from your Going. If the steps look narrower than expected, read the note in the viewport.
- **A custom landing height (At Height) must be strictly inside a single storey, otherwise the build stops.** Unlike `zStairByRegion`, which ignores invalid settings silently, this tool stops. If no stair appears at all, first check whether an At Height value conflicts with a storey boundary.
- **A picked edge with zero length is an error.** It usually means the wrong object was picked. Pick a valid edge again.
- **Exploding part of a baked stair means that part can never be edited again.** The fragments lose the stair identity, and the edit command does not recognize them or clean them up.
- **Deleting the hidden reference line makes a moved stair jump back to its original place.** If the reference line is deleted or edited so it is no longer a straight line, the next edit builds the stair at the original position, with no message.
- **A manual deformation of a single part is thrown away when you run Edit.** Edit deletes and rebuilds the whole stair, it does not patch parts.
- **Copying and pasting a baked stair is safe.** The tool recognizes a copy as independent.
- **If a stored setting is deleted or changed, Edit silently replaces it with the default.** The missing setting is rebuilt with its default, with no message telling you which one changed.
- **Repeated climbing segments are baked as Rhino blocks.** In Plank construction, the treads, risers and nosings of identical climbing segments (typically every storey) share one block definition, which keeps the file small. Platforms, stringers and railing are ordinary objects. Monolithic and Ramp are not instanced. Use Edit as usual to change the stair. Do not use Block Edit on these blocks.
