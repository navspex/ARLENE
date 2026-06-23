# /garmin-chapter [chapter] [aircraft] — GFC 500 Course Chapter Generator

Trigger: "write garmin chapter [N]" / "/garmin-chapter [N]"
Purpose: Generate or re-anchor a GFC 500 ESP course chapter using the
established structure and verified Arrow III anchor — without rebuilding
context each session.

Course: GFC 500 + ESP Pilot Familiarization and Safety Training
Primary accident: NTSB CEN21LA121 (C182Q, two fatalities — breaker pulled to defeat ESP)
Anchor aircraft: Piper Arrow III PA-28R-201 (ONLY X-Plane 12 sim with confirmed ESP modeling)
Simulator: vFlyteAir Piper Arrow III G5 (X-Plane 12, $33.95 direct)
Course files live in the claude.ai "GFC 500 Garmin" project

## Chapter Status (as of 2026-06-23)
| Chapter | Title | Status | Notes |
|---|---|---|---|
| 1 | Introduction / Accident Context | ✓ Written | Arrow III anchor — verify |
| 2 | Controls & Preflight | ✓ Written | Arrow III |
| 3 | Normal Operations | ✓ Written | Arrow III — verify |
| 4 | ESP — How It Works | ⚠️ STALLED | C182 numbers still present — needs re-anchor |
| 5 | ESP in the Traffic Pattern | ⚠️ STALLED | C182 numbers still present — needs re-anchor |
| 6 | Abnormal & Emergency | ✓ Written | Rewritten from AFMS 190-02291-29 Rev 5 §3/§3A |
| 7 | [topic TBD] | ✓ Written | Verify anchor |
| Workbook | Student Workbook | ✗ Not built | Gated on Ch 4-5 fix |
| Guide | Instructor Lesson Guide | ✗ Not built | Gated on Ch 4-5 fix |
| Scripts | Audio + Video Scripts | ✗ Not built | |
| Scenarios | X-Plane 12 scenarios | ✗ Not built | Gated on source docs |

## Open Blocker Before Writing Ch 4-5
G-B2: ESP activation window conflict — 2,000 ft vs 1,500 ft AGL.
Resolution requires Dale's Arrow III AFMS set (190-02291-29, Gain Addendum, ICA, TCDS).
Without this, the ESP envelope numbers in Ch 4 remain UNCERTAIN.
If Dale hasn't provided the docs: ask for them before proceeding with Ch 4.

## Steps

1. **Identify chapter** and whether it's new or a re-anchor.
   If re-anchor (Ch 4 or 5): flag that C182 numbers must be removed.

2. **Load course context:**
   - Anchor aircraft: Piper Arrow III PA-28R-201
   - ESP behavior: activates during unusual attitudes, provides control inputs
   - Traffic pattern risk: ESP misdiagnosis at pattern altitude = fatal
   - NTSB CEN21LA121: the why this course exists
   - GFC 500 components: autopilot servos, AHRS, GPS, flight director

3. **Check source docs available:**
   - AFMS 190-02291-29 Rev 5 (sections 3, 3A) — Ch 6 source, verified
   - Dale's AFMS set for Arrow III — needed for Ch 4-5
   - If doc not in context: ask Dale to upload or provide key spec numbers

4. **Write the chapter** using established structure:
   - Learning objectives (3-5 bullet points)
   - Background / why this matters (tie to accident case study)
   - Technical content (system description → pilot interaction → limits)
   - Common errors (from accident database + real-world pattern)
   - Key points summary (5-7 bullets, student-facing)
   - Review questions (5 questions, mix of recall and application)

5. **Verify all numbers against source docs.** Flag any UNCERTAIN values.
   Never use C182 numbers in Arrow III chapters.

6. **Deliver chapter** as markdown, ready for Pandoc/LaTeX → PDF formatting.

## Arrow III Key Specs (verified from AFMS 190-02291-29)
- ESP activation: UNCERTAIN (2,000 ft vs 1,500 ft AGL — G-B2 unresolved)
- Approach speed: per POH (varies with weight)
- Pattern altitude: typically 800-1,000 ft AGL at most fields
- Retractable gear: PA-28R-201 — gear warning horn is a factor in pattern workload

## Hard Rules
- NEVER use C182 numbers in Arrow III chapters. If a number is C182-derived
  and no Arrow III equivalent is confirmed: flag as [VERIFY: Arrow III value needed]
- NTSB CEN21LA121 is the anchor accident — reference it in every chapter where
  relevant. Don't let it disappear into background.
- Verify all regulatory citations (14 CFR, AC numbers) against current FAA sources.
- X-Plane 12 scenarios are gated on source docs (G-B4) — don't promise them
  without the Arrow III AFMS set confirmed in context.
