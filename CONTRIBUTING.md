# Contributing to Caribbean Gambit

Thanks for wanting to help. This is a small hobby project, so the process is light.

## Ground rules

- **Keep it a single, self-contained `index.html`.** The game must work with no build step and no network. Please do not add external scripts, frameworks or CDN dependencies. Google Fonts are the only external request, and the game works without them.
- **Original assets only.** Do not add film frames, logos, posters, music, voice lines or other third-party artwork or quotes. Artwork is generated in code. Captain taunts must be original text.
- **Be kind.** See the [Code of Conduct](CODE_OF_CONDUCT.md).

## Getting set up

You need [Node.js](https://nodejs.org) 18 or newer (the `.nvmrc` pins 20). There are no dependencies to install.

```bash
git clone <your fork URL>
cd caribbean-gambit
npm start        # serves the game at http://localhost:3000
npm test         # runs the automated tests
```

You can also just open `index.html` in a browser.

## Where things live

`index.html` contains everything. The two parts covered by automated tests are fenced by marker comments:

| Markers | Contents |
|---|---|
| `/* ENGINE-START */ ... /* ENGINE-END */` | The chess rules engine (`Chess` class) |
| `/* AI-START */ ... /* AI-END */` | Evaluation and search used by the captains |

**Do not remove or rename these markers.** The tests extract the code between them.

For a tour of the rest of the file (roster, ranks, ships, themes) see the "Customise the game" section of `INSTRUCTIONS.md`.

## Tests

`npm test` runs the Node built-in test runner (it discovers every `*.test.js` file):

- `engine.test.js`: perft counts from the standard reference suite, SAN, castling, en passant, promotion, stalemate, repetition and draw rules, make/unmake.
- `ai.test.js`: the AI finds a mate in one, takes a free queen, only plays legal moves, and respects its time limit.
- `structure.test.js`: the page has its four routes, 20 captains, 9 ranks, no external scripts, and all referenced files exist.

If you change the engine or AI, add or update a test. If you change the roster or ranks, `structure.test.js` may need updating too.

## Manual checklist for gameplay or visual changes

Automated tests do not cover the UI. Before opening a pull request, please:

1. Play a full game as White and as Black.
2. Try castling, en passant, promotion, check and checkmate.
3. Test one timed control and let the clock run out, and test the no-clock option.
4. Beat a captain and confirm the badge appears and the next captain unlocks.
5. Reload and confirm progress persists.
6. Open the Quarterdeck directly (`/#/play`) and confirm the board responds.
7. Check a phone-sized viewport and both themes (Midnight and Dusk).

## Pull requests

1. Fork the repository and create a branch from `main`.
2. Keep the change focused. Several small pull requests are easier to review than one large one.
3. Fill in the pull request template and attach screenshots for visual changes.
4. Make sure `npm test` passes. The same tests run in GitHub Actions.
5. Add a line under **Unreleased** in `CHANGELOG.md`.

## Reporting bugs and ideas

Use the issue templates. For a bug, include your browser and device, steps to reproduce, and the text of the red error banner if the game shows one. Security problems: see [SECURITY.md](SECURITY.md) instead of opening a public issue.

## Licence

By contributing you agree that your contribution is released under the project's [MIT License](LICENSE).
