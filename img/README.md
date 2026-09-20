# Photographs

Everything in here except `map.svg` is a placeholder. The layout is already built
to the final aspect ratios, so dropping real files in with the same names and the
same shapes needs no code changes.

## What each slot needs

| File | Shape | Placeholder size | Subject |
|---|---|---|---|
| `bento-a.jpg` | 4 : 5 portrait | 1600 × 2000 | The courtyard on Dunkley Square, tables set out under the trees. The tall hero image, so the strongest one goes here. |
| `bento-b.jpg` | 11 : 5 landscape | 2200 × 1000 | A loaded meze table, several plates shared. Shot wide, from above or along the table. |
| `bento-c.jpg` | 1 : 1 square | 2000 × 2000 | A food detail: grilled calamari or fried halloumi. Close in. |
| `bento-d.jpg` | 4 : 3 | 2000 × 1500 | Inside the restaurant, or a dog asleep under a table. |
| `og.jpg` | 1200 × 630 | 1200 × 630 | The social-media card. Currently the wordmark on cream, which is fine. A photograph works too. |

## Specification

- **Minimum 2000px on the long edge.** `bento-b` is wider than it is tall, so its
  long edge is the width.
- JPEG, quality ~80%. Progressive if your tool offers it.
- Strip EXIF. Do not upload anything with GPS data in it.
- Landscape and portrait matter more than exact pixels: the browser crops to fill
  the cell with `object-fit: cover`, centred. Keep the subject away from the edges.
- If a replacement has different pixel dimensions, update the matching
  `width` and `height` attributes on that `<img>` in `index.html`. The CSS also
  pins each cell's aspect ratio, so the page will not jump either way, but correct
  attributes keep the Lighthouse score clean.

## WebP

Each slot has a `.webp` beside the `.jpg`, offered first through `<picture>`.
Browsers that cannot read WebP fall back to the JPEG on their own. If you replace
the JPEGs and cannot make WebP files, delete the matching `<source>` lines in
`index.html` rather than leaving stale WebP files in place, or the old image will
keep showing.

Make WebP with [Squoosh](https://squoosh.app) in a browser, or on the command line:

    cwebp -q 80 bento-a.jpg -o bento-a.webp

## Where the photographs come from

From the restaurant: their own camera, their Instagram
([@mariasgreekcafe](https://www.instagram.com/mariasgreekcafe/)), or a shoot.

Do not scrape, hotlink or reuse Google Places photos, TripAdvisor photos, or
anything else found by searching. Those are other people's copyright and Google's
terms forbid it. If you need pictures today, one hour with a phone on a bright
day on the square will beat anything you could take without permission.

## map.svg

Hand-drawn from OpenStreetMap street geometry, so the page makes no third-party
requests and sets no cookies: no consent banner needed. Street data
© OpenStreetMap contributors, ODbL, credited in the bottom corner of the image
as that licence requires. Leave that credit in place.

Labels use whatever monospace font the viewer's system provides, because an SVG
loaded through `<img>` cannot fetch a web font. At that size it is invisible. If
you ever want DM Mono in the map exactly, inline the font as base64 inside
`map.svg` and ship the SIL Open Font License with it.
