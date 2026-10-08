# zModelScaler

`zModelScaler` scales the selected objects to a chosen print or model scale. It also has a separate two-way converter between physical size and model size.

## Quick start

1. Run `zModelScaler`.
2. You can press Enter without selecting anything and use only the dimension converter.
3. To scale objects, select them first.
4. Adjust **SCALE** and **DIMENSION CONVERTER**.
5. **Bake** creates the scaled copy. **Cancel** discards it.

## SCALE

- **Scale presets**: a full set of 20 architectural scales plus **Custom**. Imperial presets are all written as `1" = X'-Y"`.
  - Detail scales: `1"=0'-4"`, `0'-8"`, `1'-0"`, `2'-0"`, `4'-0"`, `8'-0"`.
  - Plan scales: `1"=16'-0"`, `32'-0"`, `64'-0"`, `128'-0"`.
  - Metric scales: `1:1`, `1:5`, `1:10`, `1:20`, `1:25`, `1:50`, `1:100`, `1:200`, `1:500`, `1:1000`.
- **Custom Feet** (shown only for the Custom preset): the denominator of your own scale.

## DIMENSION CONVERTER

Two fields, left and right, that can be swapped. Each has its own unit (mm, cm, m, in, ft). Type a physical size on one side, or a model size, and the other side is calculated live from the current scale. The button in the middle swaps which side is the physical size.

## How the settings work together

- **The Custom Feet box appears only for the Custom preset.** Every other preset uses its fixed scale.

## Good to know

- **The dialog opens even with nothing selected.** It then works only as a dimension converter, with no error.
- **The whole selection is scaled as one rigid group about the bottom center of the combined bounding box**, not each object on its own.
- **Bake creates a new copy and never moves or changes the original.**
- **A frame rectangle that fits the outline of the scaled result is added automatically on the `zModelScaler` layer.** It is no longer a fixed-size print sheet.
