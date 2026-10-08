# zBlockManager

`zBlockManager` is a table-style manager for working on many blocks at once: rename them, redefine base points, change layers, and clean up duplicate or unused block definitions. **It works on the block definitions themselves. It does not pick curves to build new geometry.**

## Quick start

1. Run `zBlockManager`. The first time you open it in a document, a two-step **Block Manager Setup** wizard appears (only once per document), which narrows the scope before scanning so that a large model does not freeze. Three counts always show at the top of the window: Definitions, Instances and Unused. Go through it with Back / Next / Finish. Closing the window (X) skips the remaining steps and keeps what you finished (each step is applied when you press Next).
   - **Step 1, Unused Definitions**: the **Purge Unused** button is at the bottom left, on the same row as Back and Next. Press it and you are first asked to confirm (the default is No), and then the block definitions without instances are deleted at once, which you can undo with Ctrl+Z. The Definitions and Unused counts at the top update at once, and the button turns gray. Next deletes nothing.
   - **Step 2, Exclude Blocks**: lists every block that has more than 100 instances (it scrolls when there are more than 8). The checkbox at the left of the header selects all. Ticked blocks are excluded from the scan and do not appear in the list, and the Definitions and Instances counts at the top follow the ticks live. The scan scope is always all blocks. For a model with more than 300 definitions, pressing Finish without narrowing anything asks you to confirm.
   - The **Auto Update** switch at the top of the main window is on by default (the list refreshes automatically when the document changes). On a large model, you can turn it off yourself and refresh by hand with Rescan.
   - At any time you can right-click the zBlockManager icon on the toolbar (the command `zBlockManagerSetup`) to run the wizard again. When it finishes, the Block Manager opens (or refreshes) and rescans with the new settings.
   - The document's blocks are scanned automatically only after the wizard has finished.
2. Select one or more rows in the table (click, Ctrl+click to toggle, Shift+click to select a range, and there are no checkboxes). The readout row at the top shows the number selected. Click an empty area of the table or press Esc to clear the selection.
3. Use the icons in the rows or the buttons in the **ACTIONS** area at the bottom. **Every action takes effect at once. There is no confirm and commit step.**

## The table

Columns, left to right:

- **Block Definition**: the name of the block definition. Nested child blocks are shown in gray with an L-shaped tree line.
- **Layer**: the layer of the instances. It shows `Mixed` when there are several layers, and a red `unused` when there are no instances.
- **Instances**: the number of instances placed directly in the model. The sum of the column equals INSTANCES in the setup wizard. A block that is used only inside other blocks shows 0, and hovering over the number shows how many times it is used nested.
- **Objects**: the number of members in the definition.

**Nesting.** A block that contains other blocks has its child blocks right below it, in gray text with an L-shaped tree line on the left. A block that is used only inside other blocks and is not itself placed in the model has 0 instances, and its Layer shows a gray `(inside parent)`. DEFINITIONS is the number of block names in the list. Then come **Vis** (visibility icon), **Iso** (isolate icon) and **Zoom** (selects and zooms to all instances of that block).

Double-click the name column to rename. When the list opens it is sorted by block name A to Z (you cannot reorder by dragging). Right-click the Block Name, Instances or Objects header to change the sort: name A-Z or Z-A, number of uses high to low or low to high, number of objects high to low or low to high, or back to Rhino's native order (this changes only the display and is not written to the document). **The Sequential numbering order of Rename Block** is, by default, the table from top to bottom. In the confirmation preview window each row has a drag handle at the left that lets you change the order (nested child blocks move together with their parent), and the new names and numbers are recalculated live. Find/Replace mode has no handle.

**Right-click menu of the name column** (select the row first. If it is already part of a multiple selection, the multiple selection is kept and the action applies to the whole selection): Rename, **Delete**, Show, Hide, Isolate, Zoom to Objects, Change Layer..., Select All, Invert Selection (Show and Hide are two separate items that show or hide the selected blocks, not a toggle). The tooltip of the name column lists these actions too.

## ACTIONS

One group box. The title shows the number selected, and there are four columns grouped by function. A button that needs a selection is grayed out when nothing is selected.

- **EDIT BLOCK**:
  - **Rename Block**: opens the rename window (5 rules with a live preview. With nothing selected it changes all).
  - **Reset Scale**: opens the Reset Scale window (see below).
- **LAYER & COLOR**:
  - **Set Block Layer**: choose a layer by hand and move every instance of the selected blocks to it.
  - **Layer by Block Name**: creates a layer named after each selected block (an existing layer with that name is reused, and a new layer gets a random color) and moves the instances to it.
  - **Color Code Block**: opens a menu with **By Parent** and **By Layer**, with a check mark on the current state. The choice applies at once to the selected blocks. By Parent makes the members of the block show the color of the layer of the instance that holds them. By Layer makes the members show the color of their own layer.
