# zSetCamera

`zSetCamera` creates a separate floating viewport with a **two-point perspective** camera and a parameter panel that drives the camera live. It **does not change any geometry in your scene**. It only works on camera parameters.

## Quick start

1. Run `zSetCamera`.
2. Optionally pick a ground or slab surface (or just press Enter to skip). The tool casts a ray straight down from the camera position of the source viewport to find the ground Z.
3. The tool creates a floating window (1600 x 900 by default, in a render display mode) and opens the parameter panel.
4. Adjust **VIEWPORT**, **CAMERA CONTROL** (LENS, POSITION and FRUSTUM SHIFT) and **NAMED VIEW**. The floating window previews live.
5. **Set** confirms and closes the panel (the floating window stays). **Cancel** discards and closes the floating window.

## VIEWPORT

- **Aspect Ratio**: **16:9**, **1:1**, **4:3**, **5:4**, **4:5** or **Custom**.
- **Width / Height**: the render resolution.
- **Lock Ratio**: forced on and disabled unless the ratio is Custom.
- **Show Grid**: shows the rule-of-thirds lines. On by default.

## CAMERA CONTROL > LENS

- LENS **Length** (mm): starts at the 35 mm equivalent focal length of the source viewport camera.

## CAMERA CONTROL > POSITION

- **Eye Height**: a preset of **Custom**, **Seated** (3.75 ft), **Standing** (5.25 ft, the default), **Tall** (6.5 ft), **Balcony** (12 ft) or **Bird's-eye** (50 ft).
- **Camera Height**: the camera height, kept in sync both ways with the Eye Height preset.
- **Camera Dolly / Strafe**: move the camera forward and back, or left and right, along its viewing direction.

## CAMERA CONTROL > FRUSTUM SHIFT

- **Vertical Shift / FRUSTUM SHIFT Horizontal**: shifts the view frustum up or down and left or right, without re-aiming the camera.

## NAMED VIEW

- **Prefix + Save View**: the name is built as `prefix_aspect_lens`. If the prefix is empty, a timestamp is used.

## How the settings work together

- **Camera Height and the Eye Height preset follow each other both ways.** Choosing a preset writes the value into Camera Height. If you type a Camera Height that does not match any preset, the dropdown falls back to Custom.
- **Width / Height and Aspect Ratio are linked.** With the ratio locked, changing Width works out Height. If you change Height by hand while the ratio is not Custom, the dropdown switches to Custom.
- **Dolly and Strafe cast down again to find the ground Z**, so Camera Height always means the height above the ground directly below the current position, which suits uneven ground.
- **Horizontal Shift needs the lens symmetry constraint to be unlocked first.** The tool does this automatically.

## Good to know

- **Set does not create a named view.** It only closes the panel and keeps the floating window. To create a named view, press **Save View**.
- **Cancel really closes the floating window and discards it.**
- **You can press Enter without picking a ground.** The ground Z is then 0.
