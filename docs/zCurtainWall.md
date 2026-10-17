# zCurtainWall

`zCurtainWall` builds a complete curtain wall system along one or more path curves: mullions, transoms, glass, silicone joints and cover caps. It handles corners along the path, and can split the wall by **storey**.

Good for:

- A curtain wall along a plan path, including polyline corners and closed outlines, with mullions, transoms and glass panels.
- A single-storey or multi-storey wall (Story = Multiple) where transoms, and even spandrel glass, should appear automatically at the floor boundaries.
- Concept-stage models that show silicone joints and cover caps.

Not for:

- Filling a closed boundary region. The tool only picks path curves. A closed curve is treated as a path whose first and last point meet at a true corner.

## Quick start

1. Run `zCurtainWall` and pick one or more path curves (see Good to know).
2. The dialog opens with a live preview. The left column has **LAYOUT** and **DIVISION**. The right column has **GENERAL**, **MULLION**, **TRANSOM** and **GLASS**.
3. In **LAYOUT**, choose **Story**: Single or Multiple. In **GENERAL**, type the Height (Single) or fill the Ground / Typical floor table (Multiple).
4. In **DIVISION**, set the COLUMNS **Division** (Even or Fixed) and COLUMNS **Spacing** for the bay width. In the ROWS section, add custom transom rows if you want them.
5. In the **MULLION**, **TRANSOM** and **GLASS** cards, set the width and depth of the mullions and transoms (Body and Cap side by side, each with its own check box), and the glass thickness and silicone width.
6. If the path curves you picked still exist in the document, you can drag their control points in the viewport and the preview follows.
7. **Bake** creates the real geometry (including a hidden anchor curve of the path, so that `zCurtainWallEdit` can detect if you moved the wall). **Cancel** discards the preview.
8. To change a baked curtain wall, select any part of it and run `zCurtainWallEdit`. It rebuilds the path and every setting from what was stored at bake time and reopens the same dialog. Bake deletes the old parts and builds new ones. The rebuilt path is not a real viewport object, so you cannot drag its control points, and it works only while the picked part still carries its identity mark (it has not been exploded).

## LAYOUT

