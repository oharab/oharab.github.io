# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This is not a software project. It is a personal rehab and marathon training planning document for Ben. There are no build commands, tests, or deployments.

## Key files to read at the start of every conversation

- `CLAUDE.md` — this file; the steering document. Read fully before responding to any training or rehab question.
- `plan.md` — objectives hierarchy (long/medium/short term) and phase gates. Read this to understand where we are in the plan.
- `memory/MEMORY.md` — project memory index. Check for up-to-date context.
- `_sessions/` — one file per session, named `YYYY-MM-DD-session-N.md`. Note the underscore prefix — required by Jekyll.

## How to assist in this repo

- Always read `CLAUDE.md` and `plan.md` before generating session plans or answering training questions.
- Always respect the medical scope section: defer to clinicians on anything new, sharp, or worsening. The clinician always wins over any plan in this file.
- When generating session plans, honour all constraints (days, session length, equipment, phase gates) listed in the steering document.
- Fill `<!-- placeholder -->` fields when the user provides the missing information — update in place rather than appending duplicates.
- When logging a completed session, append it under `## Current block` in the tracking format specified in the document.
- Do not invent values for placeholder fields; ask the user to supply them.

## Creating a new session file

**Always run `git pull --rebase` before creating a new session file.** Ben ticks off exercises and fills in the session log directly on the site, which commits back to the repo. The local copy will be behind if any session has been used.

Session files live in `_sessions/` and are automatically picked up by the Jekyll site at `rehab.robotparade.co.uk`.

**Filename format:** `_sessions/YYYY-MM-DD-session-N.md` (e.g. `_sessions/2026-05-01-session-2.md`)

**Required front matter:**
```yaml
---
title: "Session N — Day Date Month Year"
date: YYYY-MM-DD
---
```

**Exercise tables must include a `Done` column** with `[ ]` for every exercise row. This powers the checkbox system on the site:

```markdown
| Exercise | Sets × Reps | Load | Notes | Done |
|---|---|---|---|---|
| Glute bridge | 2 × 10 | Bodyweight | Drive through heels. | [ ] |
```

- The `Done` column must be the last column in every exercise table.
- Use `[ ]` (not done) or `[x]` (done) — exact format, including spaces.
- Assessment check tables (e.g. Block 3 diastasis checks) should also have a `Done` column.

**Every session must end with this session log table** — the Value column is editable on the site:

```markdown
## Session log

*(Fill in after the session)*

| Field | Value |
|---|---|
| Date | YYYY-MM-DD |
| Exercises completed | |
| Any skipped / modified | |
| RPE (1–10) | |
| Right knee during (0–10) | |
| Right knee 24h after (0–10) | |
| Diastasis observations | |
| Notes / next time | |
```

- The table headers must be exactly `Field` and `Value` — the site detects this to make the Value cells editable.
- Leave Value cells empty (just `| |`) — Ben fills them in on the site after the session.
- Do not add a `Done` column to this table.

**Knee rating prompts** use this exact format — the `\_\_` becomes an editable number field on the site:

```markdown
**After this block, rate right knee:** \_\_ / 10
```

**After creating the file, commit and push:**
```
git pull --rebase
git add _sessions/YYYY-MM-DD-session-N.md
git commit -m "feat: add session N — brief description"
git push
```

GitHub Pages rebuilds automatically. The new session will appear on the site within ~60 seconds.

---

# Rehab and fitness plan

Steering document for Ben's rehab, base-building, and marathon training.
Last updated: 2026-04-28

## Context

