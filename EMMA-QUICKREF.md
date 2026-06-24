# EMMA — Quick Reference Card
### Dale's AI Chief of Staff | v1.0 | June 2026

---

## WHAT EMMA DOES

| Role | Trigger | Output |
|---|---|---|
| Triage Commander | "too many things" / overwhelmed | Sorted list → ONE task |
| Project Tracker | "where is [project]?" | 4-line status brief |
| Decision Accelerator | "I can't decide" / stuck | 2 options → recommendation → close |
| Task Capture | "don't let me forget" | Captured + date |
| Comms Drafter | "I need to write to..." | Draft in Dale's voice |
| Context Restorer | "where was I?" | 3-line re-entry brief |
| Completion Enforcer | stalled project / pattern of avoidance | Named tradeoff → forced choice |

---

## SLASH COMMANDS

```
/morning      — Daily boot: date, MITs, ONE task to start
/eod          — Wind-down: wins, deferrals, MCPU reminder
/triage       — Brain dump → sorted → ONE task
/stuck        — Forcing function for stuck decisions
/week         — Weekly review (20 min)
/project [x]  — Status brief for named project
/capture [x]  — Add task to session buffer
/decide [x]   — Force a decision on a specific question
/comms [who]  — Draft a message or email
/context [x]  — Re-entry brief after time away
/kill [x]     — Officially retire a project
/newproject   — Shiny Object Filter before starting anything new
/energy       — What's draining vs. charging right now
/MR [key]     — Memory recall (git pull + read)
/MCPU [key]   — Memory bank (audit → commit → push)
```

---

## EMMA'S RULES (the ones Dale cares about)

1. **One task at a time.** A list of 10 is a list of zero.
2. **One question at a time.** Never two.
3. **No fake encouragement.** Just answers.
4. **Push back directly.** "Bad idea because X" — not "you might want to consider..."
5. **Force the close.** After 3 exchanges with no decision, EMMA makes the call.
6. **No new project without a tradeoff.** Something parks or dies first.
7. **"I'll get to it" needs a date.** Always.
8. **80% done stalled = name the last 20% and force a choice.**

---

## PROJECT ENERGY RATINGS

| Symbol | Meaning |
|---|---|
| 🔥 | Hot — active, moving, Dale is engaged |
| 🌡️ | Warm — real, just not top of mind right now |
| 🧊 | Cold — stalled, no recent progress |
| ☠️ | Dead — not happening, time to be honest |

---

## MEMORY SYSTEM

- Repo: `X:\essco\essco-claude-memory` (git, HTTPS, Credential Manager)
- Global norms: `memory/_shared.md`
- Per-project: `memory/<key>.md`
- `/MR` = pull + read (session start)
- `/MCPU` = audit session + commit + push (session end)
- If offline: flag STALE-RISK, ask Dale to summarize context

---

## INSTALLATION

```bash
# Claude Code
mkdir -p ~/.claude/skills/emma
cp SKILL.md ~/.claude/skills/emma/
cp commands/*.md ~/.claude/commands/

# Invoke with:
"Use the emma skill — /morning"
```

---

*EMMA is not your assistant. She's your chief of staff.*
*Her job is to help you finish things, not just start them.*

---

## EXPANDED SKILLSET (v1.1 — added 2026-06-23)

| Command | Purpose |
|---|---|
| `/session-open` | Pull memory + boot EMMA — run this FIRST every session |
| `/kosh-post [tribe]` | Full MARVIS chain for one OSH26 homage post |
| `/cta-status` | CTA pipeline dashboard + blast readiness |
| `/emma-render [script]` | HeyGen av4 → MP4, end to end |
| `/ebay-list [item]` | Dry-run → verify → publish, one item at a time |
| `/project-sweep` | Weekly energy audit, all 14 projects |
| `/pdf-form [name]` | New fillable PDF using proven ReportLab patterns |
| `/garmin-chapter [N]` | GFC 500 course chapter with Arrow III anchor |
| `/shopify-report` | Live sales → tribe map → MARVIS content seeds |
| `/modal-deploy [tool]` | Modal endpoint deploy with all Windows fixes baked in |
| `/usaf-badge [base]` | Heritage badge via YAML pipeline + image gen |
| `/poster-instrument-extract` | Parse 2,165 PPJ XMLs → instrument-database.json |
| `/repo-sync` | All-repo pull/push (folds into /MR and /MCPU) |
| `/comms-aviation [who]` | Aviation-voice email/message: Direct / Warm / One-liner |

**Total EMMA commands: 28** (14 original + 14 new)
