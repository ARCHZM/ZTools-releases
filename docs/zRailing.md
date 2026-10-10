# zRailing

`zRailing` builds a railing along a curve. It supports five infill systems, **baluster, cable, glass, metal panel and industrial guardrail**, combined with a top rail, intermediate rail, bottom rail and an accessible handrail.

Good for:

- Stairs, balconies, terraces and atria that need a continuous railing along a path.
- Combining several kinds of infill (balusters, glass, cables) on one curve.
- Paths that are flat or that rise and fall in 3D (for example along a stair).

Not for:

- Posts that grow out of a vertical wall or side face instead of up from a path on the floor: use `zRailingSide`.

## Quick start

1. Run `zRailing`.
2. Pick one or more curves as the railing path (see Good to know).
3. The dialog opens with a live preview. Choose the **System** first, then adjust the groups below.
4. **Bake** creates the real geometry. **Cancel** discards the preview.
5. To change a baked railing, select it and run `zRailingEdit` (see Good to know for the limits).

## GENERAL

- **Format**: the display unit for every length field. Only the display changes.
- **System**: the main infill. It also switches the content of DIVISION and GUARDRAIL and decides whether RAIL is shown.
  - **Guardrail**: an independent pipe-guardrail unit system, usually for industrial or equipment use. DIVISION is replaced by GUARDRAIL, and the whole RAIL group is hidden, because a guardrail is a separate system that is not mixed with ordinary rail parts.
  - **Baluster**: traditional vertical balusters. Top Rail and Bottom Rail are forced on and cannot be turned off.
  - **Cable** (labeled Horizontal in the dialog): horizontal cables.
  - **Glass**: glass panels, either framed with posts, or a frameless glass wall with posts off.
  - **Metal**: metal panels, again either framed or frameless.
- **Height**: the total railing height, from the path curve to the top of the top rail. The usual range is 36 in to 42 in (common code heights for houses and public buildings).
- **Offset**: the horizontal offset of the whole railing from the path curve. Together with Flip it decides which side.
- **Flip In/Out**: the direction the railing is offset to (used together with Offset).
- **Linearize Path**: whether the input curve is straightened into line segments between the post positions, instead of following every small bend of the curve.

## DIVISION > POST

(Not shown when System is Guardrail. It is replaced by the GUARDRAIL group.)

- **Post** (switch): whether solid posts are made. It can be switched by hand only when System is Glass or Metal (to choose framed or frameless). For other systems it is forced on and cannot be turned off.
- **Section**: the post cross-section (round, rectangular or double bar). Its size sets how thick the posts look.
- **Corner**: how posts are handled at corners of the path.
  - **Single**: one shared post at the corner.
  - **Split**: no shared post. One post is set back about 10 in on each side. For frameless glass or metal (posts off) this has no meaning and is forced to Single and locked.
- **Distribution**: how posts are placed along the path.
  - **Even**: posts are spread evenly along the whole path with a spacing no larger than **Spacing**.
  - **Fixed**: posts use the fixed **Spacing** value, and the remainder is collected at the end of the path.
- **Spacing**: the maximum or fixed post spacing.
- **Base Plate**: whether a base plate is made under each post.
- **Fitting** (post-top fitting): available only when the Intermediate Rail is on. Makes a saddle fitting at the post top that connects the intermediate rail to the top rail.

## DIVISION > INFILL

(The content depends on the System.)

**System = Baluster**

- **Section**: the cross-section of each vertical baluster.
- **Gap**: the largest clear gap between neighboring balusters. Codes commonly require no more than 4 in so a child's head cannot pass through (the "4 inch sphere rule").

**System = Cable**

- **Section**: the cable cross-section (diameter).
- **Mode**: whether the number below controls the spacing or the count.
  - **Spacing**: the number of cables is worked out from the largest clear gap you set.
  - **Count**: you set a fixed number of cables.
- **Gap or Count**: shows the one that matches Mode: the largest clear gap between cables, or the total number of cables.
- **Top mgn / Bottom Margin**: how far the cable zone is pulled in from the top and the bottom.

**System = Glass**

- **Thickness**: the glass panel thickness.
- **Gap**: the clearance between the glass edge and the post face.
- **Clearance** (clearance above the floor): can be adjusted only for framed glass (posts on) with Bottom Rail off. It sets how high the bottom edge of the glass is above the floor. For frameless glass (posts off), Bottom Rail is forced off, and Clear is forced to zero and locked.

**System = Metal**

- **Gap / Clear**: the same meaning as for Glass: the edge gap and the clearance above the floor (again adjustable only when framed and Bottom Rail is off).
- **Inset Top/Bottom**: how far the metal panel is pulled in from the top and the bottom (applied to both).
- **Inset Left/Right**: how far the metal panel is pulled in from the left and the right.

## GUARDRAIL

(Shown only when System = Guardrail. It replaces DIVISION.)

- **Type**: segmented or two-tier.
  - **Segmented**: the guardrail is made of separate units.
  - **Two-Tier**: the guardrail has an upper and a lower rail, and **Lower Height** (lower rail height) appears.