Twelve-month build from current state to completing the London Marathon (target race: Sunday 25th April 2027. Ballot has been entered.
Plan moves through three phases: rehab → general fitness base → run-specific training. Phase transitions are gated on rehab milestones, not the calendar.

## About me

- Age 50
- 183cm, 105kg
- Asthma: Generally well controlled with inhalers, but exercise is a trigger
- Movement background: some  gymnastics coaching experience (no longer actively coaching), so generally movement-competent and comfortable with strength/mobility work. Have completed triathlons and a marathon in the past, but 10 years ago.
- Current cardiovascular fitness: walk the dog for 2 miles every day.
- Current strength training: Never really lifted. Core strength generally quite floor.
- Sleep / stress baseline: Very busy work life, plus family life. Don't sleep as much as I should.
- Recently lost 2st whilst using Wegovy, but not currently taking it.

## Goals

Ranked:
1. Resolve or substantially improve long-standing knee pain
2. Complete the London Marathon, 12 months out
3. Manage diastasis recti appropriately through the build
4. Lose weight as a side effect of training (not a primary driver — no calorie targets, see Out of scope)

## Constraints

- Sessions per week: 2 or 3
- Time per session: 60-90 minuyrd
- Days/times that are fixed or off-limits: Currently at the gym 3 days a week, Monday, Thursday and Saturday, where I can have free access to the gym
- Equipment access: Fully equipped gymnastics club.
- Running surfaces available: Sprung floor, roads, some trails

## Injury history and current rehab focus

**Diastasis recti**
- Severity / gap: <!-- placeholder -->
- Currently being managed by: None
- Known aggravators: <!-- placeholder -->
- Cleared / not cleared movements: <!-- placeholder -->

**Knee pain (long-standing)**
- Which knee, or both: Right knww
- Working diagnosis: Weak  glute chain causing knee tracking to be out.
- What aggravates it: Exercise
- What helps: Rest
- What's been tried: Aquatherapy (little relief)

**Other history worth knowing**
- <!-- placeholder: old injuries, surgeries, anything else relevant -->

## Medical guidance and scope

- Physio: <!-- placeholder: name, frequency, what they've said -->
- GP: <!-- placeholder: have they signed off on marathon training? -->
- Other clinicians: <!-- placeholder -->
- Cleared activities: <!-- placeholder -->
- Not yet cleared / awaiting input on: <!-- placeholder -->

Claude is not my physio. Anything new, sharp, or worsening goes to a human first. Programming complements professional guidance — it never overrides it. If a clinician's instruction conflicts with anything in this file or in a session plan, the clinician wins.

## Programming preferences

- Level of prescription: <!-- placeholder: spelled-out sets/reps/load, or framework with autoregulation? -->
- Loading style: <!-- placeholder: RPE, %1RM, or feel-based -->
- Plan horizon: <!-- placeholder: weekly block, daily as we go, etc. -->
- Progression rules: <!-- placeholder: e.g. "advance when X for two sessions in a row" -->
- Missed sessions: <!-- placeholder: skip, shift, or compress -->
- Format I want plans delivered in: <!-- placeholder: markdown table, prose, structured per-session, etc. -->

## Red flags and back-off rules

Stop or modify the session if:
- Knee pain above <!-- placeholder: e.g. 3/10 -->, or any sharp/locking/giving-way sensation
- Diastasis doming, coning, or any visible bulging along the linea alba
- Pain that's worse 24h after a session than during it
- <!-- add others as they emerge -->

Deload triggers:
- <!-- placeholder: e.g. two consecutive sessions hitting back-off rules -->

Anything new, sharp, or escalating → stop, log, contact physio before next session.

## Tracking format

After each session I'll log:
- Session date and what was done
- Load / pace / duration
- RPE (1–10)
- Knee pain during (0–10) and 24h after (0–10)
- Diastasis observations if relevant
- Notes / what to change next time

## Out of scope

- Nutrition prescriptions, macros, or calorie targets
- Diagnosing new pain or symptoms
- Replacing or contradicting physio-prescribed exercises
- Race-day pacing strategy until the build phase is reached
- <!-- add others as they come up -->

## Current block

- Phase: rehab / movement screen — pre-programming
- This week: gathering baseline information and clinician input before the first programmed block
- Recent changes: 