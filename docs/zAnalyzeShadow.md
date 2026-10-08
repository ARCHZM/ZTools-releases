# zAnalyzeShadow

`zAnalyzeShadow` uses real latitude, longitude and sun position to analyze shadow coverage for a set of roofs or site boundaries. It supports three study ranges, a single day, key solstice and equinox days, or the whole year. It can export images with a heat map, CSV data and an animated GIF.

Good for:

- Judging how much an area is shaded over the year or on specific dates (for example choosing a solar panel location or checking a courtyard).
- Producing shadow study images and animations.

## Quick start

1. Run `zAnalyzeShadow` and pick one or more closed planar curves as the analysis boundaries.
2. In **SETUP**, set the study range, the date or hours, and the latitude and longitude.
3. In **SHADOW**, adjust the shadow quality and the pass threshold, with a live preview.
4. In **DISPLAY**, adjust how the heat map is shown.
5. To export, set **EXPORT** (image size and so on) and press **Export PNG** (this also makes a GIF) or **Export CSV**.
6. **Cancel** closes the tool. There is nothing to save, because the tool has no persistent session.

The dialog has two columns: **SETUP** on the left, **SHADOW**, **DISPLAY** and **EXPORT** on the right. The **ANALYSIS OBJECTS** table of boundaries runs across the top.

## ANALYSIS OBJECTS

A table of boundaries. You can reorder by dragging, or sort by clicking the Boundary column header. The heat map and the coverage shown follow the currently selected row.

**Delete and rename.** There is no separate delete column. Right-click a boundary name for the Rename / Delete menu (the hover color is red). With a name selected, the Delete key also deletes. Click selects, Shift+click selects a range, Ctrl+click adds or removes one, and deleting several asks for confirmation. Double-clicking a name renames it.

## SETUP

- **Study Range**: **Single Day**, **Key Dates** or **Yearly**.
  - **Single Day**: analyzes one chosen day. A Date picker is shown.
  - **Key Dates**: analyzes the spring equinox (3/20), summer solstice (6/21), autumn equinox (9/23) and winter solstice (12/21) of the current year. No Date field is shown. The dates are worked out each time from the current system year, so an old date from an earlier analysis is never reused.
  - **Yearly**: samples one fixed time on every day of the year. Only an Hour field is shown (9:00 by default), and Hour Range, Frequency and Preview Hour are hidden.
- **Date**: shown only in Single Day mode.
- **Hour Range**: a two-handle slider, 0 to 24 hours in steps of 0.5, labeled in 12-hour style (for example 8AM-6PM).
- **Frequency**: every 15 minutes, 30 minutes, 1 hour or 2 hours.
- **Preview Hour**: a live preview slider, independent of the export settings. It is clamped to the Hour Range automatically.
- **Latitude / Longitude**: taken from the document's Earth Anchor Point first, and you can edit them by hand. Whichever was changed last wins. The **Paste** button next to them parses a `latitude, longitude` text copied from Google Maps. The time zone has no setting of its own. It is always worked out from the longitude (one hour per 15 degrees, rounded), and does not know about daylight saving time or political boundaries.
- **Min Sun Hours**: 3.0 hours by default. The test for one sample point to count as good: the length of the selected Hour Range minus the shaded time must be at least this value.

## SHADOW

- **Sharpness**: **Low**, **Medium** or **High** (from low to high video memory use). High by default.
- **Intensity**: 0 to 100, 65 by default.
- **Color**: black by default. Double-click or right-click the swatch for the system color picker.
- **Pass %**: 0 to 100, 80 by default. The target line for the overall verdict: at least this share of the sample points must meet Min Sun Hours for the result to pass.

## DISPLAY

- **Gradient bar**: double-click to add or remove a color stop, right-click a stop to change its color, and right-click the bar to choose a preset. Hover over a preset to see its name. Presets you saved yourself can be deleted with right-click, Delete.
- **Grid Divisions** (on by default): overlay the grid lines on the colors.
- **Map Result**: the single global switch for the heat map. It follows the currently selected boundary row, not one switch per row.
- **Blur Gradient** (off by default): when on, colors blend by vertex interpolation. When off, each face has one flat color.
- **Highlight Cells** (off by default): paints each cell green or red by Min Sun Hours. This affects only the display and does not change the Compute result or the Pass % verdict.
- **Verdict Gauge** (on by default): the pass / fail gauge for the overall verdict.

## EXPORT

- **Image Size**: Medium (1600 x 1200), Large (2048 x 1536) or Custom (a W/H row appears, 1920 x 1080 by default).
- **North Arrow / Basic HUD / Gradient** (all on by default): whether the exported image includes the north arrow, the information panel and the color legend.
- **GIF Delay**: 20 to 500 ms, 200 ms by default.
- **Floating Viewport** (off by default): opens a separate floating viewport at the export resolution, for composing the image.

## How the settings work together

- **Study Range decides which time fields are visible.** Key Dates and Yearly do not read the Date field and work out their own dates. Yearly also hides Hour Range, Frequency and Preview Hour, and keeps one Hour field.
- **Preview Hour narrows with the Hour Range** and never points outside it.
- **Changing Sharpness rebuilds the native shadow map** and refreshes the preview.
- **Export PNG also creates a GIF** at the same resolution and in the same folder. There is no separate GIF checkbox, only the GIF Delay. If a set has 0 frames, the empty GIF is removed automatically.
- **The heat map switch (Map Result) follows the selected row in the table.** Selecting another row changes whose heat map is shown.

## Good to know

- **Boundaries must be closed planar curves.** Curves that are not closed or not planar are skipped with a message on the command line, and the other picked curves are still processed.
- **Picking more than 20 boundaries at once asks for confirmation**, so a slip does not make the analysis slow.
- **The start and end times and the frequency are checked before export.** Outside Yearly mode, if the end is not later than the start, or the frequency is not valid, a warning appears and the export stops. If the total frames (dates x times) is more than 50, you are asked to confirm.
- **When the latitude and longitude text is not in the right format, Paste only reports on the command line** and does not interrupt you with a pop-up. If nothing changed after pasting, check that the clipboard holds `latitude, longitude` text.
- **Min Sun Hours and Pass % are two different levels of judgment.** Min Sun Hours decides whether one sample point is good. Pass % decides whether the whole result passes (by default at least 80% of the points must be good).
- **The time zone is always derived from the longitude, with no manual setting**, and ignores daylight saving time and administrative boundaries. This is a deliberate simplification.
- **While the tool is open, the viewport temporarily switches to its own shadow display mode.** When you close or cancel, the display mode and the sun settings from before are fully restored, and nothing is left in your document.
