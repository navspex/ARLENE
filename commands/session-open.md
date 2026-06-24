# /session-open — Automated Memory Pull + EMMA Boot

Trigger: Start of ANY working session. Run this FIRST, before anything else.
This command exists because skipping /MR at session open is the #1 documented
failure mode across all ESSCO sessions — logged 4 times with proven consequences.

## Steps (run in order, no skipping)

1. Pull memory repo:
   ```
   git -C X:\essco\essco-claude-memory pull --ff-only
   ```
   If offline or pull fails: flag STALE-RISK and continue. Never abort.

2. Read `memory/_shared.md` — global norms, DALE MODE, communication rules.

3. Determine active project key:
   - Check if Dale stated a project at session open
   - Check "PROJECT KEY:" line in any active Project Instructions
   - If ambiguous: ask ONE question — "Which project are we on today?"

4. Read `memory/<key>.md` — print triage only:
   - STATE (one line)
   - CLAUDE-ACTIONABLE NOW (numbered, no Dale dependency)
   - BLOCKED ON DALE (numbered)

5. Run EMMA morning boot:
   - Today's date and day
   - Top 3 MITs from the project state
   - ONE task to start right now
   - ONE thing Dale has permission to drop today

6. Set the active task tracker:
   `▶ Active: [task]`

## Output format (total: under 15 lines)
```
[EMMA: Ready] — Tuesday June 23, 2026

Memory: pulled ✓ | Project: [key]

STATE: [one line]

CLAUDE-ACTIONABLE NOW:
1. [task]
2. [task]

BLOCKED ON DALE:
1. [item]

Today's MIT: [one task]
Drop today: [one thing]

▶ Active: [task]
```

## If no project key is determinable
Print global STATE only (from _shared.md), ask ONE question:
"Which project are we opening today?"

## Hard rule
This command NEVER skips the git pull. Even if Dale says "just get started."
The pull takes 3 seconds. A missed /MR cost hours in 4 documented sessions.
