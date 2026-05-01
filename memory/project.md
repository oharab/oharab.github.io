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

## Current state (as of 2026-05-01)
- Phase 1: rehab / movement screen — Session 1 complete, draft sessions 2–5 in place.
- Sessions completed: 1 (2026-04-30). 24h-after log filled in 2026-05-01.
- Next session: Session 2 — Saturday 3 May 2026.

## Session 1 → forward signals
- **Knee:** 2/10 during, **1/10 at 24h** — recovered, not aggravated. Current loading is well below knee tolerance; progression as planned is safe.
- **Diastasis:** Visible coning on dead bug single-arm but **controllable with bracing**. This is the operative signal — not just doming. Progression rule: hold volume if coning is visible-but-controllable; only progress when fully clean.
- **RPE:** 5–6, with explicit feedback "could go a little harder." Sessions 2–5 deliberately keep volume conservative; the new patterns are the intended load.
- **Hip flexor stretch:** Half-kneeling on right knee on hard surface = pressure pain (not joint pain). Crash mat resolves it. Standing variant is the fallback.
- **Hamstring stretch:** Felt like calf — corrected in Session 2 with slight knee bend cue.
- **Rings:** Were unavailable in Session 1, not skipped by choice. Session 2 onwards retries.

## Open blockers
- Many `<!-- placeholder -->` fields in `CLAUDE.md` still unfilled (physio details, GP clearance, progression rules, missed-session policy).
- No physio exercises prescribed yet — GP declined referral (cosmetic). Self-guided programming.
- Phase 1→2 gate criteria not yet assessed (Session 5 has the gate-check table).

**Why:** Phase 1 rehab. Self-guided due to no physio input available.
**How to apply:** Always read `CLAUDE.md` and `plan.md` before generating session plans. Run `git pull --rebase` before creating new session files. Update this memory file and `plan.md` after each session or significant decision.
