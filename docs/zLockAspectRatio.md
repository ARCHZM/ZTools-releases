# zLockAspectRatio and zUnlockAspectRatio

`zLockAspectRatio` locks the current aspect ratio of a floating viewport window, so that dragging the window border keeps the ratio. There is no dialog, and it never creates a viewport for you. `zUnlockAspectRatio` removes the lock.

Good for:

- A fixed-ratio window for composing or exporting (for example 16:9 or square) that should not get distorted when you drag its edges.

Not for:

- Viewports docked inside the main Rhino window. They are not supported.

## Quick start

1. Make a viewport into a floating window yourself (this tool does not create it), and size it to the aspect ratio you want to lock.
2. Make sure that floating window is the active view, and run `zLockAspectRatio`.
3. The command line confirms the lock and shows the real ratio and the current pixel size.
4. From now on, dragging a border of that window changes width and height together at the locked ratio.
5. To release it, make the locked viewport the active view and run `zUnlockAspectRatio`.

## How it works

There are no settings.

- **Locking:** every time you run `zLockAspectRatio`, the current width and height of the viewport are captured again as the new locked ratio, even if it was already locked.
- **Independent viewports:** any number of floating viewports can be locked at the same time, each to its own ratio, without affecting each other.
- **Dragging a width border changes the height, and dragging a height border changes the width**, based on the side you actually drag.
- **The lock stays in effect until you release it or the window is closed.** It is not lost when you switch to another command or another viewport.

## Good to know

- **Only floating viewport windows are supported.** On a docked viewport, the command refuses with a message asking you to activate a floating window first.
- **Closing a locked window (or destroying it with a layout reset such as "4 Viewport") releases its lock automatically.** You do not need to unlock first.
- **Running `zUnlockAspectRatio` on a viewport that is not locked** just says so on the command line. It is not an error.
