# /cta-status — CTA Video Pipeline Tracker

Trigger: "cta status" / "where are the CTAs" / "what's blocking the blast"
Purpose: Single-command status on all 4 open CTAs and the Mailchimp blast gate.

The 25K-subscriber Mailchimp blast is ESSCO's launch milestone.
CTAs are the last gate before it fires. This command keeps that chain visible.

## Known CTA State (as of 2026-06-23 — update from memory on each run)

| CTA | Script | Emma Render | B-Roll | Wired on Site | Blocker |
|---|---|---|---|---|---|
| Wide-Format | ✓ (rewritten — "wide-format gear" + slug) | STALE (pre-rewrite 50s+52s) | Not built | ✗ | Re-render Emma against new script |
| Posters | ✓ approved | ✓ 32s, olive bg | Needs catalog re-resolve (20K) | ✗ | Post-production in Camtasia |
| Lamination | DRAFT (Dale decides: "D-P-E's" or "instructors") | ✓ 52s, white bg | Needs catalog re-resolve | ✗ | Script decision + post-production |
| Binding | DRAFT (Dale decides: "nine-thousandth" phrasing) | ✗ not shot | Not built | ✗ | Script lock → shoot Emma → post-prod |

## Steps

1. Read current CTA state from memory (essco-launch.md blockers T-CTA-01 through T-CTA-08).
   If memory is stale, use the table above as baseline and flag STALE-RISK.

2. Print the dashboard:

```
CTA PIPELINE STATUS — [date]
Blast gate: [N] of 4 CTAs wired. Launch needs: 4/4.

Wide-Format:  [status emoji] [one-line status] — blocker: [what]
Posters:      [status emoji] [one-line status] — blocker: [what]
Lamination:   [status emoji] [one-line status] — blocker: [what]
Binding:      [status emoji] [one-line status] — blocker: [what]

BLAST READINESS: [X] of 4 CTAs complete.
Estimated days to blast: [N] (assuming [assumption])

DALE MUST DO NOW:
1. [most urgent Dale action — usually script decision or Emma re-render]
2. [next Dale action]

CLAUDE CAN DO NOW (on Dale GO):
1. [Claude-side task ready to execute]
```

3. Name the ONE thing Dale needs to do TODAY to advance the chain.
   Don't give him a list of 4. One thing.

4. If all 4 CTAs are wired: "Blast is ready. Dale fires it. Go."

## Status Emojis
✅ Done and wired | 🎬 Rendered, needs post-prod | 📝 Script not locked
🎤 Emma not shot | 🔧 Wired, needs QA | ❌ Not started

## Post-Production Architecture (locked 2026-05-25)
- Pipeline produces b-roll-only MP4 (slideshow + chyrons + music)
- Dale composites Emma via Camtasia AI Background Removal on Track 2
- Chyrons: left-aligned at 6% from left edge, clearing Emma's lower-right zone
- Output: H.264 High@4.0 CRF 22 AAC 192k +faststart (NEVER HEVC)

## Six Missing Stills (needed before final render)
These are shootable in ~60 seconds each on a phone in the ESSCO shop:
- Wide-Format slot 1: Phone with tiny wiring diagram on screen
- Wide-Format slot 10: Sharpie marking on laminated document (wipe-clean demo)
- Lamination slot 5: Stapled photocopy (negative example vs laminated)
- Lamination slot 13: Kneeboard with card
- Binding slot 1: Manual that won't lay flat
- Binding slot 2: Page blowing out during hot start
