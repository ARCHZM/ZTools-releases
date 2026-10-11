# zAnalyzeDaylight

`zAnalyzeDaylight` uses the hour-by-hour sun data of a real EPW weather file to compute three LM-83 daylight metrics for a set of faces: **sDA** (spatial daylight autonomy), **ASE** (annual sunlight exposure) and **UDI** (useful daylight illuminance). It handles occluders, reflectances and managing several targets at once.

Good for:

- Daylight compliance in green building certification such as LEED or WELL (sDA and ASE).
- Judging whether the natural light in an interior space is sufficient or excessive over the whole year.

## Quick start

1. Run `zAnalyzeDaylight` and pick one or more faces or surfaces as the analysis targets. To pick a single face inside a block, use Ctrl+Shift+click.
2. In **SETUP**, load an EPW weather file, set the occupied hours, the occluder mode and the reflectances.
3. In **METRICS**, choose sDA, ASE or UDI and adjust its thresholds.
4. In the **ANALYSIS OBJECTS** table, press **Compute** for each target, or for all targets at once.
5. In **DISPLAY**, adjust the heat map and the on-screen panels.
6. To produce images or data, set **EXPORT** and press **Export PNG** or **Export CSV** inside it, or press **Bake** at the bottom.
7. **Close** closes the tool. There is no persistent session, so every opening starts fresh, but the analysis settings and history stored on each target object are kept.

The dialog has two columns. The left column has **SETUP** (with Weather, Occluders and Reflectance sub-groups) and **METRICS**. The right column has **DISPLAY** (with a Viewport sub-group) and **EXPORT**. The **ANALYSIS OBJECTS** table runs across the top.

## SETUP > Weather

- **EPW File**: press **Browse...** and choose an EnergyPlus weather file (.epw). When loaded, it shows the city name with its latitude and longitude. If loading fails, an error is shown. There is no built-in fallback weather data, so you must choose a valid file.
- **Occupied Hours**: 0 to 24 hours, applied to every day of the EPW year. The LM-83 default is 8 am to 6 pm.

## SETUP > Occluders

- **Occluders**: **All** or **Pick**.
  - **All** (default): every visible object in the document except the analysis target itself blocks the sun, including locked objects (the lock does not matter). Hidden objects and objects on hidden layers do not count.
  - **Pick**: only the objects you pick block the sun. Switching to this mode starts the picking at once. Confirming with nothing selected clears the list, and pressing Esc cancels and returns to All.

## SETUP > Reflectance

Four reflectance values (0 to 1, in steps of 0.05), applied automatically by the orientation of the occluding surface:

- **Ceiling** (normal facing down): 0.8 by default.
- **Wall** (normal nearly horizontal): 0.5.
- **Floor** (normal facing up): 0.2.
- **Other** (anything that does not fit, such as a pitched roof): 0.35.

## METRICS

**Metric**: sDA, ASE or UDI, one at a time. sDA and ASE share the same full-year sun calculation, so switching between them needs no new calculation.

**sDA**

- **Threshold**: the illuminance threshold, 300 lux by default. An hour counts as good only when the illuminance reaches this value.
- **Occupied %**: 50% by default. A sample point passes when it reaches the threshold for at least this share of the occupied hours.
- **Pass %**: 55% by default. Used only for the overall verdict shown for the area. It does not change the point by point calculation.

**ASE**

- **Threshold**: the direct illuminance threshold, 1000 lux by default. Only direct sun is counted, not sky diffuse light or reflected light.
- **Max Hours**: 250 hours by default. A fixed number of hours per year, not a percentage.
- **Fail Max %**: 10% by default. Used only for the overall verdict shown for the area.

**UDI**

- **Lower**: 100 lux by default. Below this the point counts as insufficient.
- **Upper**: 3000 lux by default. At or above this the point counts as exceeded. The boundary between Supplement and Autonomous is fixed at 300 lux and cannot be changed.
- **Autonomous %**: 60% by default. Used only to draw a reference line on the HUD histogram. There is no industry standard default.

## DISPLAY

