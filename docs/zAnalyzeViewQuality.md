# zAnalyzeViewQuality

**This tool answers one question: standing in different places in a room, where is the view outside worth more?**

You tell the tool which things in the scene are worth looking at, for example park trees, a river or a landmark building, and give each one a score (the higher, the more worth seeing). The tool then stands on every point of the floor or desk you pick, looks around in a full circle, counts which scored things that point can see, and gives the point a score from 0 to 100. The result is drawn as a color heat map: the warmer and lighter the color, the better the view from that position.

Typical uses:

- Compare which workstations or rooms on a floor have a better view, as a basis for pricing or seating.
- Judge how much view a new building blocks for others, or whether your own window direction wastes a good view.
- Make a view quality picture that can go straight into a presentation.

It is a sister tool of `zAnalyzeView` and shares the same dialog. The list, the heat map, the HUD and the export work exactly the same way. The only difference is what is calculated: `zAnalyzeView` measures "how much of the line of sight leads outdoors", and `zAnalyzeViewQuality` measures "how many points the things you can see are worth". If you have never used `zAnalyzeView`, the steps below are complete.

## Quick start

A complete analysis from scratch takes about five minutes.

**Step 1: choose the floor to analyze.** Run `zAnalyzeViewQuality`. When the viewport says "Select sample face(s)", select the floor or desk you want to judge (for example the slab of a ground-floor office) and press Enter. These faces are called **sample faces**. The tool lays a grid of small points on each of them, and each point is one place to look from. You can select several faces. Each face becomes one row in the list at the top of the dialog.

**Step 2: tell the tool what is worth looking at.** In **SCORED OBJECTS** on the right, press **Open Scored Objects...** to open the scoring window, click **+ Add Object** on the first row of the list, and in the viewport select the things to score (trees, a landmark building, water and so on, several at once if you like). Press Enter. If something is already selected in Rhino, it is used directly. Each thing appears in the list with an initial score of 5. The Score column holds the score, and you can click the color block to change it, or select several rows and press a preset button at the bottom to set them at once. A rough guide:

| Thing | Suggested score |
|---|---|
| A very beautiful park, water or landmark | 8 to 10 |
| Ordinary trees, green space | 4 to 6 |
| An ordinary building facade | 1 to 3 |
| Things you want to count against the view, such as a parking lot or a plant room | A negative number, such as -3 |

Scores are compared only among the things you score, so they do not need to be precise. What matters is their relative size. Things you do not score give neither points nor penalties (see Scoring rules below).

**Step 3: check the Sky Score.** **Sky Score** on the left, in SCORING, is the score for "a line of sight that hit nothing and goes straight out into the distance", on the same scale as the object scores, 4 by default. The higher it is, the more an open view scores. If it equals the score of your best thing, an open sky is as good as that thing, and the whole map flattens toward it. The top of the SCORING box has a small table that updates live with your settings and shows the distance range and the factor for near, mid, far and sky.

**Step 4: set the standing height and the view range.** In **SETUP** on the left:

- **Elevation**: the standing height, that is how high the eyes are above the slab. Default 5. Use the eye height at a desk or standing.
- **# of Rays**: how many lines of sight in a full circle. Default 144. More is finer and slower.
- **V Range** and **V Samples**: how wide to look up and down. Default 60 degrees in total, with 3 angles. Raise them for a finer look, or set V Samples to 1 to look only at the horizontal circle.
- **View Range**: how far the eye can see. A line of sight that travels this far without touching anything counts as "sky".

**Step 5: choose what blocks the view.** **Occluders** in SETUP has two choices, All and Pick. **All** (the default) lets everything visible in the scene block the view, which is closest to reality. **Pick** lets only the things you pick block the view.

**Step 6: compute.** In the list at the top of the dialog, press the triangle button (Compute) at the far right of a row, or press the triangle on the global row at the very top to compute all rows at once. A progress ring shows while it runs, and you can press the square button to stop.

**Step 7: read the result.** When it finishes, a color heat map appears in the viewport, and the HUD at the top left shows the average, lowest and highest score of this face. The **Metric** in SETUP switches what the heat map shows (Quality Score, Sky View Factor, Scored View Factor). **Range**: with **Auto**, the colors stretch from the lowest to the highest value over all sample faces so small differences are visible, and the legend shows the real limits. With **Fixed**, the range is a fixed 0 to 100%. To see why one point got its score, press **Pick** in RAYS and click that point in the viewport, and every line of sight from it is drawn. For a summary table of all sample faces, press **Results...** in EXPORT. In DISPLAY, tick **Point Values** to see the exact number in every cell, and use the **Verdict Gauge** to see whether it passes (the **Threshold** in SETUP is the pass line).

