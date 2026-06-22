# Palette Bench

A single-file, zero-build tool for laying out and judging color palettes in context.
The board is a document of **titled sections** of color cards — drag cards between
sections, add the sections you need, and see each color rendered as a swatch, dot,
pill, dashed node, and text sample. No framework, no build step — just `index.html`.

## Layout

- **Sections** — titled groups of cards. Add / rename / reorder (▲▼) / delete from the
  controls that appear when you hover a section header.
- **Per-section layout** — toggle **Grid** (compact swatch + name + hex cards) or
  **Rows** (each color shown across UI contexts: dot · alpha pill · dashed node · `Ag`
  text sample), matching the two card styles from a real design system sheet.
- **Drag & drop** — grab a card (by its swatch or body) and drop it anywhere, in any
  section, in any order. An accent placeholder shows where it'll land.
- **Per-card** — hover to recolor or delete; click the name to rename. Live WCAG
  contrast ratio vs. the background on every card.
- **Background** — set by picker or hex; card surfaces and text auto-adapt to light or
  dark backgrounds so contrast reads honestly.

## URL scheme

State lives entirely in the URL — bookmarkable, shareable, and agent-writable.

### Simple (hand-write this)
```
index.html?bg=0e1116&p=7fb2ff,4bd07f,f06464&t=ff8787,3bc9db
```
- `bg` — background hex (no `#`)
- `p` — builds a **Palette** section (grid) from these colors
- `t` — builds a **Try** section (rows) from these colors

### Full (what "Copy share link" emits)
```
index.html?d=<base64url token>
```
`d` is a base64url-encoded JSON snapshot — `{ bg, s:[{ t:title, l:'g'|'r', c:[{h,n}] }] }` —
that preserves every section, title, layout, and color name. Decoding/encoding:

```js
const data = { bg, s: [{ t: "Accents", l: "g", c: [{ h: "#7fb2ff", n: "ice" }] }] };
const token = btoa(unescape(encodeURIComponent(JSON.stringify(data))))
  .replace(/\+/g,'-').replace(/\//g,'_').replace(/=+$/,'');
// → index.html?d=<token>
```

> **For AI agents:** the simple `?bg=…&p=…&t=…` form is the easy path. Use the `d=`
> token only when you need named colors or multiple custom sections.

## Deploy to GitHub Pages

```bash
git init
git add index.html README.md
git commit -m "Palette Bench"
git branch -M main
git remote add origin git@github.com:<you>/palette-bench.git
git push -u origin main
```

Then: **Settings → Pages → Source: Deploy from a branch → `main` / root**.
Live at `https://<you>.github.io/palette-bench/`. (Or just open `index.html` locally.)
