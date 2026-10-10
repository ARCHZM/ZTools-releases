# zCurtainWall

`zCurtainWall` builds a complete curtain wall system along one or more path curves: mullions, transoms, glass, silicone joints and cover caps. It handles corners along the path, and can split the wall by **storey**, with spandrel glass at the floor slabs.

Good for:

- A curtain wall along a plan path, including polyline corners and closed outlines, with mullions, transoms and glass panels.
- A single-storey or multi-storey wall (Story = Multiple) where transoms, and even spandrel glass, should appear automatically at the floor boundaries.
- Concept-stage models that show silicone joints and cover caps.

Not for:

- Filling a closed boundary region. The tool only picks path curves. A closed curve is treated as a path whose first and last point meet at a true corner.

## Quick start

1. Run `zCurtainWall` and pick one or more path curves (see Good to know).
2. The dialog opens with a live preview. The left column has **LAYOUT** and **DIVISION**. The right column has **DIMENSIONS**, which contains PANEL, MULLION and TRANSOM sections.
3. In **LAYOUT**, choose **Story**: Single (type a Height) or Multiple (use the Ground / Typical floor table and a Slab Thickness).
4. In **DIVISION**, set MULLION **Division** (Even or Fixed) and MULLION **Spacing** for the mullion spacing. In the TRANSOM section, add custom transom rows if you want them.
5. In **DIMENSIONS**, set the glass thickness (PANEL) and the width and depth of the mullions and transoms, with their Silicone and Cap switches.
6. If the path curves you picked still exist in the document, you can drag their control points in the viewport and the preview follows.
7. **Bake** creates the real geometry (including a hidden anchor curve of the path, so that `zCurtainWallEdit` can detect if you moved the wall). **Cancel** discards the preview.
8. To change a baked curtain wall, select any part of it and run `zCurtainWallEdit`. It rebuilds the path and every setting from what was stored at bake time and reopens the same dialog. Bake deletes the old parts and builds new ones. The rebuilt path is not a real viewport object, so you cannot drag its control points, and it works only while the picked part still carries its identity mark (it has not been exploded).

## LAYOUT

- **Format**: the display unit for lengths (mm or inch). Values are converted to model units on bake.
- **Story** (Single / Multiple): how many storeys the wall covers.
  - **Single**: a **Height** field gives the total wall height.
  - **Multiple**: shows a Typical row and a Ground row (each with **FFH**, the floor-to-floor height, and **Count**, the number of repeats) plus a **Slab Thickness** field. Ground is the lowest storey (Count is normally 1, and Z = 0 starts at its floor). Typical is the standard storey stacked above Ground. The total height is the sum of all storeys, so you no longer type a Height. A Typical Count of 0 is allowed and means only the Ground storey.
- **Slab Thickness** (Multiple only, default 24 in / 609.6 mm): when above 0, every storey boundary (including the one just below the top edge) gets an extra forced transom at Slab Thickness below the boundary transom, representing the underside of the floor slab. The glass between the two transoms is classed as **spandrel** glass, on its own layer and color. Normal vision glass is not affected. Set it to 0 to skip spandrel glass completely.
- **Flip In/Out**: the path you pick is the centerline of the glass itself. By default the mullions are on the interior side and the caps on the exterior side. Tick this to swap the two sides. It only affects the depth direction, not widths or heights.
- **Linearize Path**: on by default. At the soft polyline nodes that the tool creates when it breaks up a smooth curve, transoms, caps, silicone joints and glass or spandrel panels are cut as straight rectangles, one per bay. When off, these parts follow the real bend at every internal node with zero-width miters, so the transoms and glass hug the curve continuously. Mullions, mullion caps and mullion silicone always stay straight rectangles, whatever this setting is.

## DIVISION

Two collapsible headings, **MULLION** and **TRANSOM**. They collapse only and have no on/off switch.

### MULLION

- MULLION **Division** (Even / Fixed): how the width of each bay is decided.
  - **Even**: fits a whole number of bays and shares the remainder equally among them.
  - **Fixed**: every bay is exactly MULLION Spacing, and the last bay may be shorter.
- MULLION **Spacing**: the target mullion spacing. In Even mode, an `-> actual: X` hint under it shows the real spacing.

### TRANSOM

The custom division table is the only way to divide vertically. There is no Count or Spacing mode.

- **Single story**: one flat table. Each row has a **Height** (measured from the bottom of the wall, that is the Z of the input curve) and a **Pattern**: a comma-separated string like `0,1,0`, in the style of Grasshopper, applied around the bays. Empty means every bay is enabled. `0` means no transom in that bay (the glass continues through), any other number means enabled. Click **+ Add Row** to add a row. Each row has a drag handle on the left for ordering and a delete button on the right.
- **Multiple stories**: one combined Ground + Typical table. Each row has an extra **Scope** column (a G / T switch) to the left of Height. Height here is measured from the bottom of the storey the row belongs to, not from the bottom of the whole wall. A row with Scope = G applies once, to the Ground storey. A row with Scope = T is repeated on every Typical storey above Ground, from the bottom of each storey. If a row falls close to a storey boundary (or the spandrel transom under a slab), the storey boundary or slab row wins, and a custom row that is too close is merged or ignored.

