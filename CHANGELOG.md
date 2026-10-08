# Changelog

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
