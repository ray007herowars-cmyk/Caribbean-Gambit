# Instructions: Run, Deploy, Customise

Developer-oriented instructions for **Caribbean Gambit**. For how to play, see the [User Guide](USER_GUIDE.md).

## Contents

- [Requirements](#requirements)
- [Run locally](#run-locally)
- [Deploy with Vercel](#deploy-with-vercel)
- [Deploy with GitHub Pages](#deploy-with-github-pages)
- [Run fully offline](#run-fully-offline)
- [File structure](#file-structure-of-indexhtml)
- [Customise the game](#customise-the-game)
- [Saved data](#saved-data)
- [Testing and debugging](#testing-and-debugging)
- [Troubleshooting](#troubleshooting)

---

## Requirements

- A current desktop or mobile browser (Chrome, Edge, Firefox or Safari). The game uses modern CSS (`inset`, `translate`, `clip-path`, `paint-order`) and SVG filters. It has been rendered and checked in Chromium; other browsers are expected to work.
- Internet access for the chess.js library (cdnjs) and Google Fonts, unless you [bundle chess.js](#run-fully-offline).
- Nothing else. No Node, npm or build tools.

## Run locally

**Quickest:** double-click `index.html`.

**With a local server** (recommended, avoids any `file://` quirks):

```bash
# Python 3
python3 -m http.server 8000

# or Node
npx serve .
```

Then open `http://localhost:8000`.

## Deploy with Vercel

The game is a static site, so Vercel needs no build step and no `vercel.json`.

**From the dashboard (recommended)**

1. Push this folder to a GitHub repository. Make sure `index.html` is at the **repository root**.
2. Go to [vercel.com](https://vercel.com), sign in, and choose **Add New → Project**.
3. Import your repository.
4. Set **Framework Preset** to **Other**. Leave **Build Command** and **Output Directory** empty (or default) and leave **Install Command** empty.
5. Press **Deploy**. Your game goes live at `https://<project-name>.vercel.app`.

Every push to your main branch redeploys automatically, and every pull request gets its own preview URL.

**From the command line**

```bash
npm i -g vercel
cd caribbean-gambit
vercel          # first run: preview deployment, answers the setup questions
vercel --prod   # deploy to production
```

**Notes**

- **No rewrites needed.** Pages use URL hashes (`#/fleet`, `#/play`, ...), so deep links and the Back button work without any server routing.
- **Progress is per address.** Browser storage belongs to the exact origin, so players' progress on a preview URL, on `*.vercel.app` and on a custom domain is stored separately. Pick your final domain before sharing the game widely.
- **Custom domain:** Project → Settings → Domains → Add.
- **Content-Security-Policy:** if you add security headers, allow `cdnjs.cloudflare.com` (scripts), `fonts.googleapis.com` (styles) and `fonts.gstatic.com` (fonts), and allow inline scripts and styles, because the game is one file. Otherwise the board or fonts will fail to load.

## Deploy with GitHub Pages

1. Create a GitHub repository (for example `caribbean-gambit`) and push the files in this folder. Make sure the game file is named **`index.html`** at the repository root.
2. In the repository go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select branch **main** and folder **/ (root)**, then **Save**.
5. After a minute your game is live at `https://<your-username>.github.io/<repository-name>/`.

Pages are routed with the URL hash (`#/fleet`, `#/play`, `#/treasury`, `#/articles`), so no server rewrite rules are needed and deep links work.

Other static hosts (Netlify, Cloudflare Pages, Vercel, any web server) work the same way: serve `index.html` as-is.

## Run fully offline

The game loads chess.js from a CDN. To remove that dependency:

1. Download `chess.min.js` from the cdnjs URL used in `index.html`:
   `https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js`
2. Save it next to `index.html` as `chess.min.js`.
3. In `index.html`, replace the `<script src="https://cdnjs...chess.min.js">` tag with:
   ```html
   <script src="chess.min.js"></script>
   ```
4. Keep the version at **0.10.3**. Newer chess.js versions renamed functions (for example `in_check` became `isCheck`) and this code uses the old API.

Fonts are optional: without Google Fonts the page falls back to system serif fonts. To self-host them, download the three font families (Pirata One, Cinzel, IM Fell English) and replace the Google Fonts `<link>` with local `@font-face` rules.

## File structure of `index.html`

Everything is in one file. Use your editor's search to jump between sections.

| Section | How to find it | What it does |
|---|---|---|
| `<style>` | top of file | Theme tokens (`:root` colour variables), layout and component styles |
| HTML body | after `</style>` | Header, navigation, the four pages and the result overlay |
| Roster data | `const OPP=[` | The 20 captains |
| Difficulty data | `const DD=` and `ZZ=` | Search depth and noise per captain |
| Time controls | `const TC=` | Clock options |
| Art | `const DEFS=`, `const SP=`, `function shipG`, `badgeSvg`, `portraitSvg`, `roseSvg`, `sceneSvg` | All the SVG artwork |
| Ranks | `const RANKS=` and `BC=` | Ship ranks, thresholds and badge colours |
| AI | `/* ---------- AI ---------- */` | Evaluation and search |
| Game flow | `/* ---------- game flow ---------- */` | Start, move, finish, clocks |
| Rendering | `/* ---------- rendering ---------- */` | Board, strips, opponent card |
| Pages | `/* ---------- pages ---------- */` | Fleet, Treasury, Articles, routing |
| Input | `/* ---------- input ---------- */` | Click handling and buttons |

## Customise the game

### Change a captain

Each entry in `OPP` is:

```js
['Name', 'Title', 'Emoji', 'Taunt line', 'Badge name']
```

Edit the text freely. The emoji is used for the portrait and badge seal. Use a colour emoji (append `\uFE0F` if a symbol renders as a flat black glyph).

### Change difficulty

In `index.html`:

```js
const DD=[1,1,1,2,2,2,2,2,3,3,3,3,3,3,3,4,4,4,4,5];     // search depth per captain
ZZ=[320,280,240,210,180,150,120,100,80,60,45,35,25,15,10,5,0,0,0,0]; // noise per captain
```

- **`DD`** is the search depth in plies. Higher is stronger and slower. Depths above 5 may be too slow without a transposition table.
- **`ZZ`** is random noise in centipawns added to each root move score. Higher means more mistakes. Use `0` for the strongest play.
- The per-move time cap is set just below them: `o.t=1200+i*170` (milliseconds). Raise or lower it to make the hardest captains faster or slower.

### Change the number of captains

The number 20 appears in several places (`Array(20)` for saved progress, `*5` for the threat bar width, the `RANKS` thresholds and band logic, and text such as "of 20"). If you add or remove captains, search for `20` and `*5`, and update `RANKS` thresholds so that the last rank equals your captain count.

### Change ranks and ships

`RANKS` rows are `[captainsNeeded, 'Rank title', 'Ship name', 'shipId']`. The `shipId` must exist in the `SP` table. Badge bands are derived automatically from the thresholds by `bandOf()`.

### Add or restyle a ship

Ships are defined in the `SP` table, for example:

```js
pearl:{n:3,h:'#0a0a12',t:'#7fb4ff',s:'#171826',gp:2,f:'jolly',z:1.04,rim:1}
```

| Key | Meaning |
|---|---|
| `n` | Number of masts (2 or 3) |
| `h` | Hull colour |
| `t` | Trim colour (wales, port outlines, rigging highlight) |
| `s` | Sail colour |
| `gp` | Rows of gun ports (0–3) |
| `f` | Flag type: `navy`, `plain`, `jolly`, `ghost` |
| `z` | Overall scale |
| `tat` | Tattered, holed sails |
| `ghost` | Glow, translucent hull, ribs |
| `barn` | Barnacles on the hull |
| `rim` | Light-coloured sail outline (good for black sails) |
| `patch` | Patched sails |

Add a new entry, then use its id in a `RANKS` row.

### Change colours and themes

Colours are CSS variables at the top of the `<style>` block (`--s1`, `--s2`, `--c1`, `--c2`, `--ink`, `--gold`, `--lt`, `--dk`, `--wp`, `--bp`, and so on). The default is the Midnight theme. A lighter Dusk theme is defined by `:root[data-theme="dusk"]` (and by the light-mode media query). The board's light and dark squares are `--lt` and `--dk`. White and black piece colours are `--wp` and `--bp`.

### Change the opponent flag, ticker slogans or header

- Ticker slogans: the `SL` array.
- Header scene: `sceneSvg()`.
- Compass roses: `roseSvg()`.
- Enemy flag in the duel strip: the `FLAG` constant.

### Change the draw-offer rule

In the `#bDraw` click handler: a draw is accepted when `g.history().length>=16` and the captain's evaluation is `<=60` centipawns.

### Add a promotion choice

Promotion is hard-coded in the `play()` function (`promotion:m.promotion||'q'`). To add a picker, detect a pawn reaching the last rank in the board click handler, show a small dialog, and pass the chosen piece letter (`q`, `r`, `b` or `n`) as `promotion`.

## Saved data

Progress is stored in `localStorage` under the key `caribbean`:

```json
{"beat":[1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"all":0,"w":3,"d":0,"l":1}
```

| Field | Meaning |
|---|---|
| `beat` | 20 flags, 1 when that captain has been defeated |
| `all` | 1 if "Chart all seas" (unlock everyone) is on |
| `w`, `d`, `l` | Total wins, draws and losses |

To reset from the browser console: `localStorage.removeItem('caribbean'); location.reload()`.

To grant yourself progress for testing, run for example:

```js
localStorage.setItem('caribbean', JSON.stringify({beat:Array(20).fill(1),all:0,w:20,d:0,l:0})); location.reload()
```

## Testing and debugging

There is no automated test suite yet. Suggested manual checks after changes:

1. Play one full game as White and one as Black.
2. Try castling, en passant, promotion, check and checkmate.
3. Test each time control, including ∞, and let a clock run out.
4. Beat a captain and confirm the badge appears and the next captain unlocks.
5. Cross a rank threshold (use the console snippet above) and check the **PROMOTED!** panel.
6. Reload the page and confirm progress persists.
7. Test on a phone-sized viewport.

Open the browser developer console to see errors. If the console reports `Chess is not defined`, the chess.js script did not load.

**Rendering the art in a headless browser** (optional, for screenshots) with Playwright:

```bash
pip install playwright && playwright install chromium
```

```python
from playwright.sync_api import sync_playwright
with sync_playwright() as p:
    b = p.chromium.launch(); pg = b.new_page(viewport={'width':1200,'height':1500})
    pg.goto('file:///path/to/index.html#/treasury'); pg.wait_for_timeout(800)
    pg.screenshot(path='treasury.png'); b.close()
```

## Troubleshooting

| Symptom | Fix |
|---|---|
| Blank board, console says `Chess is not defined` | chess.js failed to load. Check the network, or use the [offline setup](#run-fully-offline). |
| `in_check is not a function` after swapping chess.js | You installed a newer chess.js. Use version 0.10.3. |
| GitHub Pages shows a 404 | The file must be named `index.html` and be in the branch root you selected in Settings → Pages. |
| Navigation links do nothing on a custom host | Make sure nothing rewrites URLs. The game relies on the `#` hash and needs no server routing. |
| Progress is not saved | The browser blocks `localStorage` (private mode or strict settings). |
| Layout overflows on small screens | Check for a browser zoom or custom viewport. The board size is `min(10.2vw, 62px)` per square. |
