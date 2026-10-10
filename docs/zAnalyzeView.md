# zAnalyzeView

`zAnalyzeView` measures how visible the outdoors is from a floor or a desk, and how open the space around each point is. It casts a 360 degree fan of horizontal rays from a set of sample points (an **isovist** analysis). It is useful for checking whether a workstation can see out of a window, how open a room in a floor plan is, or for quickly comparing the visible area of different layouts at the setback or atrium design stage.

One analysis can include several sample faces. Each face is calculated on its own, and each can use its own boundary curve. Two complementary metrics are provided: **Outdoor Visibility** and **Openness**.

## Quick start

1. Run `zAnalyzeView`. In the viewport, select one or more sample faces (a floor or a desk, or a face inside a block) and press Enter. The dialog opens and each selected face becomes a row in the **ANALYSIS OBJECTS** list. Several faces of the same block can be added together, each with its own sample grid, density, name and history.
2. Set the boundary curve. Click the **Boundary** icon in the global controls row at the top of the list (the Add Object row) and pick one closed planar outline curve of the building. It applies to every object. If one object needs a different boundary, click the Boundary icon on that object's own row and pick a curve just for it (it overrides the global one).
3. In **SETUP**, choose the **Metric** (Outdoor Visibility or Openness) from the dropdown, set **Elevation**, **Rays** and **View Range**, and for Outdoor Visibility also **Threshold**. Then choose the **Occluders** mode: **All** (every visible object except the sample face blocks the view) or **Pick** (only the objects you pick block the view).
4. Back in **ANALYSIS OBJECTS**, set **Density** and **Plane** for each row (or use the global row at the top), then press **Compute** on the row (or the global Compute). The calculation runs in the background, a ring shows progress, and you can **Stop** at any time.
5. When the calculation finishes, use the gradient bar in **DISPLAY** to set the colors. **Grid Divisions**, **Blur Gradient**, **Point Values** and **Color Occluders** control the details. The VIEWPORT sub-group switches Basic HUD, Gradient, Verdict Gauge, Histogram and North Arrow.
6. In **ISOVIST**, press **Pick** and click any sample point in the viewport to show that point's own isovist outline. The value of that point is shown at the top left of the dialog. Pick again for another point, or **Unpick** to clear.
7. **Export Isovist** in the EXPORT card turns the isovist of the point you picked into real curves (the boundary polygon plus every ray), on a separate Isovist sublayer. It does not enter picking mode again.
8. **Bake** keeps the analysis result as real geometry. To only preview, press **Close** (closing the dialog leaves no geometry behind, and the preview meshes are removed).
9. To export an image, choose the **Image Size** in **EXPORT** (or Custom Size), optionally turn on **Floating Viewport** to compose the view, and press **Export PNG**.

The top row of the dialog shows the value of the currently picked isovist on the left and the number of **Participating Occluders** on the right. Below it is the **ANALYSIS OBJECTS** list across both columns. The left column has **SETUP** and **ISOVIST**, and the right column has **DISPLAY** and **EXPORT**.

## ANALYSIS OBJECTS

- **Object** (row name): the name of each sample face. It is numbered Object 1, 2, 3 by default. Double-click or right-click, Rename to change it.
- **Density**: the density of this face's own sample points, a slider from 0 to 1. A higher value gives more points and a slower calculation.
- **Plane**: **World** (sample on the world horizontal plane) or **Pick** (pick an edge, and that edge becomes the X axis of a local construction plane for this sample face).
- **Boundary**: a single icon. Click it to pick a boundary curve just for this object, overriding the global one, without affecting other objects. The curve must be closed and planar.
- **Compute / Stop**: starts or stops the calculation for this row only. A ring shows the live progress.
- **History**: the most recent calculations of this row (up to 20). Click one to restore it.

**Select, rename and delete.** There is no separate delete column. Click a name to select it and show that object's HUD. Shift+click selects a range and Ctrl+click adds or removes one, and selected names have a light red background. Right-click a name for the Rename / Delete menu (the hover color is red). With a name selected, the Delete key also deletes. Deleting several objects asks for confirmation. When you delete an object from the list, its sample grid and preview mesh are removed from the viewport too.

