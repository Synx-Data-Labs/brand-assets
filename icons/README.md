# SYNX icon / favicon set

Derived from the circular logo `SYNX-Circular-Logo-Base.svg` (repo root, untouched). Only the visible mark
(ring with two end dots, three bars with heads) is used. The hidden "SYNX" lettering layer, clip paths and Inkscape
metadata were dropped, so **no font or font-licensed outline is included** (the fonts zip in the repo root is not
used or copied). Transforms were flattened and the `viewBox` set to `21 21 135 135`, a square with about 5% safe
margin on each side (the mark itself spans 27..150 in the source coordinates).

## Files

| File | Use |
|------|-----|
| `favicon.svg` | Modern browsers. Black mark, switches to white under `prefers-color-scheme: dark` (embedded `<style>`). |
| `favicon.ico` | Legacy fallback, 16/32/48 px, black on transparent. 16 and 32 px frames use the Simplified mark, 48 px the full mark. |
| `apple-touch-icon.png` | 180x180 black on white (iOS home screen; must not be transparent). Copy of the 180x180 White-background PNG. |
| `site.webmanifest` | Android/PWA icons: 192 and 512 px, black on white. |
| `svg/SYNX-Circular-Icon-{Black,White}-symbol-Transparent-background.svg` | Clean square vector sources. |
| `svg/SYNX-Circular-Icon-Simplified-{Black,White}-symbol-Transparent-background.svg` | Simplified mark for 16/32 px only. |
| `png/SYNX-Circular-Icon-<S>x<S>-{Black,White}-symbol-Transparent-background.png` | S = 16, 32, 48, 64, 128, 180, 192, 256, 512. |
| `png/SYNX-Circular-Icon-<S>x<S>-Black-symbol-White-background.png` | S = 180, 192, 512 (opaque, for touch/PWA icons). |
| `png/SYNX-Circular-Icon-Simplified-<S>x<S>-{Black,White}-symbol-Transparent-background.png` | S = 16, 32. |

White-on-transparent PNGs are for dark backgrounds (they are invisible on white). For dark browser tab strips,
`favicon.svg` handles the switch automatically; `.ico` and PNG favicons cannot.

## Simplified variant

At 16 px the three small bar heads and narrow bars blur into a smudge (and are still busy at 32 px). The Simplified
variant keeps the ring and end dots, drops the heads and draws three wider bars rising left to right. Use it only for
16 and 32 px. `favicon.svg` uses the full mark (it scales to any size; browsers rasterise it at 16/32 px, where the
simplified mark would look sharper; swap the shapes in `favicon.svg` if that matters).

## HTML

```html
<link rel="icon" href="/icons/favicon.svg" type="image/svg+xml">
<link rel="icon" href="/icons/favicon.ico" sizes="48x48">
<link rel="apple-touch-icon" href="/icons/apple-touch-icon.png">
<link rel="manifest" href="/icons/site.webmanifest">
```

Paths in `site.webmanifest` assume the folder is served at `/icons/`; adjust if hosted elsewhere.

## How it was generated

Tools: `rsvg-convert` (librsvg) and ImageMagick 7 (`magick`), plus a small Python (stdlib only) script that extracted
the nine visible shapes (ring, 2 dots, 3 bars, 3 heads) from the base SVG, hid nothing else, and wrote the SVGs.

```sh
B=svg/SYNX-Circular-Icon-Black-symbol-Transparent-background.svg
W=svg/SYNX-Circular-Icon-White-symbol-Transparent-background.svg
for s in 16 32 48 64 128 180 192 256 512; do
  rsvg-convert -w $s -h $s $B -o png/SYNX-Circular-Icon-${s}x${s}-Black-symbol-Transparent-background.png
  rsvg-convert -w $s -h $s $W -o png/SYNX-Circular-Icon-${s}x${s}-White-symbol-Transparent-background.png
done
for s in 180 192 512; do
  rsvg-convert -w $s -h $s $B -b white -o png/SYNX-Circular-Icon-${s}x${s}-Black-symbol-White-background.png
done
for s in 16 32; do for c in Black White; do
  rsvg-convert -w $s -h $s svg/SYNX-Circular-Icon-Simplified-$c-symbol-Transparent-background.svg \
    -o png/SYNX-Circular-Icon-Simplified-${s}x${s}-$c-symbol-Transparent-background.png
done; done
cp png/SYNX-Circular-Icon-180x180-Black-symbol-White-background.png apple-touch-icon.png
magick png/SYNX-Circular-Icon-Simplified-16x16-Black-symbol-Transparent-background.png \
       png/SYNX-Circular-Icon-Simplified-32x32-Black-symbol-Transparent-background.png \
       png/SYNX-Circular-Icon-48x48-Black-symbol-Transparent-background.png favicon.ico
```
