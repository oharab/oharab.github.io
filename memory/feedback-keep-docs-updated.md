---
name: Keep documentation updated
description: Ben expects docs and memory to be updated whenever a decision is made — not as a separate step later
type: feedback
---

Always update documentation immediately when a decision is made or a feature is built — do not defer it.

**Why:** Ben explicitly asked for this after several features were built without the docs being updated. Stale docs mean future Claude instances make inconsistent session files or miss established patterns.

**How to apply:** After any significant change, update whichever of these are relevant before committing:
- `CLAUDE.md` — if the change affects how session files should be structured (new columns, new formats, new rules)
- `memory/project.md` — if the site architecture, current phase, or key decisions have changed
- `plan.md` — if phase gates, objectives, or session status have changed

Include doc updates in the same commit as the feature, not as a follow-up.
