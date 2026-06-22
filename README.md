# Palette Bench

A single-file, zero-build tool for laying out and judging color palettes in context.
The board is a document of **titled sections** of color cards — drag cards between
sections, add the sections you need, swap the page background, and preview your colors
under real fonts. No framework, no build step — just `index.html`.

**Live:** https://colliery-io.github.io/color-picker/

## Layout

- **Sections** — titled groups of cards. Add / rename / reorder (▲▼) / delete from the
  controls that appear when you hover a section header.
- **Per-section layout** — toggle **Grid** (swatch + name + hex tiles) or **Rows**
  (compact lines). Both show each color across UI contexts: a dot · alpha pill · dashed
  node · `Ag` text sample — matching the two card styles of a real design-system sheet.
- **Drag & drop** — grab a card (by its swatch or body) and drop it anywhere, in any
  section, in any order. An accent placeholder shows where it'll land.
- **Per-card** — hover to recolor or delete; click the name to rename. Live WCAG
  contrast ratio vs. the background on every card. Swatches bleed onto the page
  background so you read the true color-on-background edge.

## Backgrounds

A section marked as **Backgrounds** holds candidate page backgrounds — click any swatch
in it to make that color the board background, and the whole palette re-judges against
it (contrast ratios update live). Toggle the **BG** control in any section header to
promote/demote it as a Backgrounds section. Card surfaces and text auto-adapt to light
or dark backgrounds.

## Font preview

Paste a list of font families into the sidebar's **Font list** and hit **Load fonts** —
non-system families are fetched from Google Fonts. The prominent picker at the top of
the board switches the active font, previewed live on every card name and `Ag` sample.

## URL scheme

State lives entirely in the URL — bookmarkable, shareable, and agent-writable. The
sidebar's **Copy agent brief** button copies a ready-to-paste version of this spec for
handing to an AI agent.

### Quick form (hand-write this)
```
?bg=0e1116&p=7fb2ff,4bd07f,f06464&t=ff8787,3bc9db
```
- `bg` — background hex (no `#`)
- `p` — builds a **Palette** section (grid) from these colors
- `t` — builds a **Try** section (rows) from these colors

### Full form (what "Copy share link" emits)
```
?d=<base64url token>
```
`d` is a base64url-encoded JSON snapshot that preserves every section, title, layout,
color name, and the font list:

```json
{
  "bg": "#0e1116",
  "ft": ["Inter", "Roboto Slab"],
  "f":  "Inter",
  "s": [
    {
      "t": "Accents",
      "l": "g",
      "r": "bg",
      "c": [ { "h": "#7fb2ff", "n": "ice" } ]
    }
  ]
}
```

| Key | Meaning |
| --- | --- |
| `bg` | page background hex |
| `ft` | optional list of Google Font families to offer |
| `f`  | optional active preview font |
| `s`  | sections, top to bottom |
| `s[].t` | section title |
| `s[].l` | layout — `g` grid, `r` rows |
| `s[].r` | optional — `bg` marks a Backgrounds section |
| `s[].c` | cards — `h` hex, `n` name |

Encode:
```js
const token = btoa(unescape(encodeURIComponent(JSON.stringify(data))))
  .replace(/\+/g,'-').replace(/\//g,'_').replace(/=+$/,'');
// → ?d=<token>
```

> **For AI agents:** the quick `?bg=…&p=…&t=…` form is the easy path. Use the `d=`
> token when you need named colors, multiple custom sections, or fonts.

## Deploy

The live site is served by GitHub Pages via the Actions workflow in
`.github/workflows/pages.yml` — every push to `main` redeploys the repo root. To run
locally, just open `index.html` (no server needed).
