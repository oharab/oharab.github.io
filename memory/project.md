---
name: Rehab project overview
description: High-level context for Ben's rehab, marathon training plan, and website — phase, key files, current state, and site architecture
type: project
---

# Project: Ben's rehab → London Marathon 2027

## Key files
- `CLAUDE.md` — steering document (profile, constraints, injury history, red flags, session file format). Read this first.
- `plan.md` — objectives hierarchy (long/medium/short term), phase gates, session log index.
- `_sessions/` — one file per session, named `YYYY-MM-DD-session-N.md`. Underscore prefix required by Jekyll.
- `memory/` — this memory store. Update after every significant decision.

## Website
The repo is a Jekyll site hosted on GitHub Pages at `rehab.robotparade.co.uk` (repo: `oharab/oharab.github.io`, branch: `main`).

- `_config.yml` — Jekyll config. Sessions collection has `output: false` (no individual pages).
- `_layouts/default.html` — single layout with all CSS and the auth modal.
- `index.md` — home page. Date-aware JS picks today's session or next upcoming. All interactivity lives here.
- `plan.md` — rendered as `/plan/`.
- `history.md` — rendered as `/history/`, lists all sessions.
- `.github/workflows/daily-rebuild.yml` — rebuilds the site at 04:00 UTC daily.

### Site interactivity (all client-side JS in index.md)
- **Checkboxes** — `[ ]`/`[x]` cells in exercise tables become tappable checkboxes.
- **Demo links** — plain YouTube URLs in `Demo` column cells become ▶ buttons opening in a new tab.
- **Session log inputs** — Value cells in `Field | Value` tables become inline text inputs.
- **Knee rating inputs** — `\_\_ / 10` inline prompts become number inputs (0–10).
- **Commits** — all changes are debounced (1.2s) and committed to the session markdown file via GitHub Contents API (GET for SHA + PUT).
- **Auth** — GitHub PAT entered once via modal, stored in `localStorage`.

## Current state (as of 2026-04-29)
- Phase 1: rehab / movement screen — pre-programming.
- Sessions completed: 0 (stretch test session used to verify checkbox system).
- Session 1 planned: 2026-04-30.

## Open blockers
- Many `<!-- placeholder -->` fields in `CLAUDE.md` still unfilled (diastasis severity, physio details, GP clearance, programming preferences, red flag thresholds).
- No physio exercises prescribed yet.
- Phase 1→2 gate criteria not yet assessed.

**Why:** Pre-programming baseline phase. No training load set yet.
**How to apply:** Always read `CLAUDE.md` and `plan.md` before generating session plans. Run `git pull --rebase` before creating new session files. Update this memory file and `plan.md` after each session or significant decision.
