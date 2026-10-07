# ZTools for Rhino 8

A collection of parametric modeling and analysis tools for architects and designers working in Rhino 8 for Windows: stairs, railings, windows and doors, roofs, curtain walls, sections, dimensions, site context, daylight, shadow and view analysis, block and level management, and small utilities.

**Status:** pre-release (`0.1.0-beta1`). Free to use, including for commercial projects, under the [EULA](EULA.txt). Report problems in [Issues](../../issues) or through the contact form at https://www.ze-meng.com/.

> Pre-release means some edges are still rough. Keep a backup of your file before running batch tools, and use Undo if a result is not what you expected.

## Install

1. In Rhino 8, run `_PackageManager`.
2. Turn on the option to show pre-release versions. Pre-release packages are hidden by default. *(verify the exact label before publishing)*
3. Search for `ztools`, install it, and restart Rhino.
4. Run `zTools` to show the toolbar, or type any command below.

Requirements: Rhino 8 for Windows. Mac is not supported yet.

## What is inside

The tools are grouped like the toolbars. Tools that make geometry from a dialog have an `...Edit` command (for example `zRoofEdit`) that reopens a baked result with its saved settings.

### Site
- **zSiteContext** - Builds terrain with contours, buildings, roads, railways, water and green space for a site whose rectangle you draw on a map page opened in your web browser (with address search). (The trees layer is not in this pre-release.)
- **zSiteModifier** - Reshapes a site surface so it follows the top of the buildings or terrain you pick, with a smooth transition.

### Architecture
- **zRoof** - Gable, hip, gambrel and mansard roofs from a footprint outline, with dormers.
- **zLouver** - Louver panels (frame and angled blades) on a flat face.
- **zWindow** - Fixed, hung, casement and sliding windows inside a closed opening curve on a wall; cuts the opening when you bake.
- **zWindowByCrv** - A run of windows along a path curve, divided by length.
- **zCurtainWall** - Curtain walls with mullions, transoms, glass, silicone and caps along path curves, single or multi-storey.
- **zDoor** - Single or double doors with frame, glass, handle styles, optional transom and sidelights; cuts the opening when you bake.
- **zStairByCrv** - Stairs that follow a path curve, as planks, a monolithic slab or a ramp.
- **zStairByRegion** - Straight, L, U, Z or C stairs fitted into a rectangular stair-well outline, single or multi-storey.
- **zSpiralStair** - Spiral stairs from a straight edge that gives the radius direction.
- **zRailing** - Railings along curves with baluster, cable, glass, metal or guardrail infill, plus top, intermediate and bottom rails and a handrail.
- **zRailingSide** - Side-mounted railings: posts or glass clamps fixed to the edge of a slab.

### Geometry
- **zScale** - Batch scale, uniform or per axis, each object around its own base point.
- **zArrayBetween** - Arrays objects between two picked points on a plane, by count or spacing.
- **zFilletCrv** - Fillets the corners of one or many curves at once, by radius or distance.
- **zBreakAtIntersections** - Breaks curves where they cross each other or themselves.
- **zScatterToCrv** - Scatters copies along a curve with random scale, rotation and offset, avoiding collisions.
- **zScatterToSrf** - Scatters copies over a surface with random scale and rotation, avoiding collisions.
- **zProjectToGround** - Drops objects straight down onto a ground surface, with optional random scale and rotation.
- **zTextureMappingByObjects** - Applies box texture mapping sized to each object, with optional random rotation and offset.
- **zStraighten** - Rotates each object so its longest straight edge is parallel to the X axis.
- **zPackObjects** - Packs object footprints into a rectangle on the ground plane.
- **zModelScaler** - Scales a selection to a print or model scale and converts between real and model dimensions.

### Dimension
- **zDimLength** - Labels the length of curves, edges and faces.
- **zDimAngle** - Labels the angle at each corner between two straight segments.
- **zDimRadius** - Labels the radius of arcs and fillets.
- **zDimArea** - Labels the area of closed planar curves, surfaces and meshes.
- **zDimVolume** - Labels the volume of closed solids.

