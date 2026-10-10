# zAnalyzeTerrain

`zAnalyzeTerrain` analyzes a ground surface or mesh in one window. It colors the ground by **Elevation**, **Slope** or **Aspect**, draws **contour lines**, and lets you place **analysis points** to read exact values and compare locations. The result is painted onto your original objects, so nothing is hidden or copied until you press **Bake**.

Use it to check height differences on a site, find slopes that are too steep, see which slopes face south, produce a contour drawing, export a viewport image with a legend, or bake the result as real geometry.

## Quick start

1. Run `zAnalyzeTerrain`. Select the ground surfaces or meshes first, or select them when the command asks. If the objects from your last analysis still exist, press **Enter** to reuse them.
   The objects are fixed before the window opens. To analyze different objects, close the window and run the command again.
2. The window opens with three analysis points already placed: **High**, **Low** and **Average**.
3. Switch between **Elevation**, **Slope** and **Aspect** in **SETUP**. The colors update in the viewport right away. Statistics appear at the top right of the viewport and the legend at the bottom left.
4. To read more values, click **+** at the top right of the **ANALYZE POINTS** table and click on the model. Click as many points as you like and press **Enter** to finish.
5. Adjust colors in **DISPLAY**, labels in **LABELS** and mesh quality in **MESH**.
6. Press **Bake** to turn what you see into real geometry. The window stays open, so you can switch the analysis and bake again. **Export PNG** saves the viewport as an image, **Reset** restores the defaults, and **Close** closes the window.

## The window

From top to bottom: **ANALYZE POINTS** across the full width, then **SETUP** and **LABELS** on the left and **DISPLAY**, **MESH** and **EXPORT** on the right, and the buttons **Reset**, **Export PNG**, **Bake** and **Close** at the bottom. The width is fixed. You can make the window taller, and only the points table grows.

## ANALYZE POINTS

Each row is one point. From left to right:

- **Drag handle**: drag a row up or down to change its position in the list. A dark red line shows where it will land.
- **Color swatch**: the point's color in the current analysis (a color on the gradient for Elevation and Slope, a color on the compass ring for Aspect).
- **POINT, ELEVATION, SLOPE, ASPECT**: the name, the elevation, the slope, and the downslope direction as a compass letter with an arrow. The arrow points up for north. Flat ground has no arrow.
- **Trash icon**: removes the point.

**+** (Add Point) at the top right of the table: click points on the model, then press **Enter**. Shift+click removes the nearest point.

### The default points

- **High** is the highest point of the analyzed surface and **Low** is the lowest.
- **Average** is not a location. It shows the average over the whole surface: elevation and slope are averaged by area, and the aspect is averaged by direction. Its marker sits near the surface's center for reference.
- If you delete one of these points, it does not come back.

### Renaming, ordering and selecting

- **Rename**: double-click the name, or right-click and choose **Rename**. An empty name restores the automatic one. Points you add yourself are named P4, P5 and so on, by their position in the list.
- **Reorder**: drag the handle on the left of a row.
- **Select**: click a row. Ctrl+click toggles a row and Shift+click selects a range. Click empty space to clear the selection. **Del** removes the selected points and **Esc** clears the selection.
- **Right-click menu**: Rename, Move Up, Move Down, Delete, Zoom to Point, Select All, Invert Selection, Clear All (asks first).

### Comparing points

Select two or more points. A summary line appears under the table, and the points are joined in the viewport **in the order of the list**:

- Two points: `High to Low: 104.7' apart, -27.4', 26.2% average slope` (horizontal distance, height difference, average slope).
- Three or more points: `High > Low > Average: 826.7' along the path, -15.0' net, 6.3% average slope` (horizontal length along the path, net height change from first to last, average slope).

The line in the viewport labels the horizontal length of each leg. To change the order of the path, drag the points in the list. The slope unit follows **Show as** in the Slope settings.

### How points look in the viewport

Each point is a small dot on the ground with a thin line rising from it. At the top of the line is a label with the point name on the first line and the value on the second line. The label color is the point's color in the current analysis, and it has a white frame. Hovering a row or selecting it makes the frame and the line thicker. A small arrow at the point shows the downslope direction (its size is set by **Arrow size** in LABELS). The labels, lines and arrows are always drawn in front of the surface.

## SETUP

Choose **Elevation**, **Slope** or **Aspect**. Switching is fast because results are cached.

### Elevation

Colors the surface by height.

- **Zero at**: the height shown as 0. This only changes the numbers, never the colors or lines. **Pick** lets you click a point on the model to set it.
- **Format**: the length unit used for elevations, contour labels and distances.
- **Interval**: the height difference between neighboring contour lines. Whole units only.
- **Primary Contour**: every Nth contour is a primary contour, drawn thicker and labeled with its elevation. 0 means none.
- **Contour Lines**: draws contour lines on the surface. On by default. Over the colored surface the lines are dark gray. With **Show Colors** off, the lines take the gradient colors and are shown alone.

