# zScale

`zScale` scales the selected objects in a batch. It supports uniform and non-uniform scaling and lets you choose one, two or all three axes.

## Quick start

1. Run `zScale`. The dialog is titled **Batch Scale**.
2. Select one or more objects.
3. Adjust **SETTING** and **FACTOR**.
4. **Apply** applies the scaling. **Cancel** discards it.

## SETTING

- **Scaling Mode**: **Uniform** or **Non-Uniform**.
- **Axis X / Y / Z**: switches that turn scaling on or off for each axis. All are on by default.
- **Align**: **World** or **CPlane**. The coordinate system the scaling refers to.
- **Base Plane**: **Bottom**, **Volume** or **Top**. Where the scaling base point sits in the bounding box: at the bottom, at the volume center, or at the top.

## FACTOR

- **Scale X / Y / Z**: the scale factors. You can type a simple math expression.

## How the settings work together

- In **Uniform** mode, changing the value of any enabled axis updates the other enabled axes too (priority X, then Y, then Z). An axis that is not enabled is locked at 1 and grayed out.

## Good to know

- Only one dialog can be open at a time. Running the command again brings the existing dialog to the front instead of opening a new one.
- Every object is scaled about **its own** base point, not about the common center of the whole selection.
