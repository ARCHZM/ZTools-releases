# zDimRadius

`zDimRadius` detects the arc and fillet segments in the objects you pick and dimensions their radius. The label is always `r=value`.

## Quick start

1. Run `zDimRadius`.
2. Pick curves or Brep edges.
3. The tool finds the parts that can be reduced to circular arcs and dimensions them.
4. Adjust the **TEXT**, **DIMENSION** and **ARROW** groups.
5. **Mark** commits the dimensions. **Cancel** discards them.

A row at the top has three presets, **Tiny**, **Small** and **Large Object**. The parameters are in three group boxes: **TEXT**, **DIMENSION** and **ARROW**.

## TEXT

- **Unit Format**: the length unit format of the radius text, from the same list of units as `zDimLength`.
- **Text Size**: the height of the dimension text.
- **Decimal Places**: the number of decimals in the value. All three presets share the same range.

## DIMENSION

- **Label Offset**: the distance from the arc to the label, along the radius direction.
- **Show Circle & Center Cross** (on by default): draws a dashed full circle and a center cross for each arc, as a reference.

## ARROW

- **Arrow Style / Arrow Size**: the style and size of the leader arrow.

## How the settings work together

- **Changing Unit Format regenerates every label text.**

## Input requirements

Only parts that can be reduced to a circular arc are dimensioned. Straight segments and free curves that are not arcs give no dimension. A mixed curve with fillet or arc transitions dimensions only its true arcs.

## Good to know

- **If a picked curve changes its structure while the dialog is open (more than a move or a scale), a one-time notice appears** saying the curve has changed and the dimensions may no longer be accurate, and suggesting you run the command again.
- **If you close the dialog without pressing Mark, the tool removes its empty layer**, as `zDimLength` does.
- **After Mark, the dimensions are split into 3 separate groups:** the text labels, the reference circles and cross lines, and the leaders. This lets you select or hide one group at a time.
