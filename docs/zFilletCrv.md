# zFilletCrv

`zFilletCrv` fillets the line-to-line corners of one or more curves in a batch. Each corner gets a circular arc, by a fixed radius or by a fixed trim distance, and you can choose whether arcs that already exist in the curve are left alone.

Good for:

- Rounding every corner of several polylines at once instead of one by one.
- Curves that have both straight segments and arcs, where you only want to fillet the true straight-to-straight corners and keep the existing arcs.

Not for:

- A true tangent fillet at a line-to-arc transition. The tool does not support that, and skips those corners.

## Quick start

1. Run `zFilletCrv`.
2. Pick one or more curves. Only curves with **at least two straight segments** can be picked. A pure arc or a single straight segment cannot (see Good to know).
3. The dialog opens, and the live preview shows the fillet at every corner.
4. Adjust **Mode**, **Radius / Distance** and **Keep Arcs**.
5. **Apply** creates the geometry. **Cancel** discards it and the original curves are untouched.

## Parameters

- **Mode**: decides whether the number below is read as a **radius** or as a **trim distance**.
  - **Fixed Radius**: the number is the radius of the fillet arc. The sharper the corner (the smaller the angle), the more length is trimmed from the two sides to make that radius.
  - **Fixed Distance**: the number is the length trimmed from the corner vertex along each of the two edges. The sharper the corner, the smaller the resulting fillet radius for the same trim distance.
  The two modes are two ways of entering the same geometry. Switching mode keeps the number you typed but changes its meaning, so the result usually looks quite different. Check the preview after you switch.
- **Radius / Distance**: the value. The label changes between Radius and Distance with Mode. A larger value gives a gentler arc and uses more of the length of the two sides. If the value is larger than the available length of an edge, the corner is skipped and stays sharp, with no error and no wrong geometry. The slider only goes to a small range, but you can type a much larger number in the box.
- **Keep Arcs**: off by default. Whether only the straight-to-straight corners are filleted while arcs already in the curve are kept.
  - **On**: existing arcs keep their radius exactly, and only true corners are replaced by new fillet arcs.
  - **Off**: the whole curve, existing arcs included, is first straightened into a plain polyline, and every corner of the straightened curve gets a fillet with the same radius or distance. The shape of the old arcs is discarded. If an old arc had a much larger radius than your new value, you will see a gentle curve turn into a tighter small fillet.
- **Original**: also shows the original, unfilleted curve in the preview, for comparison. This affects only the preview, not what is baked.
- **Circles**: shows the full reference circle of each fillet arc in the preview, to help you judge the curvature. This affects only the preview.
- **Radius** (display): labels each fillet arc with its real radius. This affects only the preview.

## How the settings work together

- **Mode and the number are two readings of one value, not two separate parameters.** Switching Mode does not clear the number, but it changes what the number means, so the same number gives a clearly different fillet. Check the preview after switching.
- **Original, Circles and Radius affect only the preview**, never the baked result. Turn them on and off freely.

## Good to know

- **Only curves with at least two straight segments can be picked.** A curve that is entirely an arc, or has only one straight segment, simply cannot be selected: the picker skips it, so you will find you cannot click it. If a curve cannot be picked, check whether it contains at least two real straight segments.
- **A value larger than an edge skips that corner silently.** If a corner "has a fillet set but does not change", the most common reason is that the value is too large for the length of the edge. The tool skips the corner to avoid bad geometry and shows no message. Reduce the value to see the effect.
- **The slider tops out at 10, but you can type a much larger number in the box.**
- **Corners near 180 degrees, or nearly collinear corners, are left as they are.** This is a deliberate safeguard against oddly shaped arcs at degenerate angles, and is normal.
- **With Keep Arcs off, existing arcs are straightened and rebuilt.** If the old arc radius was much larger than the new value, the gentle arc becomes a tighter fillet. This is expected (the curve is treated as a plain polyline). If you do not want existing arcs changed, keep Keep Arcs on.
- **A bad setting cannot produce a self-intersecting or broken curve.** The tool guards against all known extreme inputs. A corner that cannot be filleted safely is skipped. In the worst case some corners are simply not filleted.
