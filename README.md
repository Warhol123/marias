# Maria's Greek Café — website

One static page for Maria's Greek Café, 31 Barnet Street, Dunkley Square,
Gardens, Cape Town.

Plain HTML, CSS and a few lines of vanilla JavaScript. No framework, no build
step, no package manager, nothing to install. The only outside request is Google
Fonts. There is no analytics, no tracking and no cookies, so the page needs no
consent banner.

```
index.html      the whole page
styles.css      all styling
img/            photographs, the map, the social card — see img/README.md
```

## Putting it online

Upload the three items above, keeping the folder structure, to any static host:
Netlify, Cloudflare Pages, GitHub Pages, Vercel, or ordinary shared hosting over
FTP. There is nothing to compile. To preview it on your own machine, open
`index.html` in a browser, or from this folder run:

    npx http-server . -p 8080

### Before launch

Three things carry placeholder values. Search `index.html` for each.

1. **The domain.** `mariasgreekcafe.co.za` appears in the Open Graph tags, the
   Twitter tags, `rel="canonical"` and the JSON-LD block. Replace every instance
   with the real domain. Social-media previews need absolute URLs, which is why
   they cannot be left relative.
2. **The Facebook URL.** Marked with a `TODO` comment in the footer. Facebook
   blocks automated checks, so it was never verified. Open the restaurant's page,
   copy the address from the browser bar, paste it in.
3. **The photographs.** All four are placeholders. See `img/README.md`.

## Everyday changes

### The photographs

Covered in `img/README.md`. Short version: same filenames, same shapes, drop them
in `img/`.

### The hours

They live in two places and both must be changed together.

**What visitors read** — in `index.html`, in the Practical section:

```html
<dt>Hours</dt>
<dd>
  <span>Mon 11:00–22:00</span>
  <span>Tue–Sat 08:30–22:00</span>
  <span>Sun 10:00–16:00</span>
</dd>
```

Add or remove `<span>` lines freely, one line each.

**What Google reads** — the `openingHoursSpecification` block in the
`application/ld+json` script in `<head>`:

```json
{ "@type": "OpeningHoursSpecification", "dayOfWeek": "Monday", "opens": "11:00", "closes": "22:00" }
```