**Step 8: save or export.** To keep the result in the file, press **Bake**. To make a picture, choose the image size in EXPORT and press **Export PNG**. To keep nothing, press **Close**: the preview that was not baked is removed, but the scores you gave the objects are saved and are read back the next time you open the tool.

## The dialog

The top of the dialog is **ANALYSIS OBJECTS** (the faces to analyze), across both columns. Below it, the left column has **SETUP** and **SCORING**, and the right column has **SCORED OBJECTS**, **DISPLAY** and **EXPORT**.

## ANALYSIS OBJECTS

This is exactly the same as in `zAnalyzeView`: one row per sample face. Click a name to select it, right-click a name for Rename or Delete, press Delete to delete, and hold Shift or Ctrl to select several. Each row also has:

- **Dens.** (Density): how dense the sample points are, 0 to 1. A larger value gives denser points and a slower calculation.
- **Plane**: the direction of the sample grid. **World** aligns to the shape of the face itself, **Pick** asks you to pick an edge that becomes the horizontal axis of the grid.
- **History**: the most recent calculations of this face. Click one to restore it.
- **Compute**: the triangle starts the calculation, and during it becomes a progress ring with a square button beside it to stop.

## SETUP

- **Metric**: what the heat map, HUD, histogram and point values show. **Quality Score** is the normalized score from 0 to 100. **Sky View Factor** is the share of the view that is sky. **Scored View Factor** is the share of the view occupied by scored things. The shares are weighted by solid angle and count all lines of sight (including those blocked by unscored things and those pointing down at the ground), so sky, scored, skipped and ground add up to 100%. The Verdict Gauge and Threshold work only for Quality Score, and Threshold is grayed out for the other metrics.
- **Range**: how values map to colors. **Auto**: the colors stretch from the lowest to the highest value over all sample faces, to compare rooms. **Fixed**: a fixed 0 to 100%.
- **Elevation**: the height of the eyes above the slab, 0 to 10, default 5.
- **# of Rays**: the number of lines of sight in a horizontal circle, default 144.
- **V Range**: the total opening angle up and down, in degrees, centered on the horizontal. Default 60, which is 30 degrees up and down. 0 means horizontal only.
- **V Samples**: how many angles are taken within the up and down range. Default 3. The total number of rays is # of Rays times V Samples. 1 means the horizontal circle only.
- **View Range**: how far a line of sight can go. A line that travels farther without touching anything counts as sky. While you drag this number, a circle is shown briefly in the viewport so you can see how large the range is.
- **Threshold**: the pass line, 0 to 100, default 50. It is used by the Verdict Gauge in the HUD: a face passes when its average score is not below it.
- **Occluders**: **All**: everything visible in the scene blocks the view, including unscored buildings. **Pick**: only the things you pick block the view. Scored things always take part, whichever mode you choose.

## SCORING

- **Sky Score**: the base score of a line of sight that hits nothing and goes straight out (horizontal or upward). Default 4. It is on the same scale as the object scores: the higher it is, the more an open view wins. If it is as high as the best object, the whole map tends toward 100. Lower it to bring out the differences between objects.
- **Near Distance / Mid Distance**: the two cut-off points that divide distance into near, mid and far (in model units). Defaults 25 ft and 100 ft.
- **Near Weight / Mid Weight / Far Weight**: a discount factor for each of the three distance bands. Defaults 0.5, 0.8 and 1.0. The closer the thing you see, the bigger the discount (a view right in your face is not a good view), and the farther it is, the closer to the full score. Sky always uses the Far factor.

Example: a tree scored 8 that is seen 60 ft away is in the mid band and scores 8 x 0.8 = 6.4. The same tree 15 ft outside the window scores 8 x 0.5 = 4.

## SCORED OBJECTS

The dialog shows only a one-line summary (how many objects, the average score and the range) and the **Open Scored Objects...** button. The full list is in a separate window. The list in the scoring window has the same style as the lists of `zAnalyzeDaylight` and other tools: a header, divider lines, a name cell that turns light red and bold when selected, and 24 px icon buttons.

