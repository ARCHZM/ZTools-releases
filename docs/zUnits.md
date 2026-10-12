# zUnits

`zUnits` switches every ZTools window between two unit systems, **Imperial** (inches and feet) and **Metric** (millimeters and meters). There is one switch for the whole tool set, no window has its own unit setting.

## Quick start

1. Click the **zUnits** button in the Utility toolbar. Each click flips between Imperial and Metric.
2. Or type `zUnits` and choose **Imperial**, **Metric** or **Toggle**.
3. Every open ZTools window changes at once, and every window you open later starts in the chosen system.

Imperial is the default. The choice is remembered between sessions.

## What each system shows

- **Imperial**: building parts (stairs, railings, windows, doors, curtain walls, roofs, louvers, dimension text sizes) are in inches. Site sizes (eye height, view range, contour interval, level heights) are in feet. Dimension texts offer feet and inches, decimal feet, decimal inches and fractional inches. Area and volume offer ft and in.
- **Metric**: building parts are in millimeters, site sizes in meters. Dimension texts offer mm, cm and m. Area and volume offer m, cm and mm.
- Angles stay in degrees and show a degree sign.

## How it behaves

- **Each system keeps its own values.** If you set a railing height to 42 in and then switch to Metric, the field shows its metric value (1067 mm by default, or whatever you used last). Switching back shows 42 in again. Nothing is converted on a switch, so the numbers stay tidy in both systems.
- **It does not depend on the Rhino document unit.** A model in millimeters can be worked on in Imperial and the other way around. ZTools converts what you type into the document unit when it builds geometry.
- **You can change the document unit while a window is open.** The window keeps showing your numbers in the chosen system, and the preview is rebuilt for the new document unit. No restart is needed.
- **Already baked objects are not changed.** Switching the system only changes what the windows show. Editing a baked object (for example with `zDoorEdit`) shows its stored size in the active system.
- **Typing units.** Some fields accept a unit, for example `zLevelTag` (`3.2m`, `3200mm`, `10ft`, `10'-6"`) and the Size fields of `zTextureMappingByObjects` (`20cm`, `8in`).

## Things to know

- `zModelScaler` is a free converter: both sides keep their own unit list. Only the starting unit of the physical side follows the system.
- Settings saved by older versions for the old per-window unit switches are not read, so those fields start from their defaults once.
- The metric defaults are the same real sizes as the imperial ones, rounded to tidy millimeters (for example 38 / 150 / 75 mm instead of 1.5 / 6 / 3 in).
