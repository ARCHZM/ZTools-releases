# zSiteContext

`zSiteContext` builds the surroundings of a site from real map data: **terrain, buildings, roads, railways, waterways, green space and trees**. Use it to give `zAnalyzeShadow` and `zAnalyzeDaylight` a realistic neighborhood, or to block out a city-scale site quickly. The idea comes from tools like Elk and CadMapper for Grasshopper, but it runs inside ZTools with no extra plug-in.

How it works: draw a rectangle on the map, tick the layers you want, press **Query**. All ticked layers are queried in parallel and baked into the model, and the view zooms to the result. You can query again to replace the old result. **Cancel** removes everything baked during the current session of the dialog.

## Quick start

1. Run `zSiteContext`.
2. In **MAP**, search for an address or city (search box at the top right), or just drag a rectangle on the map.
3. In **DATA**, tick the layers to query. All are ticked by default: Terrain, Buildings, Roads, Railway, Waterway, Green Space, Trees and Contours.
4. **Terrain** needs a free **OpenTopography access token**. Click **Get it!** to open the sign-up page, then paste the token. It is remembered after the first time. If Terrain is ticked and no token is entered, the Status column of the Terrain row tells you so. The message disappears when you untick Terrain or enter a token.
5. Press **Query** at the bottom right. If Terrain is ticked, it is queried and baked first, so buildings, roads, railways and trees can follow the latest terrain. The other layers then run in parallel. If Terrain is not ticked, the other layers start immediately. Each row shows a spinner, a status text and a **Stop** button while it works, and a green (success) or red (failure) result when it is done.
6. A successful layer is baked into the model right away and the view zooms to it. You do not have to wait for OK.
7. Double-click the **Contours** row name to change the contour spacing (in feet). The current value is shown in the name, for example `Contours (3ft)`.
8. Double-click the **Dataset** text of any row (for example `Overture Buildings`) to open that data source's documentation.
9. **OK** closes the dialog and keeps everything that was baked. **Cancel** removes everything baked in this session and closes. If layers are still being queried when you close, you are asked to confirm.

## MAP

- **Search box**: type a city or address and the map moves there.
- **Rectangle tool**: drag a rectangle on the map for the query area. Drawing a new one replaces the old one.
- The MAP heading can be clicked to collapse or expand the map, which is useful on small screens. Collapsing does not lose your rectangle or the map position.

## DATA

Each layer row has a checkbox (include it in the next Query), an icon and name, then the **Dataset** it comes from, the **Year** (shown as 2026) and the **Status**.

- **OpenTopography access token**: only needed for Terrain. It is free, and it is saved after you enter it.
- **Terrain**: from OpenTopography. It automatically falls back through USGS 1 m, 10 m and 30 m, then Copernicus 30 m, SRTM and Copernicus 90 m, and uses the finest data available for your area. You do not choose the resolution. It creates one terrain mesh and, optionally, contour lines.
- **Contours**: queried together with Terrain, not a separate layer. The spacing is 3 ft by default and can be set from 1 to 10 ft by double-clicking the row name.
- **Buildings**: from Overture Maps. Buildings with a real height are extruded to that height. Buildings without height data are extruded to a 20 ft placeholder. The two kinds are baked to different layers so you can tell real data from assumptions.
- **Roads**: from Overture Maps (transportation, road types). Roads are extruded into strips whose width depends on the road class (motorway, primary, secondary, minor, footpath and so on) and follow the terrain. Each road also gets a centerline curve that follows the terrain. The strips go in the 3D group and the centerlines in the 2D group of the same class.
- **Railway**: same data source and method as Roads, on its own layers, with strips in 3D and centerlines in 2D.
- **Waterway**: from Overture Maps (water). Rivers, streams and canals become strips by width. Lakes, ponds and reservoirs become flat areas.
- **Green Space**: from Overture Maps (land use: parks, grass, golf, recreation, protected areas, gardens and so on). Flat outlines, no height.
- **Trees**: from OpenStreetMap (`natural=tree` points). Each tree is a simple sphere crown on a cylinder trunk, baked as block instances (one block definition, each tree scaled on its own) to keep the file small. Real height and crown size are used when the map data has them, otherwise random values (height 8 to 15 m, crown 4 to 8 m) are used. One query returns at most 2000 trees.

## How the layers work together

- After Terrain succeeds, Buildings, Roads, Railway and Trees are placed on that terrain by projecting their base points straight onto the terrain mesh. Roads and railways follow the terrain at every vertex along their length, so a long road really rises and falls with the ground.
- Contours are part of the Terrain query: same Query, same undo step. If Terrain is not ticked, no contours are made.
- Query runs all ticked layers in parallel, so they finish at different times, and one failing layer does not stop the others.
- **Stop** on a row only stops that row.
- Querying again removes the previous result of that layer before making the new one, so nothing piles up.

## Good to know

- A selected area smaller than about 250 m square makes Terrain, Buildings and Roads fail. Draw a larger rectangle.
- Road widths are typical values by road class, not measured widths, because real width data is almost always missing.
- Tree sizes are mostly random, because OpenStreetMap trees rarely carry height or crown size. They do not represent the real trees on site.
- Closing the dialog (X, OK or Cancel) while queries are running asks for confirmation. Answer **No** to let the queries finish first, or **Yes** to stop everything and close.
- **Cancel** removes everything baked in this dialog session, not only the last query. **OK** keeps everything.
- After you press **Stop**, the text changes to `Cancelling...` at once, but Buildings, Roads, Railway and Green Space can take a minute or more to stop, because they are waiting on a slow network request. Terrain, Waterway and Trees normally stop within seconds. This is normal.
- An internet connection is required.
