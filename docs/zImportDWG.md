# zImportDWG

`zImportDWG` tidies up the layers after a DWG file exported from Revit has been imported into Rhino. It moves the messy original layer names into a standard layer tree (`00_WORKING`, `01_DESIGN OPTION`, `02_CONTEXT`, `02_SITE`, `03_EXT`, `04_INT`, `05_STRUCTURE`, `06_LIGHT`, `07_ENTOURAGE`, `08_HVAC+MEP`, `ZZ_JUNK`).

It **does not do the import itself.** First import the DWG file into the document with Rhino's own `_Import` command (the file selection and the DWG / DXF Import Options are all Rhino's own interface and are not part of `zImportDWG`). Then run `zImportDWG` to organize the layers of what is already in the document.

It is a **command-line tool with no dialog.** Nothing pops up when it runs, and all results are printed in the command line window.

## Quick start

1. Import the DWG file exported from Revit with Rhino's native `_Import` command (the default import settings are fine).
2. Run `zImportDWG`.
3. The command does the following automatically, with no interaction:
   - Checks whether the document has the 11 standard root layers of the template. If any is missing, it prints a warning on the command line (it **does not stop**, it continues).
   - Using a fixed built-in map, reassigns the matching objects, and the objects inside block definitions, to the standard layers.
   - Runs a one-time Purge over the whole document (cleans up unused block definitions).
   - Deletes the non-standard layers that became empty after the tidy-up (the standard layer tree itself is protected and is not deleted).
4. The command line prints a complete report: the number of layers matched, objects moved, block definitions redirected, target layers created, block definitions purged and empty layers deleted, together with any conflicts and unmatched layers (see below).

## Parameters

`zImportDWG` has no dialog and no settings. The layer map is a fixed map built into the tool (about 100 entries, from source layer name to target layer path), and you cannot edit it or save your own. You neither need to nor can set any option before running.

## How the settings work together

- **It relies on Rhino's native `_Import` having been done first.** `zImportDWG` handles only the objects already in the document and never starts or wraps an import.
- **Purge works on the whole document and always runs.** It is not limited to what this import added. If the document has unused block definitions from before the import, they are cleaned up as well.
- **Objects inside block definitions are handled recursively** (nested blocks included). The layer of an inner object is decided by the target layer where its top-level block instance finally lands.
- **If one block definition is used by several instances that land on different target layers** (common with Revit, where a family is placed on different layers), the last one handled wins. The conflict is recorded and listed in the report, and never happens silently.
- **A layer that has no mapping rule but still holds objects is kept as it is.** It is not swept into `ZZ_JUNK` or any catch-all layer. The report lists these unmatched layer names and their object counts, so you can decide whether they are worth adding to the map (the map itself cannot currently be edited from the interface).

## Good to know

- **There is no pop-up. All results appear only in the command line window.** If you do not look at the command line, you may think the command did nothing.
- **Do not expect a perfect result in a document that is not based on the standard template.** The command line warns about missing standard layers but still carries on with the tidy-up, so target layers may be created in places you did not expect.
- **Purge works on the whole document and is unconditional.** If the document has unused block definitions made by other tools that you want to keep, they are removed too. Make sure there are none you want to keep before you run it.
- **Unmatched layers are not classified automatically.** Check the "unmatched layers" part of the command line report to decide whether the built-in map should be extended.
- **There is no confirmation dialog**, but the whole operation is wrapped in one Rhino undo record, so you can undo it with the standard `_Undo`.
