# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This is not a software project. It is a personal rehab and marathon training planning document for Ben. There are no build commands, tests, or deployments.

## Key files to read at the start of every conversation

- `CLAUDE.md` — this file; the steering document. Read fully before responding to any training or rehab question.
- `plan.md` — objectives hierarchy (long/medium/short term) and phase gates. Read this to understand where we are in the plan.
- `memory/MEMORY.md` — project memory index. Check for up-to-date context.
- `_sessions/` — one file per session, named `YYYY-MM-DD-session-N.md`. Note the underscore prefix — required by Jekyll.

## How to assist in this repo

- Always read `CLAUDE.md`, `plan.md`, and `memory/MEMORY.md` before generating session plans or answering training questions.
- **Keep documentation updated.** Whenever a decision is made or a feature is built, update `CLAUDE.md`, `memory/project.md`, and/or `plan.md` in the same commit — not as a follow-up.
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

**Exercise tables must include a `Demo` and `Done` column** as the last two columns. `Demo` links to a YouTube video; `Done` powers the checkbox system:

```markdown
| Exercise | Sets × Reps | Load | Cues | Demo | Done |
|---|---|---|---|---|---|
| Glute bridge | 2 × 10 | Bodyweight | Drive through heels. | https://www.youtube.com/watch?v=NXwR65L7I-s | [ ] |
```

- `Demo` is always second-to-last; `Done` is always last.
- `Demo` must contain a plain YouTube URL (not a markdown link) — the site converts it to a ▶ button automatically.
- If no suitable video exists for an exercise, leave `Demo` empty: `| |`
- When writing a session, use WebSearch to find physio-quality YouTube demos for each exercise. Prefer channels like Tim Keeley Physio REHAB, or similar sports rehab channels with clear form cues.
- Use `[ ]` (not done) or `[x]` (done) in the `Done` column — exact format, including spaces.
- Assessment check tables (e.g. Block 3 diastasis checks) should also have `Demo` and `Done` columns.

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
- Movement background: gymnastics coaching experience (no longer actively coaching), so generally movement-competent and comfortable with strength/mobility work. Previously participated in adult gymnastics classes — stopped due to knee injury, returning to gymnastics is a goal. Have completed triathlons and a marathon in the past, but 10 years ago.
- Current cardiovascular fitness: walk the dog for 2 miles every day.
- Current strength training: Never really lifted. Core strength generally quite floor.
- Sleep / stress baseline: Very busy work life, plus family life. Don't sleep as much as I should.
- Recently lost 2st whilst using Wegovy, but not currently taking it.

## Goals

Ranked:
1. Resolve or substantially improve long-standing knee pain
2. Manage diastasis recti appropriately through the build
3. Run a continuous 5k (no walk breaks) — waypoint goal
4. Run a continuous 10k (no walk breaks) — waypoint goal
5. Complete the London Marathon, 12 months out — Jeffing (run-walk) is acceptable
6. Strict pull-up on the rings (comp-level set, installed 2026-05) — parallel strength track
7. Lose weight as a side effect of training (not a primary driver — no calorie targets, see Out of scope)

The continuous 5k and 10k are not stepping stones to be skipped — they're earned through the run-walk progression. Don't bypass run-walk to chase continuous running.

The ring pull-up runs as a parallel strength track through all phases. Apparatus block in each session feeds it (dead hang → scapular pull → band-assisted → negatives → full). Progression is paced by shoulder/scapular control and grip endurance, not the running phase.

Gymnastics beyond the ring pull-up is not a standalone goal with a milestone. As the knee becomes more robust, other gymnastics elements (floor conditioning, basic tumbling prep, bar work, trampoline) should be woven progressively into sessions when knee stability and glute strength support it, not on a fixed timeline.

## Constraints

- Sessions per week: 2 or 3
- Time per session: 60–90 minutes
- Days/times: Monday, Thursday, Saturday — free access to the gymnastics club
- Equipment access: Fully equipped gymnastics club. All gymnastics apparatus available (floor, bars, beam, vault, trampolines, foam pit, etc.). **Comp-level set of rings installed 2026-05** — the pull-up goal lives here. No cardio machines — no treadmill, rowing machine, or similar.
- Running surfaces available: Sprung floor (indoor), roads, some trails

## Injury history and current rehab focus

**Diastasis recti**
- Severity / gap: Gap runs from navel upward, approximately 4–5 fingers long. Assessed Session 1 (2026-04-30).
- Currently being managed by: None. GP declined referral (classified as cosmetic). No professional input expected in the near term. Programming must be self-guided and conservative.
- Known aggravators: Hollow body hold (pressure at and below navel); dead bug single-arm extension (tension at top/upper end of gap). Heel slide had no effect.
- Cleared / not cleared movements: Heel slide — cleared. Dead bug single arm — manageable; visible coning during Session 1 but controllable with deliberate bracing. Hollow body hold — proceed with caution; mild doming present but controllable with deliberate bracing. No loaded core work until physio input.
- Operative signal for progression: **Coning, not just doming.** Visible coning that you can control with bracing is the current baseline — not a stop signal. Progression rule: hold volume if coning is visible-but-controllable; only progress reps/sets when a session is fully clean (no visible coning); regress if coning becomes uncontrollable.

**Knee pain (long-standing)**
- Which knee, or both: Right knee
- Working diagnosis: Weak glute chain causing knee tracking to be out.
- What aggravates it: Exercise. Half-kneeling on right knee on hard surface — patella feels pushed up from below (pressure pain, not joint pain). Knee warm after Session 1 but not sore.
- What helps: Rest. Padding under right knee resolves the kneeling discomfort.
- What's been tried: Aquatherapy (little relief)
- Session 1 result: 2/10 throughout blocks 1 and 2. Knee warm ~1hr post-session, not sore.

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

- Level of prescription: Fully spelled-out sets/reps/load with cues — confirmed working in Session 1.
- Loading style: Feel-based / RPE for now (no gym history to base %1RM on).
- Plan horizon: Session by session for now (Phase 1 rehab).
- Progression rules: <!-- placeholder: e.g. "advance when X for two sessions in a row" -->
- Missed sessions: <!-- placeholder: skip, shift, or compress -->
- Format I want plans delivered in: Markdown table with Sets × Reps, Load, Cues, Demo (YouTube URL), Done checkbox — confirmed working in Session 1.

## Red flags and back-off rules

Stop or modify the session if:
- Knee pain above 3/10, or any sharp/locking/giving-way sensation
- Diastasis coning that you cannot control with bracing, or any sharp pain along the linea alba (visible-but-controllable coning is the current baseline — see Diastasis recti section for the progression rule)
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

- Phase: rehab / movement screen — Session 1 complete
- Sessions completed: 1
- Recent changes: Session 1 (2026-04-30) completed. Baseline screen done. Knee held at 2/10 during, **dropped to 1/10 at 24h** — strong recovery signal, current loading well below tolerance. Diastasis aggravators mapped — visible coning during dead bug single-arm but controllable with bracing. RPE 5–6 ("could go a little harder"). Programming preferences confirmed. Sessions 2–5 drafts updated 2026-05-01 with nuanced coning progression gates. 