- **Story** (Single / Multiple): how many storeys the wall covers.
  - **Single**: one floor (the **Height** is in GENERAL).
  - **Multiple**: stacks the floor table in GENERAL (a Typical row and a Ground row (each with **FFH**, the floor-to-floor height, and **Count**, the number of repeats). Ground is the lowest storey (Count is normally 1, and Z = 0 starts at its floor). Typical is the standard storey stacked above Ground. The total height is the sum of all storeys, so you no longer type a Height. A Typical Count of 0 is allowed and means only the Ground storey.
- **Top Edge** (Follow / Flat / Profile): where the top edge of the wall sits. The path curve you pick is always the bottom edge, so a path that rises or falls gives a sloped bottom.
  - **Follow** (default): the top is parallel to the bottom and the wall keeps one constant height. This is how the tool always worked.
  - **Flat**: the top is horizontal. **Height** is measured up from the highest point of the path curve, so the wall gets taller where the path is lower.
  - **Profile**: the top elevation is read from a second curve. Click **Pick Top Curve** (the dialog hides while you pick). Each point of the path takes the Z of the profile curve at the same position in plan, so draw the profile right above the path. The Height field is greyed out. Where the profile does not cover the path, those bays are left out and the warning window says so.
  - The top is a straight line from one mullion to the next, and mullions stay vertical.
  - **Rows** (the transom rows in DIVISION) follow the top: with **Follow** they stay parallel to the bottom, measured up from it (as before). With **Flat** they are horizontal, measured up from the lowest point of the path. With **Profile** they run parallel to the top curve and the row Height is the distance below it. A row that does not fit between the bottom and the top is left out of that bay. With Story = Multiple, the floor lines stay horizontal in every mode: the slope only changes the lowest and the highest storey.
- **Flip In/Out**: the path you pick is the centerline of the glass itself. By default the mullions are on the interior side and the caps on the exterior side. Tick this to swap the two sides. It only affects the depth direction, not widths or heights. Hover its name to see a drawing of Off and On.
- **Linearize Path**: on by default. At the soft polyline nodes that the tool creates when it breaks up a smooth curve, transoms, caps, silicone joints and glass or spandrel panels are cut as straight rectangles, one per bay. When off, these parts follow the real bend at every internal node with zero-width miters, so the transoms and glass hug the curve continuously. Mullions, mullion caps and mullion silicone always stay straight rectangles, whatever this setting is. Hover its name to see a drawing of Off and On.

## GENERAL

- Lengths follow the global unit system (see [zUnits](zUnits.md)): inches in Imperial, millimeters in Metric. There is no unit switch in this window. Values are converted to the document units on bake.
- **Height** (Single story): the total wall height. All heights (this one, the floor heights FFH, and the transom row heights) are in **feet** in Imperial and **metres** in Metric; widths, depths and thicknesses stay in inches or millimeters.

## DIVISION

Where the wall is cut into cells. At the top of the card is the **elevation overview** (no title), then two collapsible headings, **COLUMNS** (vertical bays) and **ROWS** (horizontal custom rows). They collapse only and have no on/off switch.

### COLUMNS

- COLUMNS **Division** (Even / Fixed): how the width of each bay is decided.
  - **Even**: fits a whole number of bays and shares the remainder equally among them.
  - **Fixed**: every bay is exactly COLUMNS Spacing, and the last bay may be shorter.
- COLUMNS **Spacing**: the target bay width (the mullion spacing), always measured on the plan, never along a slope. In Even mode, the top status line shows the real spacing.

### ROWS

The custom division table is the only way to divide vertically. There is no Count or Spacing mode.

- **Pattern** (four squares in each row): a four-bay pattern that repeats along the wall, whatever the real number of bays. Filled = the transom is there, hollow = switched off; click a square to flip it. All four are filled by default.
- **Elevation overview**: the drawing at the top of the DIVISION card. It is a schematic, not the real wall in length: it always shows four glass bays, and one storey (Single) or two (Ground, then one Typical, at their floor heights) however many bays or storeys the wall really has. **Click** empty wall to add a row at that height (in Multiple, a Ground or Typical row depending on the storey you click in), **click** a row line to select it (it only highlights; removing a row or switching a bay is done in the table) and **drag** a row line to move it. The drawing is in the style of the hover drawings: hatched ground under the wall, three reflection lines on every glass pane, the row heights written as feet-inches (8'-6") or metres, and level marks on the right at the floor lines (Division = Fixed shows the short remainder bay at the right end, Even shows equal bays) (the base as ±0'-0", every storey boundary and the top). The wall is drawn with the real slope of the path and of the top (Follow parallel to the bottom, Flat level, Profile as the picked curve), and the ground follows the bottom edge. **Drag** the storey boundary mark or the top mark on the right to change the Ground floor height, the Typical floor height or the wall Height (the top mark of a Profile top is fixed by the curve). The selected row is red, and its squares in the table turn red too. The strip and the table are the same rows, so you can use either.

- **Single story**: one flat table. Click **+ Add Row** to add a row. Each row has a **Height** and a Pattern (the four squares). The Height column header says what it is measured from: **FROM BASE** (the bottom of the wall, that is the Z of the input curve), **FROM LOW** (Top Edge = Flat: the lowest point of the path, rows are level) or **BELOW TOP** (Top Edge = Profile: the distance below the top curve). Each row has a delete button on the right. Rows keep the order you added them.
- **Multiple stories**: one combined table with a **Scope** column (G / T) in front of each row. The Height column is headed **FROM FLOOR**: it is measured from the floor of the storey the row belongs to, not from the bottom of the wall. A **Ground** row (G) applies once, to the Ground storey, from the bottom of the wall. A **Typical** row (T) is repeated on every storey above the Ground, from the floor of each one. The transom on every floor line is not a row: it belongs to the **TRANSOM** switch (on = a transom on each floor line, off = none). A new Multiple wall starts with two ordinary rows, 3 ft (0.9 m) below the top of the Ground storey and of each Typical storey (the underside of the floor slab); change or delete them as you like. The table keeps itself in order from the bottom of the wall to the top: the Ground rows by height, then the Typical rows by height (so a Typical 12 ft sits above a Ground 17 ft, because it counts from a higher floor). A new row is placed in that order as soon as you finish editing it.
- **Selection**: a row you click or drag in the elevation strip gets a dark bar at the left of its table row, and clicking a row's Height box selects its line in the strip.

## MULLION, TRANSOM and GLASS

The member sizes, in the right column below GENERAL. In MULLION and TRANSOM the **Body** column is the member itself and the **Cap** column its pressure cap; the check box in each column header turns that part on or off. A part that is off greys its fields (a cap also depends on its member being on). Hover the **Width** or **Depth** row name for a drawing that marks both the member and the cap size.

### GLASS

- GLASS **Thickness** (default 12 mm / 0.5 in): the thickness of the glass panel, offset inward from the frame surface.
- **Silicone** **Width** (default 10 mm / 0.4 in): always on, no switch. One setting for both axes. The silicone joint is centered on each internal mullion and transom position and shares the depth band of the glass panel.

### MULLION (the Body check box is the switch for the vertical members, on by default)

- **Width / Depth** (default 60 mm x 101.6 mm / 2.4 in x 4 in): the section of a mullion, width along the wall and depth along the wall normal.
- **Cap** (the check box over the right column, on by default, available only while Body is on): **Width / Depth** (default: the mullion width by 20 mm / 0.8 in). The cap sits against the outer glass surface (which sits against the outer mullion surface) and runs the full height without a break.

### TRANSOM (the Body check box is the switch for the horizontal members, on by default, with the same structure as MULLION)

- **Width / Depth** (default the same as the mullion), **Cap** (default the transom width). They mean the same as the matching MULLION items but apply to the horizontal parts.

## How the settings work together

- **Cap depends on its member.** The Cap check box of MULLION or TRANSOM can be used only while that Body is on; turning Body off greys the Cap column too.
- **Silicone is independent of the member switches.** It is always generated, even if the mullion or transom itself is off.
- **Story drives the Height field and the floor table.** Single shows only Height. Multiple hides Height, derives it from FFH x Count of Ground and Typical.
- **Story also changes the form of the transom table.** Single uses one flat table with heights from the bottom of the wall. Multiple uses the Ground / Typical table with heights from the bottom of each storey. The two tables are stored separately, so switching Story does not clear the other one.
- **The transoms under the floors are ordinary rows; the transom on the floor line is the TRANSOM switch.** There is no slab setting any more.
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