- **Distribution / Spacing**: the same meaning as in DIVISION > POST: how the guardrail units or posts are distributed along the path, and their spacing.
- **Lower Height**: shown only for Two-Tier. The height of the lower rail.
- **Mode** (Segmented only): like Mode for cables: work out from a spacing, or set a fixed count. Gap or Count is shown to match.
- **Pipe Dia**: the diameter of the pipe parts of the guardrail.
- **Base Plate**: whether a base plate is made under the guardrail posts or units.

## RAIL

(The whole group is hidden when System = Guardrail.)

### TOP RAIL

The heading is a switch for the top rail. With System = Baluster it is forced on and cannot be turned off. When open:

- **Section**: the top rail cross-section.

### INTERMEDIATE RAIL

Available only when Top Rail is on. The heading is a switch for the intermediate rail. When open:

- **Section**: the intermediate rail cross-section.
- **Drop**: how far the intermediate rail is below the top rail.

### BOTTOM RAIL

The heading is a switch for the bottom rail. With System = Baluster it is forced on and cannot be turned off. In frameless glass or metal wall mode it is forced off and cannot be turned on. When open:

- **Section**: the bottom rail cross-section.
- **Clearance** (clearance above the floor): the height of the underside of the bottom rail above the floor.

### HANDRAIL

The heading is a switch for a separate accessible handrail (different from the top rail itself). When open:

- **Section**: the handrail cross-section (usually slimmer than the other parts).
- **Connection**: how the handrail connects to the posts or the wall (struts or brackets, and the labels change with the handrail shape).
- **Height**: the handrail height above the floor, usually 34 in to 38 in (the common accessibility range).
- **Lateral**: the horizontal offset of the handrail from the main body of the railing.

**ADA EXTENSION**: the heading is a switch for whether the handrail ends extend into the horizontal sections that accessibility rules ask for. This heading cannot be collapsed. With the switch off the values below are grayed out but stay visible. They are:

- **Return**: how the end of the extension finishes: down, to the wall, or a loop back.
- ADA EXTENSION **Length**: the length of the extension at each end.
- **Corner Radius**: the corner radius at the bend of the extension.

## How the settings work together

- **Changing System rebuilds several groups.** It resets the infill defaults (for example it forces Top Rail and Bottom Rail on for the non-guardrail systems), rebuilds the Infill group, swaps DIVISION and GUARDRAIL, and refreshes which controls are available. After you change System, check every group again, and do not assume the earlier settings still apply.
- **Baluster locks Top Rail and Bottom Rail on.**
- **Turning Top Rail off disables Intermediate Rail.** The intermediate rail can be enabled only while the top rail is on.
- **Framed and frameless Glass or Metal change a chain of settings.** Only these two systems let you switch Post by hand. With posts off (frameless), Bottom Rail is forced off, Corner is forced to Single and Clear is forced to zero. These follow from the frameless glass or metal wall construction itself and are not bugs.
- **Post Fitting depends on the Intermediate Rail.**
- **Guardrail and the ordinary railing parts are mutually exclusive.** With Guardrail, DIVISION is replaced by GUARDRAIL and the RAIL group is hidden. A guardrail has its own parameters and is not mixed with the top and bottom rails of a normal railing.

## Good to know

- **Very short or invalid curves are skipped silently and do not stop the rest.** If you pick several curves and some are too short or invalid, they are skipped and the others are processed. If all curves are skipped, the command is cancelled with a message that no usable curve was picked.
- **Sharp hairpin turns or a self-crossing path may leave a gap.** If a very sharp turn or a self-crossing makes the offset path cross itself, that stretch is skipped with a warning and the rest is built normally. You may see the railing "break" at a sharp corner. Check whether the path curvature there is extreme.
- **An infill gap above the 4 inch sphere rule only warns, it does not stop Bake.** The viewport tells you which stretch is over the limit, and it is your decision whether to fix it.
- **Three-dimensional paths are supported, with no extra planarity check.** A path that rises and falls (for example along a stair) is followed correctly. This is a normal, supported case.
- **Exploding part of a baked railing means that part can never be edited again.** A baked railing is a group of many solids, many of them block instances. If you explode one, the fragments lose the railing identity, and the edit command does not recognize them. The other parts of the group can still be edited as a whole, but the exploded fragments are not cleaned up and must be deleted by hand.
- **Deleting the hidden path reference makes a moved railing jump back to its original place.** A baked railing keeps a copy of the original path as an anchor that remembers whether you moved the whole railing. If it is deleted, any later edit builds the railing at the original position, with no message.
- **A manual change to a single part is thrown away when you run Edit.** Edit rebuilds the whole railing from the original settings and can only recognize a move or rotation of the whole railing, not a manual change of one part. It is replaced without a warning.
- **If a stored setting is deleted or damaged, Edit silently replaces it with the default** and rebuilds. The result may differ from what you set, with no message telling you which setting went back to default.
