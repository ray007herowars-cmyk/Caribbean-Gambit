# Security Policy

## What this project is

Caribbean Gambit is a static, single-player browser game. It has no server code, no accounts, no database and no analytics. It stores game progress only in your own browser (`localStorage`) and makes no network requests of its own, apart from optionally loading Google Fonts.

## Supported versions

Only the latest release on the `main` branch receives fixes.

## Reporting a vulnerability

Please **do not open a public issue** for a security problem.

Use GitHub's private reporting instead: go to the repository's **Security** tab and choose **Report a vulnerability**. Include what you found, how to reproduce it, and what you think the impact is.

You can expect an acknowledgement within a few days. This is a hobby project maintained in spare time, so fixes may take longer for complex issues, but reports are taken seriously and you will be credited if you wish.

## Scope

Examples of things worth reporting:

- A way to run attacker-controlled script through the game's pages or URL hash
- A dependency or deployment setting that exposes visitors to risk

Out of scope: weaknesses that need the player to edit their own browser storage or developer console, since they only affect that player's own save, and the AI being beatable.

## Hardening notes for people deploying their own copy

- Serve the site over HTTPS (Vercel and GitHub Pages do this by default).
- If you add a `Content-Security-Policy` header, allow inline scripts and styles (the game is one file) and, if you want the fonts, `fonts.googleapis.com` for styles and `fonts.gstatic.com` for fonts. Everything else can stay `'self'`.
