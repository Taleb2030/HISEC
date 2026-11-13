# HISEC: website (FR / EN / AR)

## Run it

```bash
npm install     # first time only
npm run dev     # dev server at http://localhost:5173 (opens the browser)
```

`npm start` does the same as `npm run dev`.

| Command | What it does |
|---|---|
| `npm run dev` / `npm start` | Generates the pages, starts the dev server, and reloads the browser on every change |
| `npm run build` | Builds the production site into `dist/` |
| `npm run preview` | Serves `dist/` locally to check the production build |
| `npm run generate` | Regenerates the HTML pages only |

## Languages

| Language | URL | Translation file |
|---|---|---|
| Français (default) | `/` | `content/fr.mjs` |
| English | `/en/` | `content/en.mjs` |
| العربية (right-to-left) | `/ar/` | `content/ar.mjs` |

To change any text, edit the translation file. While `npm run dev` is running, the pages regenerate and the browser reloads on their own. In headings, text between `*asterisks*` is shown in red.

Phone number, map and the list of solutions: `content/shared.mjs`.

**Don't edit the `.html` files by hand.** They are generated from `content/` by `scripts/generate.mjs`, and any manual change is overwritten.

## Photos

Put them in `public/assets/img/` using the names listed in `public/assets/img/LISEZ-MOI.txt`.

## Deploy

Run `npm run build`, then upload the **contents** of `dist/` to the root of the domain. Links are absolute (`/en/…`), so the site must be served from the domain root, not a subfolder.

## Structure

```
content/      translations + shared data (edit here)
scripts/      page generator
src/          main.js (GSAP, ScrollTrigger, Lenis) + styles.css
public/       images, favicon (copied as-is)
docs/         HANDOFF.md: design and development spec
*.html en/ ar/   generated pages (don't edit)
```
