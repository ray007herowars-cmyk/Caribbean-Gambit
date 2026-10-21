# ⚓ Caribbean Gambit

**A pirate-themed chess ladder that runs in a single HTML file.** Sail through
twenty AI captains, each tougher than the last. Every win earns a ship badge, and
as you win your own ship grows from a Wicked Wench to the Flying Dutchman.

![The Fleet page](docs/screenshots/fleet.png)

> Fan-made, non-commercial tribute. Not affiliated with Disney. See [NOTICE.md](NOTICE.md).

---

## Contents

- [Features](#features)
- [Quick start](#quick-start)
- [Screenshots](#screenshots)
- [How the game works](#how-the-game-works)
- [The twenty captains](#the-twenty-captains)
- [Ranks and badges](#ranks-and-badges)
- [How the AI works](#how-the-ai-works)
- [Tech stack and project layout](#tech-stack-and-project-layout)
- [Documentation](#documentation)
- [Limitations](#limitations)
- [Roadmap ideas](#roadmap-ideas)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **Full chess rules** via chess.js: castling, en passant, promotion, check, checkmate, stalemate, repetition, fifty-move rule and insufficient material.
- **20 AI captains** on a smooth difficulty ladder, from a captain who blunders often to one that searches five moves ahead. Each has a title, taunt and badge.
- **Your ship is your rank.** Nine ranks, from Castaway to Admiral of the Seven Seas, each with its own ship.
- **20 collectable badges.** Each badge is a medal showing the ship of its band, with the captain's emblem on a seal.
- **Online-style board:** clocks with increment (1+0, 3+2, 5+0, 10+0, 15+10, or no clock), captured pieces, material count, evaluation bar, move list, last-move and check highlights, legal-move dots, flip board.
- **Naval duel strip:** your ship faces the enemy's flag above the board. Hull bars track material, ships shake on captures and the loser sinks at the end.
- **Handmade artwork:** nine detailed ship illustrations (wood grain, cloth sails, rigging, gun ports, ghost ships), ornate medals, cameo portraits, compass roses and a moonlit header scene. All procedural SVG, with no image files.
- **Four pages** inside one file: The Fleet, Quarterdeck, Treasury and Ship's Articles.
- **Midnight and Dusk** dark-blue themes.
- **Progress saved in your browser** (localStorage). No accounts, no server.
- **Responsive**, with tap-to-move on phones.

## Quick start

There is nothing to build or install.

**Option 1: just open it**

1. Download `index.html`.
2. Double-click it to open it in a modern browser (needs an internet connection, see below).

**Option 2: run a local server**

```bash
git clone https://github.com/<your-username>/caribbean-gambit.git
cd caribbean-gambit
python3 -m http.server 8000
# open http://localhost:8000
```

**Option 3: deploy it free** on Vercel or GitHub Pages. See [INSTRUCTIONS.md](INSTRUCTIONS.md#deploy-with-vercel).

> **Internet required:** the chess rules library (chess.js) is loaded from the cdnjs CDN and the fonts from Google Fonts. Without a connection the board will not work. See [INSTRUCTIONS.md](INSTRUCTIONS.md#run-fully-offline) to bundle chess.js locally.

## Screenshots

| The Fleet | Quarterdeck | Treasury |
|---|---|---|
| ![Fleet](docs/screenshots/fleet.png) | ![Quarterdeck](docs/screenshots/quarterdeck.png) | ![Treasury](docs/screenshots/treasury.png) |

*Screenshots were captured in headless Chromium with progress for eight beaten captains.*

## How the game works

1. Open **The Fleet** and press **Challenge** on the first captain.
2. Play the battle on the **Quarterdeck**. You choose colour and time control.
3. **Win** to claim the captain's badge and unlock the next captain. A draw or a loss lets you rematch.
4. Beat enough captains and your ship is **promoted**.
5. Check your collection in the **Treasury**.

The full walkthrough is in the [User Guide](USER_GUIDE.md).

## The twenty captains

Captains unlock in order. "Band ship" is the ship shown on that captain's badge.

| # | Captain | Title | Badge | Band ship |
|---|---|---|---|---|
| 1 | Joshamee Gibbs | Quartermaster of the Pearl | First Anchor | HMS Interceptor |
| 2 | Weatherby Swann | Governor of Port Royal | Governor's Seal | HMS Interceptor |
| 3 | Henry Turner | Seeker of the Trident | Mapmaker's Oath | HMS Providence |
| 4 | Syrena | Mermaid of the Lagoon | Siren's Tear | HMS Providence |
| 5 | Mistress Ching | Pirate Lord of the South China Sea | Dragon of the South Seas | HMS Providence |
| 6 | Captain Teague | Keeper of the Code | Code Keeper | HMS Dauntless |
| 7 | Elizabeth Swann | Pirate King | Pirate King's Crown | HMS Dauntless |
| 8 | Angelica Teach | Daughter of Blackbeard | Rose Blade | HMS Endeavour |
| 9 | Juan Ponce de León | Explorer of the Fountain of Youth | Fountain's Chalice | HMS Endeavour |
| 10 | Sao Feng | Pirate Lord of Singapore | Lantern of Singapore | HMS Endeavour |
| 11 | James Norrington | Commodore, Royal Navy | Commodore's Medal | Queen Anne's Revenge |
| 12 | Will Turner | Captain of the Flying Dutchman | Heart of the Dutchman | Queen Anne's Revenge |
| 13 | Armando Salazar | Terror of the Silent Mary | Shroud of the Silent Mary | Silent Mary |
| 14 | Tia Dalma | Sea Witch of the Bayou | Sea Witch's Charm | Silent Mary |
| 15 | Hector Barbossa | Pirate Lord, Master of Cunning | Cursed Apple | Silent Mary |
| 16 | Edward "Blackbeard" Teach | Captain of the Queen Anne's Revenge | Beard of Smoke | Black Pearl |
| 17 | Lord Cutler Beckett | Chairman of the Trading Company | Company Ledger | Black Pearl |
| 18 | Jack Sparrow | Captain of the Black Pearl | Compass of Desire | Flying Dutchman |
| 19 | Davy Jones | Master of the Flying Dutchman | Kraken's Eye | Flying Dutchman |
| 20 | Calypso | Goddess of the Sea | Trident of the Tide | Flying Dutchman |

## Ranks and badges

Your rank depends only on **how many different captains you have beaten**. You are never demoted.

| Captains beaten | Rank | Your ship |
|---|---|---|
| 0 | Castaway | Wicked Wench |
| 2 | Deckhand | HMS Interceptor |
| 5 | Buccaneer | HMS Providence |
| 7 | Privateer | HMS Dauntless |
| 10 | Corsair | HMS Endeavour |
| 12 | Captain | Queen Anne's Revenge |
| 15 | Commodore | Silent Mary |
| 17 | Pirate Lord | Black Pearl |
| 20 | Admiral of the Seven Seas | Flying Dutchman |

Each badge shows the ship of the band that captain belongs to: captains 1–2 earn Interceptor badges, 3–5 Providence, 6–7 Dauntless, 8–10 Endeavour, 11–12 Queen Anne's Revenge, 13–15 Silent Mary, 16–17 Black Pearl and 18–20 Flying Dutchman.

## How the AI works

The opponent is a small original engine written in JavaScript, on top of chess.js for the rules:

- **Negamax search with alpha-beta pruning** and **iterative deepening** with a time limit per move.
- **Quiescence search** (up to 4 plies of captures and promotions) to avoid misjudging trades.
- **Move ordering** that tries big captures first, which makes pruning much more effective.
- **Evaluation** = material, plus bonuses for central pawns and advanced pawns, centralised knights and bishops, rooks on the seventh rank, and king safety (or king activity once queens are off).
- **Personality through noise:** weaker captains add random noise to move scores so they sometimes pick worse moves.

| Captains | Search depth | Noise | Notes |
|---|---|---|---|
| 1–3 | 1 | 320 → 240 | Frequent blunders |
| 4–8 | 2 | 210 → 100 | Casual club-level mistakes |
| 9–15 | 3 | 80 → 10 | Solid, occasionally slips |
| 16–19 | 4 | 5 → 0 | Plays its best move found |
| 20 | 5 | 0 | Strongest, up to about 4.4 s per move |

The "Threat 1 to 20" number is the project's own difficulty scale. **It is not an Elo or FIDE rating.** This engine is far weaker than Stockfish: it has no opening book, no transposition table and no endgame knowledge.

## Tech stack and project layout

- **One file:** HTML, CSS and JavaScript in `index.html`. No build step, no framework, no bundler.
- **Rules:** [chess.js 0.10.3](https://github.com/jhlywa/chess.js) from cdnjs.
- **Fonts:** Pirata One, Cinzel and IM Fell English from Google Fonts, with system serif fallbacks.
- **Art:** inline SVG generated by JavaScript functions, using SVG filters for wood grain and cloth.
- **Storage:** `localStorage` under the key `caribbean`.

```
caribbean-gambit/
├── index.html              the entire game
├── README.md               this file
├── USER_GUIDE.md           how to play, in detail
├── INSTRUCTIONS.md         setup, deploy, customise, troubleshoot
├── LICENSE                 MIT
├── NOTICE.md               trademark disclaimer and third-party notices
└── docs/screenshots/       images used in the README
```

## Documentation

| Document | For |
|---|---|
| [USER_GUIDE.md](USER_GUIDE.md) | Players: every screen, button and rule |
| [INSTRUCTIONS.md](INSTRUCTIONS.md) | Developers: running, hosting, offline mode, customising captains, ships and colours, troubleshooting |
| [NOTICE.md](NOTICE.md) | Trademark disclaimer and third-party licences |

## Limitations

Being upfront about what this is and is not:

- **Single player only.** There is no multiplayer, accounts or online ranking. All "online chess" features are local.
- **Pawn promotion is always to a queen.**
- **The board uses tap or click only.** There is no drag-and-drop and no keyboard play yet.
- **Needs internet** for chess.js and fonts unless you bundle chess.js yourself.
- **Progress lives in one browser.** Clearing site data or using private browsing loses it. There is no export yet.
- **Engine strength is modest**, and the AI can run for several seconds on the hardest captains.
- **No automated tests.** The project has been checked by hand and by rendering pages in headless Chromium. Other browsers are expected to work but have not been formally tested.
- **No sound.**

## Roadmap ideas

- Drag-and-drop and keyboard move entry
- Promotion picker (knight, bishop, rook, queen)
- Export and import of progress
- Sound effects (cannon fire, bells)
- Opening archive page that launches a position from a named opening
- Stockfish (WASM) as an optional stronger engine
- Per-captain play styles (aggressive, defensive)
- Unit tests for the engine

## Contributing

Issues and pull requests are welcome. Please keep the project dependency-free and single-file unless there is a strong reason. For code changes, test at least one full game on desktop and on a phone-sized viewport. See [INSTRUCTIONS.md](INSTRUCTIONS.md#customise-the-game) for where things live in the file.

## License

Code and original artwork are released under the [MIT License](LICENSE). Franchise names, characters and trademarks are **not** covered by that licence; see [NOTICE.md](NOTICE.md).
