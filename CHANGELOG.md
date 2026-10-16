# Changelog

## Unreleased

- Number fields no longer stop you from typing a large length: the slider range stays, but you can type beyond it. This lifts the old caps in `zCurtainWall` (Height, Spacing, Glass Thickness, mullion, transom, cap and silicone sizes), `zDoor` (Width, Pull Length), `zRailing` and `zRailingSide` (Height), `zAnalyzeView` and `zAnalyzeViewQuality` (View Range) and `zDimLength` (Extension Line).
- Offsets and thin parts may be 0: dimension offsets, stair slab, ramp and stringer sizes, the `zWindowCorner` sash, the `zRailing` Offset (now 0 or more), mansard Break Inset, `zAnalyzeView` Elevation.
- `zWindow` Rows and Columns go up to 50, `zArrayBetween` Count starts at 2, `zAnalyzeTerrain` Primary Contour goes up to 100, `zSetCamera` viewport size is 1 to 16384.

## 0.2.3

- Hover drawings: every callout curve now runs from its label straight to the side the arrow arrives from, without loops around the drawing; labels move above, below or beside the dimension when needed. The door leaf **Thickness** drawing shows the leaf swung open so the dimension sits clear of the walls.
- Drawings of one series share one scale and one text size (curtain wall mullion plans and transom sections, door plans), and the door Transom and Sidelight drawings show the same leaf as the other door drawings.
- The Package Manager entry is now named **ZTools**.
- The same thing is called the same name everywhere: the curtain wall glass card is **GLASS** (was PANEL), the window Horizontal Split row is **Count** (was Amount), and every on/off section caption reads **ENABLED** (the door glass said ENABLE).
- Window layout tidy-up: `zSetCamera` is two columns instead of one very tall one; `zDoor` and `zCurtainWall` balance their two columns; `zScatterToCrv` names the row **Spacing** or **Count** after the Mode; `zWindowCorner` uses **Offset** and **Height** in GENERAL.
- One card vocabulary for the architecture tools: **LAYOUT** (choices), **GENERAL** (overall sizes), **DIVISION** (where it is cut into cells) and one card per member type. `zWindow` has a DIVISION card (Horizontal Split, Transom, Glass Split); `zDoor` moves Transom and Sidelight into LAYOUT; `zWindowCorner` and `zRoof` call their size card GENERAL; `zCurtainWall` gets a GENERAL card (floor heights, slab thickness) and DIVISION shows COLUMNS and ROWS; `zRailing` and `zRailingSide` get a LAYOUT card (System, Flip In/Out, Linearize Path) and a separate HANDRAIL card.
- `zAnalyzeTerrain` is now SETUP / DISPLAY / EXPORT like the other analysis tools (Mesh Detail in SETUP, label settings in DISPLAY). `zArrayBetween` has only the DISTRIBUTION card. `zTextureMappingByObjects` calls its card TRANSFORMATION. `zPackObjects` ends with **Apply**.

## 0.2.2 (replaced by 0.2.3)

- `zRoof` is one column of cards (LAYOUT, DIMENSIONS, DORMERS). Hover **Style** and **Height Mode** to see drawings of the choices.
- `zWindow`: Horizontal Split and Transom (with Split At) now sit in LAYOUT, Glass Split sits in the GLASS card, and the DIVISION card is gone. Hover **Type** for the four window types, and **Open Amount** for how it works in the current type.
- The **Linearize Path** drawing in `zCurtainWall` is redrawn so the difference is easy to see, and its check box (and Flip In/Out) now show their explanation under the drawing.
- New `zUnits` toolbar icon: two opposite arrows over a ruler.

## 0.2.1 (replaced by 0.2.2)
Fixes for a fresh install, and more hover drawings.

- A new installation now opens all eight ZTools toolbars (All, Site, Architecture, Geometry, Dimension, Analyze, View, Utility), stacked from top to bottom, each as wide as its own buttons. Before, the Site, Dimension and View toolbars were not opened at all and the others were piled on one spot at a fixed small size, so `ZTools All` was cut off.
- Hover drawings (Off | On) for 12 check boxes: `Flip In/Out` and `Linearize Path` (railings, curtain wall), `Fitting` and `Base Plate` (railings), `Even Tread Split`, `Trim Underground Geo`, `Reverse Path Direction`, `Retain Path Curvature`, `Mirror Stair Flight` (stairs), `Clockwise` (spiral stair), `Force Pyramid` and `Flip Ridge` (roof).
- Cleaner drawings: view distance and dimension callouts no longer run along their own lines, the gambrel and pitch roof drawings use the same text size as the rest, and the dormer sections show the eave wall and a window.

## 0.2.0 (replaced by 0.2.1)
One unit switch for the whole tool set.

- New `zUnits` button in the Utility toolbar (and the `zUnits` command): switches every tool between **Imperial** (inches, feet) and **Metric** (millimeters, meters). Imperial is the default.
- The mm / inch switch is gone from the stair, railing, roof, window, door, curtain wall and louver windows. Each system keeps its own values, nothing is converted when you switch, and open windows follow at once.
- The Rhino document unit no longer decides what the windows show. You can change the document unit while a window is open without restarting; the preview is rebuilt.
- The dimension tools (`zDimLength`, `zDimRadius`, `zDimArea`, `zDimVolume`, `zDimAngle`) list only the formats of the active system, and their text size, offset and arrow presets are the same physical sizes in inches or millimeters. This fixes tiny text in millimeter documents.
- Site and analysis tools use feet or meters: `zAnalyzeView`, `zAnalyzeTerrain`, `zSiteContext`, `zSiteModifier`, `zSetCamera` (eye height presets in meters too) and `zLevelTag` (accepts `3.2m`, `3200mm`, `10ft`, metric level presets).
- `zFilletCrv`, `zPackObjects`, `zArrayBetween`, `zScatterToCrv` and `zTextureMappingByObjects` show their distances in inches or millimeters.
- Angle fields show a degree sign.
- Every hover tip, with or without a drawing, and the floating warning card next to a tool window now share one look: a rounded card with a soft shadow. Text under a drawing is left aligned.