### Analyze
- **zAnalyzeTerrain** - One window for ground analysis: color surfaces by elevation or slope, draw labeled contour lines, add check points, and bake the result as geometry.
- **zAnalyzeDaylight** - Annual daylight metrics (sDA, ASE, UDI) from an EPW weather file.
- **zAnalyzeShadow** - Shadow coverage and sun hours for a site, for one day, key dates or the whole year, with image and GIF export.
- **zAnalyzeView** - Isovist analysis from a grid of points: outdoor visibility and openness.
- **zAnalyzeViewQuality** - Scores what each point can see, using values you give to objects such as trees, landmarks and water.

### View
- **zSetCamera** - A two-point-perspective camera in its own floating viewport, with lens, height and shift controls.
- **zLockAspectRatio** / **zUnlockAspectRatio** - Keeps a floating viewport at a fixed aspect ratio while you resize it.
- **zFaceCamera** - Rotates selected faces around Z to face the current view.

### Utility
- **zInspector** - Live count of geometry types with quick select, isolate, hide, lock and delete; `zInspectorToggle` switches its HUD.
- **zLevelTag** - Defines building levels by elevation and tags every object with its level; export by group, block or layer.
- **zBlockManager** - Batch rename, re-layer, color code, rebase, reset scale, purge and merge block definitions.
- **zSection** / **zSectionManager** - Live section lines with a manager; a section can also be cut into real geometry.
- **zImportDWG** - Sorts the layers of an imported Revit DWG into a standard layer tree.
- **zRevitMapping** - Stores a Revit category and type for each ZTools command and stamps it on what the command bakes, for Rhino.Inside.Revit workflows.
- **zThemeSettings** - Sets the colors and light or dark mode of all ZTools dialogs.

Longer guides for each tool will be added during the pre-release. Tell me which ones you need first.

## What ZTools does with your data

- **Network.** Only `zSiteContext` goes online, and only for the area you choose and the layers you tick:
  - OpenTopography for terrain. It needs a free access token (API key) from your own account at https://opentopography.org/, stored on your computer and used only for your own requests. Free keys have a daily request limit (about 50 calls per day for non-academic accounts at the time of writing), and the 1 m USGS data is limited to academic accounts.
  - Overture Maps data (buildings, roads, railways, water, green space) read from its public storage on Amazon S3. DuckDB, the engine ZTools uses to read it, downloads two small extensions (`httpfs`, `spatial`) from DuckDB's extension server the first time.
  - The site area is picked on a map page that ZTools opens in your own web browser. The page is served from your own computer (127.0.0.1, a random port, protected by a random token, not reachable from other computers). Your browser loads the MapLibre map library from unpkg.com and the map from OpenFreeMap (openfreemap.org, data from OpenStreetMap). When you press Search, the address you typed is sent through ZTools to Nominatim (nominatim.openstreetmap.org), at most once per second. The page sends only the rectangle you draw back to ZTools. You can also type or paste the four coordinates into the dialog instead.

  Nothing else is sent anywhere, and ZTools collects no usage data.
- **Your Rhino file.** Several tools save their results as user text or user dictionary entries on your objects (for example level tags, view scores and tags, analysis settings). They stay in your file and travel with it. `zLevelTagClear` removes level tags.
- **Settings.** Window positions and tool settings are stored in Rhino's own settings on your computer.
- **Files on disk.** ZTools writes files only where you ask it to (for example exports, CSV and PNG).

## License and third-party software

ZTools is released under the [EULA](EULA.txt): free to install and use, not to be copied, resold or reverse engineered. Third-party components and their licenses are listed in [THIRD_PARTY_NOTICES.txt](THIRD_PARTY_NOTICES.txt).

Map and site data: © OpenStreetMap contributors, available under the [Open Database License](https://opendatacommons.org/licenses/odbl/); Overture Maps Foundation (https://overturemaps.org/); OpenTopography and its data providers. When ZTools bakes this data it writes the required notice into the `zSiteContext` layer's user text ("Data notice") so it stays with your file; if you publish drawings or models made from this data, keep that notice with them.

OpenStreetMap is a trademark of the OpenStreetMap Foundation. ZTools is not endorsed by or affiliated with the OpenStreetMap Foundation.

Terrain: this work is based on API services provided by the OpenTopography Facility with support from the National Science Foundation under NSF Award Numbers 2410799, 2410800 & 2410801.

## Feedback

- Bugs and ideas: [Issues](../../issues). Please include your Rhino version, the command, and a short description or screenshot.
- Contact: https://www.ze-meng.com/about

## Changelog

- **0.1.0-beta1**: first public pre-release.