- **Gradient bar**: each metric keeps its own colors, so switching metrics does not overwrite them (unless you changed the current metric's gradient). The default sDA gradient is identical to the Viridis Cyan preset. Right-click the bar for the preset list, and hover over a preset to see its name. Presets you saved yourself can be deleted with right-click, Delete. Built-in presets cannot be deleted.
- **Grid Divisions** (on by default): the grid division lines.
- **Blur Gradient** (off by default): smooth colors by vertex interpolation, instead of one flat color per face.
- **Point Values** (off by default): shows the value next to each sample point.
- **Classification** (off by default): a debugging view that colors the occluding faces by their automatic class (Ceiling, Wall, Floor, Other), so you can check that the reflectance classes match what you expect.

### DISPLAY > Viewport

- **Basic HUD** (on by default): the main switch for the live information panel in the viewport.
- **Pooled** (off by default): see How the settings work together. Not available (grayed) in UDI mode.
- **Verdict Gauge** (on by default): the pass / fail gauge.
- **Histogram** (on by default): the distribution histogram.
- **North Arrow** (on by default).
- **Gradient** (on by default): the color legend in the viewport, controlled separately from Basic HUD.

## EXPORT

- **Image Size**: Medium (1600 x 1200), Large (2048 x 1536) or Custom (a W/H row appears, 1920 x 1080 by default).
- **Floating Viewport** (off by default): opens a separate floating viewport for composing the image.
- Buttons: **Bake** (bakes the heat map mesh), **Reset**, **Bake** and **Close** at the bottom, and **Export PNG** and **Export CSV** (the file is actually an .xlsx spreadsheet) inside the EXPORT card. There is no OK or Cancel button, and Close is the only way to close.

## ANALYSIS OBJECTS

Each row is one analysis target. A row has: the name (click to select and show it in the HUD, double-click to rename), a World / Pick grid plane switch (World is the default plane that fits the geometry vertices, and Pick asks you to pick an edge as the grid X direction), **Grid Density** (0 to 1, default 0.5, set per target), a **Compute / Stop** button, a status text, a history button (with a badge showing the number of records) and a drag handle for reordering.

**Delete and rename.** There is no separate delete column. Right-click a target name for the Rename / Delete menu (the hover color is light grey). A selected row is grey across the whole row with a thin dark frame. With a name selected, the Delete key also deletes. Click selects, Shift+click selects a range, Ctrl+click adds or removes one, and deleting several asks for confirmation. Double-clicking a name renames it.

Above the table there is a set of global controls: a global World / Pick (applied to all targets at once), clear all history (asks for confirmation), compute all / stop all, a global Grid Density, and the **+ Add Object** entry.

## How the settings work together

- **Metric switches the parameter group, the gradient and the displayed result together.** The sDA, ASE and UDI threshold groups are shown one at a time. Each metric keeps its own gradient. After switching, the most recent result of the selected target for the new metric is shown. If that target has never been computed for the metric, it clearly says "not computed yet" and does not leave the previous metric's result on screen.
- **Pooled (formerly "Building sDA") replaces the existing sDA / ASE overview numbers and gauge, it does not add a row.** When on, all targets that have been computed for the metric are combined into one overall percentage weighted by sample points (for example LEED judges a whole building or floor area together, not each face on its own). UDI has no pass / fail line to pool, so the switch is disabled in UDI mode. For ASE the pooled number is only for convenience, because LEED judges ASE room by room and not pooled. The tool does not claim LEED compliance for ASE just because Pooled is on.
- **Changing the occluder mode starts or clears the picking at once.** See above.
- **Grid Density is an exponential scale, not linear.** Slider 0 is about 0.002 points per unit area (very sparse) and slider 1 is about 20 points per unit area (fine). The first half of the slider covers preview-level densities and the second half reaches production-level densities, so that a face of common building size does not accidentally hit the internal limit of 250,000 sample points.
- **The Custom W / H row appears only when Image Size is Custom.**

## Good to know

- **Nothing can be computed without an EPW weather file.** Compute asks you to choose one first. There is no built-in default weather.
- **Picking a single face inside a block needs Ctrl+Shift+click.** If you click the whole block instance, a message asks you to pick again with the key combination.
- **Picking more than 20 targets at once asks for confirmation**, so a slip does not make the analysis slow.
- **A target that reports "no geometry"** usually has no sample points at the current grid density. Try a higher Grid Density, or check whether the face itself is degenerate.
- **In Pick occluder mode, if you picked nothing, Compute is blocked** and asks you to pick occluders first.
- **Export CSV creates an Excel (.xlsx) file.** If the export fails (for example the file is open in Excel), a message tells you the exact reason. It does not fail silently.
- **If you explode a block that carries analysis records,** the name, settings and history are usually kept, but you must press Compute again to see results (the colored mesh and the per-point values are not carried over). **If the block contained more than one analyzed face, there is a small chance that restored settings land on the wrong face after exploding**, more likely with complex or deeply nested blocks. When results do not match, check each face's settings before computing again.
- **Point Values and Classification are debugging views.** They affect only the preview and never the calculation.
- **An analyzed object that is hidden is still saved correctly** (it is briefly shown and hidden again during saving, usually unnoticed). If another action interrupts the save, that setting may not be saved.
