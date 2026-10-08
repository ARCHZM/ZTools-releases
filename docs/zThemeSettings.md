# zThemeSettings

`zThemeSettings` lets you customize the color tokens shared by all ZTools dialogs, and override the default automatic light and dark detection. A change affects the look of the dialogs of every ZTools tool.

Good for:

- You do not like the default red accent and want your own brand color.
- Automatic light and dark switching is not accurate in your lighting, and you want to fix one mode.

## Quick start

1. Run `zThemeSettings`.
2. **Mode** at the top (Auto, Light or Dark) decides which set of colors is actually applied right now.
3. In the **EDITING** group, choose whether to edit the Light set or the Dark set (independent of Mode).
4. In the **COLORS** group, click any swatch to open the system color picker. A color takes effect and is saved as soon as you choose it.
5. **OK** keeps the changes and closes. **Cancel** undoes everything you changed since you opened the dialog (back to the state when it opened).

## Top (no group)

- **Mode**: **Auto**, **Light** or **Dark**. It decides which set of colors all ZTools dialogs actually use right now. Auto decides from the brightness of the Rhino interface.

## EDITING

- **Editing**: **Light** or **Dark**. It decides which color scheme the COLORS list below shows and edits. It is independent of Mode at the top: you can edit the Light scheme while Mode is Dark.

## COLORS > Accent

- **Ink** (the main color): the accent color of all dialogs (red by default). Click the swatch to change it, and right-click to reset it to the default.
- **Hover Fill / Hover Border / Text Selection / Row Selection Bg**: all four are derived from Ink automatically and cannot be edited (the swatches are not clickable). Changing Ink updates these four.
- **Hover Bg / Active Bg (computed from Ink)**: shown for reference only and not editable. They are the hover and active backgrounds that you get by laying Ink on at a fixed transparency.

## COLORS > Text

- **Text / Row Name Text / Disabled Text / Text On Ink Fill**: the text colors in four situations. Each can be changed and reset on its own.

## COLORS > Surface

- **Background / Page Background / Border / Control Track Bg**: the dialog background, the page background, the border and the control track background.

## COLORS > Table / List

- **Group Line / Table Line / Scrollbar Thumb / Scrollbar Track**: the group divider line, the table line, the scrollbar thumb and the scrollbar track.

## COLORS > Status

- **Status Good / Status Fail**: the success and failure colors (for example the pass / fail panels of the analysis tools).

## How the settings work together

- **Changing Ink also changes 4 derived colors and 2 preview colors** (Hover Fill, Hover Border, Text Selection, Row Selection Bg, Hover Bg and Active Bg). None of them can be set on its own.
- **A color is applied and saved as soon as you choose it**, with no need to press OK. Any ZTools dialog that is already open uses the new colors the next time it opens.
- **Cancel undoes every change made since the dialog opened** and goes back to the colors from before you opened it (Mode included). OK just closes the dialog and keeps everything that has been applied.

## Good to know

- **To restore a single color to its default:** right-click the swatch and choose Reset to Default. Only that one is reset.
- **The Reset button at the bottom resets the whole scheme:** it restores every color of the set currently chosen in EDITING (Light or Dark) to the default in one go, and does not affect the other set.
- **This tool does not control line weight, fonts, or the viewport guide lines and annotation colors of each tool.** It affects only the colors of the dialogs themselves (background, border, text, table lines and so on).
- **ZTools dialogs that are already open do not refresh live when a color changes.** Close and reopen them to see the new colors.