Use 24-hour times with a colon, English day names, and keep the commas between
entries. One entry can carry several days as a list, as the Tuesday-to-Saturday
one does. A missing day means closed. If you edit this and want to be sure it is
still valid, paste the page into Google's
[Rich Results Test](https://search.google.com/test/rich-results).

### The phone number

In four places: the two Reserve buttons (`href` and `aria-label` on each), the
Booking row in Practical, and `telephone` in the JSON-LD. Search for
`+27214613333` and `021 461 3333` to catch them all. The `tel:` links use the
international form so they work for people phoning from abroad.

### The dishes

In the Food section, one `<li>` per dish:

```html
<li><span class="dish">Keftethes</span><span class="note">lamb meatballs</span></li>
```

Keep descriptions to three to five words; they sit on the same line as the dish
on a desktop screen and wrap under it on a phone. To mark a house speciality, add
the terracotta dot and its screen-reader text inside the `dish` span, copying an
existing lamb line. Two dots is about the limit before they stop meaning anything.

This is deliberately not the menu. The menu changes, and a menu on a website goes
stale within a month. The page says to phone or come by for what is on today.

## How it is built

### Type

Two roles, no third font.

| Role | Font | Used for |
|---|---|---|
| Display | Italianno, plus four more script faces in the wordmark | The `h1`, the section headings, the footer mark |
| Everything else | DM Mono 300 / 400 / 500 | Navigation, buttons, paragraphs, the food list, the table, labels, fine print |

**The wordmark.** The `h1` cycles through five handwritten renderings of
"Maria's": Italianno, Mrs Saint Delafield, Pinyon Script, Style Script and Mr De
Haviland. One crossfades to the next every 2600ms over 700ms, forever.

The five faces have very different x-heights, widths and baselines, so each
variant carries its own `font-size` multiplier and its own two-axis translation,
in `styles.css`:

```css
.v3 { font-family: 'Pinyon Script'; font-size: calc(var(--script) * 1.57); transform: translateY(.267em); text-indent: -.070em; }
```

Those numbers are not guesses. Each face was rendered and measured, and the
multipliers were set so all five words occupy the same optical box: ink widths
land within 8% of each other, and every variant's baseline sits at exactly
1.58 × `--script` from the top of the reserved box. The corrections are in `em`,
so they hold at every viewport size without a second set of numbers. The effect
should read as one name written five times by hand. If you change a
`font-size` multiplier, re-measure rather than eyeball the translation.

The `h1`'s height is reserved explicitly (`calc(var(--script) * 2.06)`), so
nothing on the page moves as the cycle runs. Screen readers get the name once,
from `aria-label` on the `h1`; the five spans are `aria-hidden`. With JavaScript
off, or with "reduce motion" set in the operating system, the first variant simply
sits there and nothing cycles.

One master value drives the whole wordmark:

```css
--script: clamp(3.75rem, 9vw, 7rem);   /* 60px on a phone -> 120px on a desktop */
```

Change that one line to resize all five together.

### Colour and contrast

```css
--cream: #F4F5ED      /* the page */
--ink:   #2B5CC4      /* all text, all borders, all rules. Aegean blue */
--rule:  --ink at 20% /* dividers */
--accent:#C75B39      /* terracotta */
```

No black and no grey anywhere on the page.

Two deliberate deviations from the original palette note, both forced by WCAG AA,
both worth knowing about if you edit the colours:

- **Dimmed ink cannot carry small text.** `--ink` on cream is 5.58 : 1, which
  passes AA comfortably. The same blue at 60% opacity is 2.60 : 1 and at 70% it is
  3.12 : 1 — both fail the 4.5 : 1 that normal-size text requires. It needs about
  88% opacity before it passes, by which point it is indistinguishable from full
  ink. So secondary text on this page is full-strength `--ink` and separates
  itself by weight instead: DM Mono 300 against 400, which is what the mono face
  is for. The dimmed token, `--ink-soft` at 72%, is used only on the Italianno
  headings and the footer mark, which are large text and need only 3 : 1 (they get
  3.24 : 1).
- **The button fill is 8% deeper than the accent.** Cream text on `#C75B39` is
  3.84 : 1 and fails. `--accent-fill: #B04E2F` gives 4.81 : 1 and passes. The two
  are near-indistinguishable side by side. `--accent` itself, unchanged, is still
  what the two speciality dots use, where it is decoration and no contrast rule
  applies.

Terracotta appears exactly three times on the page: the two Reserve buttons and
the two speciality dots. The buttons are the only filled elements. There are no
shadows, no gradients and no third colour.

### Booking

There is no online booking, by design and by fact: their Dineplan is disabled and
bookings are taken on the phone. Both Reserve buttons are plain
`<a href="tel:+27214613333">`. On a desktop, hovering or tab-focusing the button
swaps the label to the number; the button is sized to the wider of the two labels,
so it never changes width. On a phone it just dials.

### Accessibility

- One `h1`; every section is an `h2`. Semantic `section`, `dl`, `ul`, `nav`,
  `footer`.
- All text meets WCAG AA against the cream — see the contrast note above.
- Keyboard navigable, with a skip link and a visible 2px `--ink` focus ring.
  `outline: none` appears nowhere.
- `prefers-reduced-motion: reduce` stops the wordmark cycling and the image
  brightness transition.
- Speciality dots carry screen-reader text, so the meaning is not colour-only.

### Fonts and licensing

All six faces are free and served from Google Fonts. Nothing is downloaded into
this repository, so there is nothing to keep licence files for.

The two faces that paint first, DM Mono 400 and Italianno, are preloaded by URL
in `<head>`. Those URLs contain a version number (`v16`, `v18`). When Google
bumps a version the preload stops matching and the browser quietly downloads the
new file instead — the page still works and looks right, it just loses a little
speed. Worth re-checking once a year: load
`https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&family=Italianno&display=swap`
in a browser and copy the current URLs for the `U+0000-00FF` latin blocks. Every
other face is `font-display: swap`.

**Upgrade path to the paid originals.** The design this page follows uses two
commercial faces: **Baraquiel** (Scriptorium, via MyFonts) for the script and
**ABC Laica Mono** (Dinamo) for the body. If the restaurant licenses them,
swapping is a `@font-face` change and a variable rename: self-host the licensed
files, declare them, and change the font stacks in `styles.css`. The script
variants would need re-measuring, since the corrections are specific to each
face's metrics. No font file belonging to another restaurant's site was
downloaded or referenced for this build, and none should be.

### Layout

Full-bleed page, content in a 1280px container with a 24px gutter. Tested at
375px, 768px, 1440px, and holds from 320px to 2560px. The image grid is twelve
columns and two rows on a desktop, two columns on a tablet, one column on a
phone, with an 8px gutter throughout.

## Writing for this page

House rules, for whoever edits the copy next. South African English. No Oxford
comma. Rand if prices ever appear.

Do not write "authentic", "nestled", "hidden gem", "culinary journey", "a taste
of Greece", "transport you to the Mediterranean", or "elevated". Every competing
restaurant site in Cape Town uses them, which is exactly why this one does not.

The street is **Barnet**, one T, matching Google Maps and the official listing.
Most local directories spell it Barnett. They are wrong.

The apostrophe in "Since the 1950's" in the hero is the restaurant's own, taken
from their Facebook bio. It is not a typo to fix.

No prices, dish descriptions, awards or quotes on this page are invented. Keep it
that way: if it is not verified, leave it out.

## One thing to check

The footer says the site is unofficial. If the restaurant adopts it, delete that
sentence — the `foot__fine` paragraph at the bottom of `index.html`.