- **BASE POINT**:
  - **Extract Base Point**: puts a point object at the base point of every instance. It uses the blocks selected in the list first, and only when nothing is selected does it ask you to pick instances in the viewport (when 50 or more instances are selected you are asked to confirm first).
  - **Redefine Base Point**: sets a new base point for the selected blocks. You only click one new location.
- **CLEANUP**:
  - **Purge Unused Blocks** (the broom icon): deletes definitions that have no instances and are not nested in another block. It asks first (the default is No), and can be undone with Ctrl+Z.
  - **Merge Duplicates**: finds duplicate block definitions and opens a preview window that lists which definitions will be merged into which one that stays. Only pressing Merge really merges them. If there are no duplicates, you get a note.

## Details of the actions

**Rename Block window.** Everything in one window. A line at the top states the scope ("Renaming 12 selected block(s) and 5 nested block(s)". With nothing selected it says in red that all N will be renamed). Nested child blocks of the selected blocks are changed together. You choose one rule at a time, and the preview below updates live. In the previews of Find/Replace and Prefix/Suffix, the changed characters are shown in bold red (Case and Simplify are not marked). The preview lists only the rows that change, with conflicting rows at the top in red, and a line at the top counts them: "Will rename X, skip Y with a name conflict, Z unchanged". Only **Apply** really renames, and one Ctrl+Z undoes it. The five rules:

- **Numbering**: fill in Name, Start at and Digits (zero padding), for example Block_001, Block_002... The numbering order is the table from top to bottom by default, and you can drag the handle at the left of a row in the preview to change it (nested child blocks move with their parent).
- **Find/Replace**: text replace. The search ignores case, and the replaced part is highlighted in the preview.
- **Prefix/Suffix**: add text before or after the name.
- **Case**: UPPERCASE, lowercase or Title Case.
- **Simplify**: simplifies Revit names by removing the numbers and tails Revit adds. You can tick separately "remove the trailing _3D and user name part" and "remove the Revit number", and let names that become duplicates after cleaning get _2, _3 and so on automatically.

**Delete** (right-click menu or the Del key): deletes the selected block definitions **and all of their instances in the document**. A confirmation always appears first (it lists the block names and the total number of instances, and the default is Cancel). After confirming, you can restore with Rhino's Undo. Pressing Del inside a text box does not trigger a delete.

**Reset Scale window.** Counts, over the whole file, how many blocks have instances whose scale is not 1, and lists only those blocks. Blocks with normal scale are only counted, not listed. A line at the top says "X of Y blocks need a scale reset." Each row has a checkbox (all ticked by default, and the header checkbox ticks or unticks all), the block name, **Scale** (the scale in X / Y / Z: if all three directions are equal only one number is written, for example 96, if they differ it is written X / Y / Z, and when different instances have different scales a range is written, for example 0.5-2) and **Instances** (the total when all are to be reset, and "2 of 3" when only some are). **Click a name** to select in Rhino the instances of that block whose scale is wrong and zoom there (hidden ones are shown), and the clicked row turns bold with a light red background. Only pressing **Reset Scale** at the bottom applies it. It handles only the instances whose scale is not 1 in the ticked blocks, keeps position and rotation, and can be undone with Ctrl+Z. The height of the window follows the number of rows and is usually short.

**Set Block Layer** changes only the layer of the top-level instances of the definition. **The layers of the members inside the definition do not change at all.**

**Layer by Block Name** differs from Set Block Layer in that the target layer is not chosen by hand but is named after the block itself and reused. It suits cases with many different blocks where each should get its own layer.

**Color Code Block** changes only where the members take their color from (the color source). **It never touches the layer of any member**, so switching back to By Layer needs no memory of any earlier state.

## How the settings work together

- **Rename with nothing selected works on all blocks.** The top of the window says in red that all N will be renamed, and Apply asks once more (the default is No). Selecting a parent block changes its nested child blocks too, and the top of the window says how many.
- **Set Block Layer, Layer by Block Name and Color Code Block need a selection.** The buttons are gray when nothing is selected. Reset Scale does not depend on the selection and always scans the whole file. Extract Base Point and Redefine Base Point ask you to pick instances in the viewport when nothing is selected.
- **After every action, a change preview appears for a second confirmation**, with Old to New, and conflicts shown in red and skipped.

## Good to know

- **This tool works on block definitions, not on curves or surfaces.** It is completely different from the other "pick, then build" tools.
- **"Layer" always means the layer of the top-level instances**, not that of the definition or of the member geometry.
- **Double-clicking a name renames it.**
- **Purge Unused Blocks and Merge Duplicates both ask first** (the default is No), and can be undone with Ctrl+Z after you confirm.
- **Blocks that are excluded or outside the scan scope do not appear in the list** and are not affected by batch actions. To see them again, right-click the zBlockManager icon on the toolbar, run the wizard again and untick them.
- **The width of the name column cannot be dragged.** It adjusts itself to the widest name found by the scan.
- **A large Explode or delete (1000 or more members or instances) fires a flood of document events at once. The tool merges and debounces them so it does not freeze**, and the background scan itself does not block the interface either.