## DIMENSIONS

One group box with three sections in this order: PANEL, MULLION, TRANSOM.

### PANEL

- PANEL **Thickness** (default 12 mm / 0.5 in): the thickness of the glass panel, offset inward from the frame surface.

### MULLION (the heading is a switch for the vertical members, on by default)

- **Width / Depth** (default 60 mm x 101.6 mm / 2.4 in x 4 in): the section of a mullion, width along the wall and depth along the wall normal.
- **Cap** (a switch in its subheading, on by default, available only while MULLION is on): **Width / Depth** (default: the mullion width by 20 mm / 0.8 in). The cap sits against the outer glass surface (which sits against the outer mullion surface) and runs the full height without a break.
- **Silicone** (a top-level switch independent of the MULLION switch, on by default): **Width** (default 10 mm / 0.4 in). The silicone joint is centered on each internal mullion position and shares the depth band of the glass panel.

### TRANSOM (the heading is a switch for the horizontal members, on by default, with the same structure as MULLION)

- **Width / Depth** (default the same as the mullion), **Cap** (default the transom width), **Silicone** (default 10 mm / 0.4 in, an independent switch). They mean the same as the matching MULLION items but apply to the horizontal parts.

## How the settings work together

- **Cap depends on its member switch.** MULLION > Cap can be turned on only while MULLION is on, and its Width and Depth rows are shown only then. TRANSOM > Cap likewise depends on TRANSOM. Turning a member switch off disables Cap and hides its rows.
- **Silicone is independent of the member switch.** Mullion silicone and transom silicone can stay on or off even if the mullion or transom itself is off.
- **Story drives the Height field and the floor table.** Single shows only Height. Multiple hides Height, derives it from FFH x Count of Ground and Typical, and adds Slab Thickness.
- **Story also changes the form of the transom table.** Single uses one flat table with heights from the bottom of the wall. Multiple uses the Ground / Typical table with heights from the bottom of each storey. The two tables are stored separately, so switching Story does not clear the other one.
- **Slab Thickness works only in Multiple mode.** In Single mode it is treated as 0 even if it has a value, and no spandrel transom or glass is made.
- **Storey boundaries and slab rows beat any custom division row** near them.
- **H Division changes the hint line.** Even shows `-> actual: X`. Fixed has no fitting step and shows no hint.
- **Fixed rules at corners (not adjustable):** mullions are always built at true corners of the path with a proper corner joint, but **caps are never built at any corner**, which avoids two caps cutting through each other and leaving stray pieces. Transoms and transom caps are mitered at corners.

## Good to know

- **Only curves can be picked, not a closed boundary to fill.** A closed curve (a rectangle or polygon outline) is treated as a path whose first and last points meet at a true corner, exactly like any other corner in the middle of a path: mullion corner joint, glass and silicone extended to the joint line, and no cap there.
- **Very short or invalid curves are skipped silently** and the other picked curves are built as normal. If none of the picked curves can be used, the command is cancelled with a message.
- **Curved paths are broken into straight pieces by default** (Linearize Path on). With it off, the transoms, caps, silicone and glass follow the real bend, but mullions, mullion caps and mullion silicone stay straight rectangles regardless.
- **There are never caps at corners.** This is deliberate. If a cap looks "broken" at a corner, that is expected.
- **While the dialog is open, the path curves you picked at creation can be dragged and the preview updates live.** In `zCurtainWallEdit` the path is rebuilt from stored data and is no longer a real viewport object, so it cannot be dragged.
- **Exploding a baked curtain wall stops `zCurtainWallEdit` from recognizing it.** The edit command relies on an identity mark on every part. New faces or objects made by exploding do not carry it, and selecting them gives the message that it is not a zCurtainWall.
- **Exploding only some parts and then editing can leave orphan pieces.** The edit command deletes and rebuilds every remaining part that carries the mark, but exploded pieces that lost the mark are not cleaned up and stay as unmanaged duplicate geometry.
- **Moving the whole baked wall with another tool and then editing may lose that move.** The tool uses a hidden reference curve (the path anchor) to detect a move after baking. If that curve is deleted, exploded or joined with another object, a later edit ignores the move silently and rebuilds the path where it was when first baked.
- **Switching Story between Single and Multiple does not lose data.** The Ground / Typical table, the Height field and both transom tables are stored separately. Only the one that is active is used, and the values are still there when you switch back.
- **Reading an older baked file (made before Story and Slab Thickness existed):** if the file has a single Height only, it is restored as Single. If it is a stack of equal-height storeys, it is restored as Ground = 1 and Typical = N. If the storey heights differ (a shape the Ground / Typical pair cannot express exactly), all heights are merged into the Ground row with Count = 1, which keeps the correct total height but loses the storey split. This is a deliberate, safe compromise.
