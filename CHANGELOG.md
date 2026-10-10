# Changelog

## Unreleased

- New look for every tool window: a warm grey ground with white cards, thin-bordered controls, a status line at the top that shows live results, and list selection that matches the segmented controls. Light and dark themes are both covered.
- Footer buttons are the same everywhere. Tools that create geometry end with `Reset` on the left and `Bake` and `Cancel` on the right. Tools that change selected objects use `Apply`, settings windows use `Save`, and analysis windows use `Bake` and `Close`. Live tools (`zLevelTag`, `zBlockManager`, `zSectionManager`) have only `Close`. Export buttons in the analysis tools moved into their EXPORT card.
- Parameter names in the dialogs are spelled out in full and no longer repeat the group title (for example `Width` and `Height` under `MOUNT PLATE`).
- `zArrayBetween`: arrays along a polyline of any number of picked points. Copies are distributed on each segment on its own and turn to follow the segment direction. The whole array is one group. The Y side is picked with a click, then Enter starts the start point. **Trim Segment Ends** (formerly Fit EndPt) shortens every segment. The Repick Cplane button and the Follow Path check box were removed.
- Stair tools (`zStairByRegion`, `zStairByCrv`, `zSpiralStair`): repeated flights of Plank stairs are baked as Rhino blocks, so a tall stair with identical storeys makes a much smaller file. Editing a baked stair works as before.
- `zStairByCrv`: fixed the stringers missing on upper storeys after Bake in Multiple mode, a gap before corner landings in Fit Curve mode, and an error in Ramp with Multiple storeys.
- Stairs: Bake with stringers is several times faster, and dragging a value shows the preview while you drag. `zStairByRegion` previews tall stacks faster.

## 0.1.1-beta3 (pre-release)

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
