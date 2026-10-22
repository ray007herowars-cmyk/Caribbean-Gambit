# Changelog

All notable changes to this project are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [1.0.0] - 2026-10-22

### Added

- Single-player pirate chess ladder with 20 AI captains of rising strength, each with a title, taunt and badge.
- Nine ship ranks (Wicked Wench up to the Flying Dutchman), earned by number of captains beaten, with a promotion panel.
- 20 collectable medal badges showing the ship of each captain's band, plus the Treasury page with the full rank ladder.
- Four pages in one file (The Fleet, Quarterdeck, Treasury, Ship's Articles) with hash routing.
- Online-style board with clocks and increments, captured pieces, evaluation bar, move list, takeback, draw offers, resign and board flip.
- Naval duel strip with hull bars, capture animation and a sinking ship at game end.
- Handmade procedural SVG artwork: nine detailed ships, medals, cameo portraits, compass roses and a moonlit header scene.
- Midnight and Dusk dark-blue themes, responsive layout and reduced-motion support.
- Progress saved in `localStorage`.
- Built-in chess rules engine, verified against the standard perft reference counts, so the game has **no external script dependencies** and works offline (fonts fall back to system fonts).
- On-screen error banner and `<noscript>` message, so a failure is never a silent blank page.
- Deployment and repository files: Vercel configuration, web manifest, icons, social preview image, 404 page, automated tests, CI workflow, issue and pull request templates.

### Fixed

- Opening the Quarterdeck directly (a refresh on `#/play`, or the nav button before choosing a captain) left a board that did not respond. A game now starts automatically.

[Unreleased]: https://github.com/OWNER/REPO/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/OWNER/REPO/releases/tag/v1.0.0