- **+ Add Object**: the first row of the list, like the ANALYSIS OBJECTS list of `zAnalyzeView`. Click it to go back to the viewport and select faces, polysurfaces, meshes, extrusions, SubDs or blocks, several at once. Things already selected in Rhino are used directly. Things already in the list are not added again. A newly added thing has score 5 and no tag, and you adjust it later with the color block, the preset buttons or the right-click menu.
- **Link selection**: on by default and remembered. When on, selecting a row selects the matching object in Rhino, and selecting objects in Rhino selects the matching rows. The search box at the top right filters by name.
- **The list**: each row has a drag handle, a name, a Score color block and a Del button.
  - Click a name to select it and zoom to the object. Ctrl or Shift to select several. Double-click a name to rename it (this changes the real name of the Rhino object). Right-click a name: Rename, Delete, Group as Tag..., Move to a Tag, Remove from Tag, Select All, Invert Selection.
  - The Score block: its color matches the viewport coloring. Click it to turn it into an input box, press Enter or click elsewhere to confirm and Esc to cancel. Scores range from -1000 to 1000, and a negative score marks an unpleasant sight.
  - The drag handle: hold and drag onto another row and release to change the order. The list follows the order you drag it into, the order is stored on the objects, and it is restored the next time you open the tool.
  - Del: removes the object from scoring. The object itself stays in the model.
  - Objects deleted in Rhino disappear from the list automatically.
- **Tag (grouping)**: puts a set of objects (for example all trees) under one name, shown folded in the list. A Tag belongs only to this tool. It does not change Rhino layers, and does not affect the calculation or the viewport coloring.
  - To create one: select some rows and right-click, **Group as Tag...**, which creates a new Tag and lets you type its name. Or right-click, **Move to** an existing Tag. Or drag the handle of a row onto a Tag row. There is only one level, no nesting.
  - A Tag row has no background color. It is told apart by bold text and a triangle, with a small gap between groups and tree connector lines in front of the children. Click the name to fold or unfold (the number in parentheses is the object count), and double-click to rename. A Tag row has no Del icon. Right-click it for Rename Tag, Delete Tag and Select Children (selects and zooms to the whole group). Delete Tag removes only the grouping, and the objects inside go back to having no tag and keep their scores. Drag the handle of a Tag row to move the whole group.
  - Objects without a Tag are listed at the top. The Tag names and the group order are stored on the objects. The folded state is not stored, and the groups are unfolded every time. An empty Tag with no objects disappears when you close the window.
- **Preset buttons -3 / 1 / 3 / 5 / 8 / 10** at the bottom: set the selected rows to that score (-3 unwanted view, 1 poor, 3 average, 5 good, 8 very good, 10 landmark).
- **Viewport coloring**: while the window is open, all scored objects are temporarily colored by score: light green for 0 to dark green for 10, red for negative, with the score shown on the ticked objects. It disappears when you close the window, and it does not change the real color of the objects. The scores are stored on the objects themselves, so they are restored when you open the tool again.

## RAYS

- **Pick / Unpick**: press Pick and click a sample point on the heat map in the viewport. Every line of sight from that point is drawn. **Unpick** clears it. **Green** is a line that hit a scored object (the darker, the higher the score). **Blue** is a line that went to the sky (only the first stretch is drawn). **Gray** is a line that hit something unscored (skipped, not counted). **Brown** is a line pointing down that hit nothing (treated as ground, skipped). The status bar at the top also gives the quality score of that point, the shares of sky, scored, skipped and ground, and the object with the largest share.

## DISPLAY

- **Gradient bar**: decides how scores map to colors. The default is **Indigo to Amber Sky**: dark indigo means a poor view and a pale warm color means a good view. Double-click the bar to add or remove a color stop, and right-click the bar for presets (hover over a preset to see its name).
- **Grid Divisions**: draws the sample grid lines over the heat map.
- **Blur Gradient**: on, the colors blend between cells. Off (the default), each cell is one flat color, which makes it easier to read the exact value of each cell.
- **Point Values**: shows the score (0 to 100) at the center of each cell.
- **Color Occluders**: when on, paints every object that actually blocks the view the same fixed blue as a temporary preview, so you can check which objects count as occluders. It does not change their real colors and is not baked.

### DISPLAY > VIEWPORT

- **Basic HUD**: the information panel at the top right of the viewport (Object, Points, Avg, Min, Max), which is included in exported images. Before computing, it shows only the title and the Object row.
- **Gradient**: the gradient legend at the bottom left of the viewport, also included in images.
- **Verdict Gauge**: a pass / fail dial that compares the average score of the face with the Threshold.
- **Histogram**: the distribution of scores over all points of the face, in 5 fixed bins (0 to 100).
- **North Arrow**: a north arrow, included in images.