## 0.1.4 (replaced by 0.2.0)

Same tools as 0.1.3, with two look changes: color swatches show a small color chip and the hex value in the same field style as the number fields, and a selected list row gets a thin frame around the whole row.

## 0.1.3 (replaced by 0.1.4)

Same tools as 0.1.2. The license text (EULA) no longer calls ZTools a pre-release.

## 0.1.2 (replaced by 0.1.3)

Same tools as 0.1.1-beta4, plus a fix: hovering a check box that has a drawing no longer shows a second plain tip, and the explanation now appears under the drawing.

## 0.1.1-beta4 (pre-release, replaced by 0.1.2)

- Hover a parameter name in a tool window and a small architectural drawing shows what it measures, with the explanation under it. Drawings exist for railings, stairs, doors, windows, window corners, curtain walls, roofs, louvers, cameras, dimension tools, `zScatter`, `zArrayBetween`, `zAnalyzeView`, `zAnalyzeTerrain`, `zAnalyzeDaylight` and `zLevelTag`. Choices (for example Single or Split, Plank or Monolithic) and check boxes get a side by side comparison.
- New look for every tool window: a warm grey ground with white cards, thin-bordered controls, a status line at the top that shows live results, and list selection that matches the segmented controls. Light and dark themes are both covered.
- Footer buttons are the same everywhere. Tools that create geometry end with `Reset` on the left and `Bake` and `Cancel` on the right. Tools that change selected objects use `Apply`, settings windows use `Save`, and analysis windows use `Bake` and `Close`. Live tools (`zLevelTag`, `zBlockManager`, `zSectionManager`) have only `Close`. Export buttons in the analysis tools moved into their EXPORT card.
- Parameter names in the dialogs are spelled out in full and no longer repeat the group title (for example `Width` and `Height` under `MOUNT PLATE`).
- `zArrayBetween`: arrays along a polyline of any number of picked points. Copies are distributed on each segment on its own and turn to follow the segment direction. The whole array is one group. The Y side is picked with a click, then Enter starts the start point. **Trim Segment Ends** (formerly Fit EndPt) shortens every segment. The Repick Cplane button and the Follow Path check box were removed.
- Stair tools (`zStairByRegion`, `zStairByCrv`, `zSpiralStair`): repeated flights of Plank stairs are baked as Rhino blocks, so a tall stair with identical storeys makes a much smaller file. Editing a baked stair works as before.
- `zStairByCrv`: fixed the stringers missing on upper storeys after Bake in Multiple mode, a gap before corner landings in Fit Curve mode, and an error in Ramp with Multiple storeys.
- Stairs: Bake with stringers is several times faster, and dragging a value shows the preview while you drag. `zStairByRegion` previews tall stacks faster.

## 0.1.1-beta3 (pre-release, replaced by 0.1.1-beta4)

Reworked `zAnalyzeTerrain`:

- Three analysis modes: Elevation, Slope and Aspect. Contour lines are now a switch inside Elevation, with an interval and primary (bold, labelled) contours.
- Analysis points: High, Low and Average points are placed when the window opens. Rename them, drag them into any order, and select several to compare them along a path in list order.
- Aspect: a fixed compass color ring, a downslope arrow for every point, and an optional field of arrows over the surface with adjustable density.
- Cleaner analysis grid: no triangles on ordinary curved surfaces, and the Detail setting (1 to 10) now gives a finer grid. Contour lines stay fast on very large sites.
- Fixed east and west being swapped in aspect colors and labels.

## 0.1.1-beta2 (pre-release, withdrawn)

Same tools as 0.1.1-beta1, which has been withdrawn. The package description is now a short list of the toolbar groups, and the package homepage points to this repository.

## 0.1.1-beta1 (pre-release, withdrawn)

First public pre-release of ZTools for Rhino 8 (Windows). It replaces an earlier test upload (0.1.0), which has been withdrawn.

- Parametric tools for stairs, railings, windows and doors, roofs, curtain walls and louvers.
- Site tools: `zSiteContext` builds terrain with contours, buildings, roads, railways, water and green space for a rectangle you draw on a map page in your web browser (with address search and an adjustable box). `zSiteModifier` reshapes a site surface.
- Analysis tools: `zAnalyzeTerrain` (elevation, slope, aspect, contour lines, analysis points), `zAnalyzeDaylight`, `zAnalyzeShadow`, `zAnalyzeView` and `zAnalyzeViewQuality`.
- Utility and documentation tools for dimensions, blocks, level tags, sections and more. See the README for the full list.

Known limits of this pre-release:

- Rhino 8 for Windows only. Mac is not supported.
- `zSiteContext` needs an internet connection and, for terrain, your own free OpenTopography access token. The trees layer is not included.
- Roads and railways can take a minute or more on a dense area.
- Keep a backup of your file before running batch tools.
