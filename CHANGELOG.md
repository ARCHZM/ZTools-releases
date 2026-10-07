# Changelog

## 0.1.1-beta1 (pre-release)

First public pre-release of ZTools for Rhino 8 (Windows). It replaces an earlier test upload (0.1.0), which has been withdrawn.

- Parametric tools for stairs, railings, windows and doors, roofs, curtain walls and louvers.
- Site tools: `zSiteContext` builds terrain with contours, buildings, roads, railways, water and green space for a rectangle you draw on a map page in your web browser (with address search and an adjustable box). `zSiteModifier` reshapes a site surface.
- Analysis tools: `zAnalyzeTerrain` (elevation, slope, contours, check points), `zAnalyzeDaylight`, `zAnalyzeShadow`, `zAnalyzeView` and `zAnalyzeViewQuality`.
- Utility and documentation tools for dimensions, blocks, level tags, sections and more. See the README for the full list.

Known limits of this pre-release:

- Rhino 8 for Windows only. Mac is not supported.
- `zSiteContext` needs an internet connection and, for terrain, your own free OpenTopography access token. The trees layer is not included.
- Roads and railways can take a minute or more on a dense area.
- Keep a backup of your file before running batch tools.
