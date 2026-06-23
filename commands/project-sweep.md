# /project-sweep — Weekly Energy Audit Across All Active Projects

Trigger: "/project-sweep" / "weekly review" / "how are all my projects doing"
Purpose: Read every memory file, print one-line status + energy for each project,
flag anything stalled 14+ days, surface the completion opportunities.
Run Sunday or Monday. Takes 90 seconds.

## Steps

1. Pull memory repo:
   ```
   git -C X:\essco\essco-claude-memory pull --ff-only
   ```

2. Read every file in memory/ (except _shared.md).
   For each project extract:
   - Energy rating (from STATE section)
   - Last milestone date (from MILESTONES — most recent entry)
   - Days since last movement (today minus last milestone date)
   - One-line status
   - Top blocker (owner: Dale or Claude)

3. Print the sweep:

```
PROJECT SWEEP — [date]
[N] active projects | [N] hot | [N] warm | [N] cold | [N] dead

🔥 HOT (moving)
  marvis          — [one-line status] | last moved: [N] days ago
  campaign-KOSH   — [one-line status] | last moved: [N] days ago

🌡️ WARM (real, not top of mind)
  asmr-fantasy    — [one-line status] | last moved: [N] days ago
  dismount        — [one-line status] | last moved: [N] days ago
  essco-launch    — [one-line status] | last moved: [N] days ago

🧊 COLD (stalled — no movement in 14+ days)
  [project]       — [one-line status] | STALLED [N] days | blocker: [who]

☠️ HONEST DEAD (hasn't moved in 30+ days, blocker is still Dale)
  [project]       — [one-line status] | [N] days | Kill it or schedule it?

COMPLETION OPPORTUNITIES (80%+ done, finish line visible):
  [project] — [what's left, estimated time]

DALE MUST DECIDE THIS WEEK:
1. [item — specific decision, not a task]
2. [item]

CLAUDE CAN ADVANCE NOW (no Dale dependency):
1. [item]
2. [item]
```

4. Flag any project stalled 14+ days with owner = Dale.
   Name the specific decision or action blocking it. Don't soft-pedal.

5. Identify completion opportunities — projects 80%+ done where the finish
   line is specific and achievable this week. Name the last 20%.

6. Pick ONE project for Dale to focus on this week.
   Criteria: highest leverage + closest to done + not blocked on external parties.

## Hard Rules
- ☠️ DEAD is not an insult — it's permission. Naming it frees the mental load.
- "Hasn't moved in 30 days with a Dale-side blocker" = dead until proven otherwise.
- Never list more than 3 items in DALE MUST DECIDE. Priority, not exhaustion.
- The sweep is read-only. It does not update memory files. /MCPU does that.
- Completion opportunities get ONE sentence: what remains + time estimate.
  "3 more chapters + student workbook — estimated 4 hours" not a paragraph.

## Known Project Keys (update as projects open/close)
marvis | campaign-KOSH-2026 | asmr-fantasy | dismount | essco-launch |
ebay-lister | garmin-esp | poster-assets | eab-wb | essco-2026-blitz |
usaf-graphics | fillable-forms | workorder-numbering | repo-sync