## EXPORT

- **Image Size**: a preset image size, or Custom to set the pixel width and height.
- **Floating Viewport**: opens a floating viewport with exactly the size of the exported image, for composing the picture.
- **Results...**: opens the results table, with one row per sample face (points, the average, lowest and highest quality score, the sky share, the scored object share, and the share of the area whose quality score reaches the Threshold). The last row is the total over all sample faces (combined by the number of sample points, the same as the Pooled option of `zAnalyzeDaylight`). At the bottom of the window, the total "share of the area that passes" is compared with a fixed 75% and shown as Pass or Fail. **Export CSV...** saves the table as a CSV file.

## Buttons at the bottom

- **Reset**: restores all settings to their defaults (your scores are not affected).
- **Export PNG**: saves the viewport as a PNG at the current size.
- **Bake**: keeps the heat map as real geometry in the file.
- **Close**: closes the dialog.

## Scoring rules in plain words

For every sample point, the tool sends out many lines of sight around it. Each line meets one of three cases:

1. It hits something you scored. The line gets "that object's score x the distance factor".
2. It hits something you did not score, such as an ordinary building that blocks the view. This line is void. It adds nothing and subtracts nothing, so it does not pull the average down.
3. It hits nothing, or flies out past View Range. If the line is horizontal or pointing up, it counts as sky and gets "Sky Score x the Far factor". If it points down, it is treated as unmodeled ground in the scene, and is skipped like an unscored object, and does not enter the average.

All valid lines are then averaged (each line is weighted by the cosine of its elevation angle, that is by the solid angle it really covers, so a horizontal line weighs more than a line 30 degrees up or down). The result is the raw score of the point.

**Why the final range is 0 to 100.** The raw score is converted to 0 to 100 by "the most your scores and factors could possibly give". So every point's score means "how close this is to the best possible view". Results of different faces and different runs can then be compared directly, and the Verdict Gauge and the Threshold make sense.

**The relative sizes of the scores matter more than their absolute values.** If you scale all object scores and the Sky Score up or down together, the heat map does not change, because the result is converted again at the end. What really counts is the ratio between them.

**Relation to `zAnalyzeView`.** The two tools share the dialog, but each saves its own settings and the data stored on objects (density, name, history, scores), so they do not overwrite each other. The same face can be analyzed with both tools in turn.

## Good to know

- **Compute does nothing, and the HUD says to add scored objects first.** There are no objects in SCORED OBJECTS yet. Select the things to score in Rhino, then press + Add Object in the scoring window.
- **The whole map is almost one color.** Most likely the Sky Score is much higher than all the object scores, so most lines count as sky and the result is close to 100. To bring out the object differences, lower the Sky Score or raise the object scores. If instead almost everything is the lowest score, check whether most lines are blocked by unscored buildings, or whether View Range is too small.
- **Why some cells are empty (a hole).** Two cases: no line of sight of that point counted (all were blocked by unscored things), or the point lies where the bottom face of an occluder overlaps it (for example a column standing on the floor). These cells are left out of the average and show no value.
- **I scored something but it had no effect.** Check whether it is visible in the viewport (hidden objects and objects on hidden layers do not block), or turn on Color Occluders to see whether it counts as an occluder. Also remember that a line of sight must really see it: farther than View Range counts as not seen.
- **I changed View Range, a score or a factor, and the result did not change.** Results already computed do not update by themselves. Press Compute again.
- **Does the sample face block the view?** No. A face chosen as the sample face does not block its own view and is not scored twice.
- **What does closing the dialog lose?** The heat map preview that was not baked is removed. The scores you gave objects are stored on the objects, and are read back when you open the tool again, so they are not lost.
- **Color Occluders is only a preview.** It does not change the real color of any object and is not baked. It disappears when you turn it off or close the dialog.
- **Looking outside from an indoor room.** There is no separate glass setting. Hidden objects and objects on hidden layers do not block, so before the analysis hide the window glass (and anything you do not want to block), or switch its layer off, and the lines of sight pass through the opening. Walls and slabs block as usual, and lines that hit them are skipped as "unscored things".
- **Results shows `-` for Sky and Scored.** These two are only available for results computed since you opened the dialog this time, not for results restored from history. Compute again to fill them.