### Slope

Colors the surface by steepness.

- **Show as**: degrees, percent or ratio.
- **Flag over**: faces steeper than this value are painted a fixed warning red, and the statistics show the share of the area over the limit.
- **Custom**: the threshold used when **Flag over** is set to Custom, in the unit chosen in **Show as**.
- The top of the color ramp is the slope that 98% of the area stays below. A very steep face, such as a nearly vertical wall, shares the top color instead of flattening the colors of the rest of the site. When this happens the ramp shows a value like `>= 14.1`, a note appears in the viewport, and **Max** in the statistics is still the true maximum.
- The statistics also list the share of the area in common accessibility bands: 0-5%, 5-8.3% (1:12), 8.3-20% and over 20%. The bands are fixed.

### Aspect

Colors each face by the direction it slopes down towards: north blue, east green, south orange and yellow, west magenta, wrapping around. Ground flatter than 1% is gray. The statistics show **Facing** (the most common direction and its share) and **Flat** (the share of flat area).

- **Arrow Density**: how many arrows to place along the longer side of the site. The default is 21. A larger number gives more arrows.
- **Arrows on Surface**: on by default. Places evenly spread arrows over the surface that point downhill and turn smoothly with the terrain. Flat ground (under 1%) has no arrows.

## MESH

**Detail** is a whole number from 1 to 10. The default is 8. It sets how fine the analysis mesh is. Higher is finer and slower.

- For ordinary curved surfaces, the analysis mesh is a regular grid of four-sided cells, from 16 cells along the long side at level 1 up to 220 at level 10. There are no triangles.
- For other geometry (trimmed surfaces, meshes) a higher number gives a finer mesh.
- On a very large site with a very dense mesh (more than 200,000 faces) and Detail above 5, the contour lines are computed from the Detail 5 mesh so they stay fast. Colors and aspect still use the fine mesh.

## LABELS

- LABELS **Size**: the text size of the labels, in the viewport and in baked text.
- **Decimals**: the number of decimal places in values.
- **Arrow size**: the length of the downslope arrow at each point, in screen pixels. The default is 38.
- LABELS **Height**: how far a point's label is raised above the point. The default is 100. It applies in the viewport and when baking.

## DISPLAY

- **Gradient bar**: double-click to add or remove a color stop, right-click a stop to change its color, and right-click the bar for presets. The button next to the bar reverses the gradient. In Aspect the bar shows the fixed compass ring and cannot be edited.
- **Grid Divisions**: draws the edges of the analysis grid over the colors. Only the cell edges are drawn, with no diagonals.
- **Blur Gradient**: smooths the colors inside each cell. Off shows one flat color per cell, so you can read the exact value of each cell. Not available in Aspect.
- **Show Colors**: turn it off to show your original objects normally.
- **North Arrow**: shows a north arrow at the bottom right of the viewport. On by default, and it is included in exported images.

## EXPORT

**Image Size** sets the size of the exported PNG: Viewport x2, Medium (1600 px) or Large (2048 px). The height follows the shape of the active viewport. **Export PNG** is at the bottom of the window. The image includes the legend, the statistics and the north arrow.

## What Bake creates

Bake creates real geometry from what is currently shown, on a sublayer `zAnalyzeTerrain::Elevation`, `zAnalyzeTerrain::Slope` or `zAnalyzeTerrain::Aspect`. Each bake is grouped automatically.

- **Elevation**: a colored mesh if **Show Colors** is on, and contour curves with elevation text on the primary contours if **Contour Lines** is on. After baking, the primary contours stay thick in every display mode.
- **Slope**: a colored mesh.
- **Aspect**: a colored mesh, plus one curve per arrow if **Arrows on Surface** is on.
- Analysis points are baked too, as points, leader lines and text in the form `Name value`.

The window stays open after baking, so you can switch to another analysis and bake again.

## Good to know

- Selected objects are not painted, because Rhino's selection highlight covers the colors. The window deselects your objects when it opens.
- If you delete an analyzed object, its colors disappear.
- Running the command again does not open a second window. It brings the existing one to the front.
- A point's values are always its own real readings. Switching between Elevation, Slope and Aspect only changes which value is shown in the viewport label.
- **Esc** does not close the window. Use the X or **Close**.
- The window width is fixed and it cannot be made shorter than its default height.
- On a big site with many objects, a high **Detail** is noticeably slower, and so is a very small contour **Interval**. Start with a lower Detail and a larger Interval to see the whole picture, then refine.
- If the selected surface is perfectly flat, there is no meaningful color range.