When a column name in the header is cut off by the column width (for example Boundary shown as B...), hover over the header to see the full name. The global controls row at the top has matching icons: a global Density, a global Plane (World or Pick for all objects at once), a global Boundary (picks one curve for all objects and clears each object's own override), clear all history, and a global Compute / Stop.

## SETUP

- **Metric** (dropdown):
  - **Outdoor Visibility**: around each point, the share of directions that really lead outdoors (0 to 100%).
  - **Openness**: the area of each point's own isovist polygon divided by the area of a disc whose radius is the View Range, then square-rooted, giving an equivalent sight-distance ratio of 0 to 100%. It means "how far can this point see on average, as a share of View Range" (100% means it can see the full View Range in every direction). The square root spreads the values out, so most points do not crowd into the low percentages. It reflects only how open the space is, and has nothing to do with indoors or outdoors. See How the settings work together for the full difference.
- **Elevation**: how far the sample points are raised along world Z, to imitate eye height. 0 to 10, default 5. The isovist outline you pick and the curves you export also start at this raised height.
- **Rays**: how many rays are in each sample point's 360 degree horizontal fan. Default 144. More is finer and slower.
- **View Range**: the farthest a single ray goes before it is cut off. At most 3000, default 1000, whole numbers only. While you drag or type this value, a circle with a radius equal to it is shown briefly in the viewport (red outline, 90% transparent red fill), centered on the geometric center of the combined bounding box of all objects. The circle disappears 0.5 seconds after you release the control or confirm the input.
- **Threshold**: the pass / fail line of the Verdict Gauge (0 to 100, default 50). Works for both metrics: an object passes when its Avg value is at least the Threshold. Both Outdoor Visibility and Openness are percentages from 0 to 100%.

### SETUP > OCCLUDERS

- **Occluders**:
  - **All**: every visible piece of geometry in the viewport except the sample face itself takes part in blocking the rays.
  - **Pick**: only the objects you pick take part. Everything else is ignored.
  - **Participating Occluders** at the top right of the dialog shows how many objects actually take part right now.

## ISOVIST

- **Point**: the title on the left, with two equal-width buttons **Pick** and **Unpick** on the right, as wide as the Color swatch below. **Pick**: click a sample point in the viewport to show its own isovist outline (the ray fan plus a semi-transparent filled boundary polygon). It stays until you pick another point or press **Unpick**. There is no separate display switch, picking shows it. **Unpick** clears the picked point.
- **Color**: one swatch controls the isovist boundary line, its 10% transparent fill and the rays (the ray color is a semi-transparent version of the same color). Double-click or right-click the swatch to open the color picker. The default is the ZTools ink red. Changes are saved and **Reset** restores the default.

## DISPLAY

- **Gradient bar**: double-click to add or remove a color stop, right-click a stop to change its color, and right-click the bar for the preset list (hover over a preset to see its name). Presets you saved yourself can be deleted with right-click, Delete. Built-in presets cannot be deleted. Outdoor Visibility and Openness each have their own completely independent gradient bar. The default for Outdoor Visibility is **Enclosed to Sky** (dark purple, coral, orange, pale yellow to sky blue, where dark means enclosed and light means open). The default for Openness is **Rainbow**.
- **Grid Divisions**: draws the sample grid lines over the colors.
- **Blur Gradient**: on, the colors blend smoothly inside each cell. Off (the default), each cell is one flat color with clear edges, so you can read the exact value of each cell.
- **Point Values**: shows the metric value of each sample point as text at its position. Cells that were removed because an occluder overlaps them (see Good to know) show no value.
- **Color Occluders**: when on, paints every object that actually takes part in blocking the rays the same fixed blue, as a temporary viewport preview, so you can check which objects count as occluders. It does not change the real color or layer of those objects.

### DISPLAY > VIEWPORT

- **Basic HUD**: the text panel at the top right of the viewport (Object, Points, Avg, Min, Max, with bold labels on the left and aligned values on the right, laid out like the other Analyze tools such as `zAnalyzeDaylight`). The title shows Analyze Outdoor Visibility or Analyze Openness depending on the Metric. It is also included in exported images. Before any calculation the panel shows only the title and the Object row.
- **Gradient**: the gradient legend bar at the bottom left of the viewport, also included in exported images.
- **Verdict Gauge**: a pass / fail dial that compares the selected object's Avg value with the Threshold. Available for both metrics. For Openness it reads as "how much of the whole View Range disc is open on average".
- **Histogram**: the distribution of values over all sample points of the selected object, in 5 equal bins (a fixed 0 to 100% for both metrics). Each bin takes its color from the current gradient, with no pass / fail coloring.
- **North Arrow**: a north arrow in a corner of the viewport, also included in exported images.

## EXPORT

- **Image Size**: the presets Medium (1600 x 1200) and Large (2048 x 1536), or Custom to set the width and height yourself.
- **Custom Size**: shown only when Image Size is Custom. The width and height of the exported image in pixels.
- **Floating Viewport**: opens a separate floating viewport window whose size is exactly the current Image Size, so you can compose the picture without touching the main viewport. Turn the checkbox off to close it.

## Buttons at the bottom

- **Export PNG**: opens a save dialog and saves the active viewport as a PNG at the current Image Size (or Custom Size). The VIEWPORT checkboxes decide whether the Basic HUD, Gradient and North Arrow are shown.
- **Export Isovist**: turns the isovist of the picked point into real curves (the boundary polygon plus every ray, without the semi-transparent fill) on the Isovist sublayer. If you have not picked a point yet, it asks you to pick one first.
- **Bake**: keeps the colored result of every computed object as real geometry.
- **Reset**: restores all settings to their defaults.

## How the settings work together

**Outdoor Visibility versus Openness.** Both come from the same ray fan, but they measure different things.

- **Outdoor Visibility** measures "of all directions around this point, what share really leads outdoors". Each ray finds its stop point as normal (it stops where it hits an occluder, or is cut off by View Range if it hits nothing), and the only question is whether that stop point lies inside the Boundary curve. Inside means indoor, outside means outdoor. The share of outdoor rays among all rays is a percentage from 0 to 100%. View Range is only the farthest a ray may travel, and does not take part in deciding indoor or outdoor. It answers "is this point open and does it have a clear line of sight".
- **Openness** measures "how large an area of space can this point see". It joins the end points of all rays (whether they hit an occluder or were cut off by View Range) into a closed polygon, divides the area of that polygon by the area of a disc with radius View Range, and takes the square root. This gives an equivalent sight-distance ratio from 0 to 100% that does not care whether the space is indoors or outdoors. It purely says how open or roomy the space is. Because the denominator is the View Range disc, the same position gives a different percentage when you change View Range. It is not a strict area share: a point whose area is 4% of the disc shows about 20%, and 25% shows 50%.
- The two can disagree completely. A huge atrium that is fully roofed can have a large Openness but 0% Outdoor Visibility (every ray is blocked by the roof and ends inside the Boundary). A tiny open courtyard surrounded by tall walls but open to the sky can have a small Openness but 100% Outdoor Visibility (every ray ends outside the Boundary). This is why the two metrics have separate gradient bars. Both are 0 to 100%, so both work with the Verdict Gauge and Threshold, and the legend and histogram bins both use a fixed 0 to 100%.

**The value at the top left of the dialog** is the metric value of the point you picked with Pick: the outdoor visibility percentage in Outdoor Visibility mode, or the openness percentage in Openness mode. It updates at once when you switch Metric, and goes blank after Unpick or after Compute clears the isovist.

- Occluders All or Pick decides which objects block the rays. Density affects only how dense the sample points of one sample face are, and does not change the direction or number of rays (that is set globally by Rays).
- The boundary curve of each object comes from the global Boundary icon or its own Boundary icon. Clicking the global Boundary icon again clears every object's own override and replaces them all with the newly picked curve.
- The size of the Floating Viewport follows Image Size or Custom Size automatically.
- Below the HUD panel in the viewport, the Verdict Gauge and the Histogram are stacked in that order. A warning shows as a small red dot at the bottom of the whole HUD group (hover to expand the text) and never covers the gauge or the histogram.

## Good to know

- **Closing the dialog (Close or the X) keeps no preview that was not baked.** All preview meshes that were not baked are removed, and only the results you explicitly baked stay in the document.
- **The isovist outline from Pick or Export Isovist is the full, untrimmed version.** Every ray is drawn to its own stop point and is not shortened depending on whether the end lies inside or outside the Boundary. The indoor / outdoor test happens only in the statistics and does not change the outline you see.
- **A sample point at the same height as the face of an occluder (within tolerance, for example the bottom face of a column standing on the floor) is treated as overlapping an occluder** and is left out of Avg, Min and Max. That cell of the preview mesh is really removed (a hole, not painted gray), and Point Values does not show it. The same rule applies in `zAnalyzeDaylight`.
- **If the sample face is itself a member of a block, only that member is excluded from the occluders.** Other objects in the same block (for example surrounding building masses) still block as normal. Several faces of one block can be added as targets together, and each shows its own sample grid.
- **Computing an object again clears the picked isovist if it belongs to that object.** After a recalculation the number and order of the sample points can change, so the old pick is no longer valid. Pick again.
- **A boundary curve that is not closed and planar is refused**, and you are asked to pick again. If an object has no boundary of its own and no global boundary has been picked, Compute asks you to pick a boundary first.
- **With Occluders set to Pick and nothing picked,** the calculation is blocked with a HUD message. Pick at least one occluder, or switch back to All.
- **Openness is relative to the View Range disc.** After you change View Range, the Openness values already computed do not update automatically, and you must Compute again, otherwise they no longer match the new radius.
- **Color Occluders is only a temporary preview.** It does not change the real colors of the objects and is not baked. It disappears when you turn it off or close the dialog.
- **A gradient you saved earlier overrides the new default.** To go back to the default, press **Reset**, or right-click the gradient bar and choose the preset.
