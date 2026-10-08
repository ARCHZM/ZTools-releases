# zRevitMapping

`zRevitMapping` is a global configuration manager. It sets the correspondence between each **bakeable ZTools command** and a **Revit Category / Family / Type**. ZTools has no live connection to Revit. The only purpose is to make sure every object ZTools bakes carries the right `RVT_Category`, `RVT_Family` and `RVT_Type` user strings, so that a Grasshopper workflow through Rhino.Inside.Revit (nodes such as `Query Family Types` and `Set Parameter`) can read them directly and turn the stairs, roofs, railings, windows and curtain walls into the right Revit elements. Building the Grasshopper side is not part of this tool.

The configuration is global and works across projects (it is not stored per .3dm file). Set it up once and every document reuses it.

## Quick start

1. Run `zRevitMapping` to open the settings window.
2. Each bakeable command has one row: the command name (read only), a **Category** dropdown, a **Family** text box, a **Type** text box and that row's own **Reset** button.
3. Every field is saved the moment you change it (a Category is saved when you choose it, and Family and Type when the text box loses focus). There is no pending state that waits for OK. OK and Cancel both just close the window.
4. **Reset** restores that row to the factory default of its command (or clears it if the command has no default).
5. You can also use the **Import...** button at the bottom to import every row at once from an .xlsx or .txt file (see the import format below).

## COMMAND -> REVIT CATEGORY / FAMILY / TYPE

Each bakeable command has **one row**, and everything that command bakes shares the same Category, Family and Type. There is **no difference between sub-parts** (for example the treads, risers and stringers of a stair all use the one row of that command).

- **The 9 bakeable commands**: `zStairByCrv` (Stair (By Curve)), `zStairByRegion` (Stair (By Region)), `zSpiralStair` (Spiral Stair), `zRoof` (Roof), `zRailing` (Railing), `zRailingSide` (Railing (Side Mount)), `zWindow` (Window), `zLouver` (Louver) and `zCurtainWall` (Curtain Wall).
- **Category**: a dropdown with a fixed list of Revit built-in architectural model categories. You cannot type freely, so that a typo does not make the Grasshopper side silently fail to match: Casework, Ceilings, Columns, Curtain Panels, Curtain Systems, Curtain Wall Mullions, Doors, Entourage, Floors, Furniture, Furniture Systems, Generic Models, Mass, Model Groups, Parking, Planting, Railings, Ramps, Roads, Roofs, Site, Specialty Equipment, Stairs, Structural Columns, Structural Foundations, Structural Framing, Structural Trusses, Topography, Walls, Windows.
- **Family**: a free text box, with no checking. On the Revit side it is entirely yours to define.
- **Type**: a free text box, with no checking.
- **Reset** (the button of each row): restores the factory default of that command. The defaults are:
  - `zStairByCrv` and `zStairByRegion`: Stairs / Assembled Stair / Monolithic Stair.
  - `zSpiralStair`: Stairs / Spiral Stair / Spiral Stair.
  - `zRoof`: Roofs / Basic Roof / Generic - 9".
  - `zRailing` and `zRailingSide`: Railings / Guardrail - Pipe / 1100mm.
  - `zWindow`: Windows / Fixed / 36" x 48".
  - `zCurtainWall`: Curtain Systems / Curtain System / Curtain System 1.
  - `zLouver` has only a default Category (Generic Models). Family and Type have no default, because Revit has no built-in louver category, so you fill them in from the families actually loaded in your project.

### The Glass sub-row (only for zCurtainWall)

Only `zCurtainWall` has an indented "Glass" sub-row below its row, because the glass panels of a curtain wall are usually a different Revit category from the frame and mullions (the frame is Curtain Systems and the glass is Curtain Panels).

- **Category of the Glass row**: a separate dropdown you can set on its own. The default is Curtain Panels.
- **Family / Type of the Glass row**: read only (grayed text boxes) and always the same as the main row of that command, so that "two rows that write the same value" does not mislead you.
- `zRailing`, `zRailingSide` and `zWindow` also bake glass geometry, but they have **no** Glass sub-row of their own. Their glass uses the same Category as the main row of the command (a deliberate trade-off).

## Import... (button at the bottom)

- Supports `.xlsx` (the first worksheet is read, and the first row is always skipped as a header) and `.txt` (tab separated. If the first cell of the first row is "Command", that row is skipped as a header, otherwise the whole file is read as data rows).
- **Exactly 4 columns**, in the order **Command / Category / Family / Type**. Command must be the internal English name of the command, such as `zStairByCrv`, not a display name in the dialog.
- A Command that is not recognized (not one of the 9 bakeable commands) is skipped and not written, and it is listed in the message when the import ends, so a typo in a command name does not lose a setting silently.
- After the import, the changes are refreshed onto the rows of a dialog that is already open (the Glass sub-row of `zCurtainWall` included).

## How the settings work together

- **The configuration is global and kept across documents** (stored in Rhino's persistent settings). It is **not** stored per .3dm file. It still applies after you switch documents or restart Rhino.
- **Every command that bakes geometry writes three extra user strings `RVT_Category`, `RVT_Family` and `RVT_Type`** next to its own existing marks, without changing any of the command's own mark fields.
- **Changing a row does not update objects already baked in a document.** It affects only objects baked afterwards. Existing objects must be baked again to get the new mapping.

## Good to know

- **Setting a different Family for one particular part of a command (for example the risers of a stair) is not supported.** An earlier version split the settings by sub-part (treads, risers, stringers and so on), but it made the dialog about 250 live controls and slow to open, so it went back to the simple "one row per command" structure, and will not split to sub-parts again.
- **If a Family or Type does not match the real family name in your Revit project, ZTools cannot know.** It has no live connection to Revit, and what you type is plain text with no checking. You have to make sure it matches the real names in the target Revit project.
- **A settings file from the older sub-part version will not import as expected.** The current import format is fixed at 4 columns (Command, Category, Family, Type), with no sub-part column, so an old file has to be rearranged into the new 4-column form first.
