# /usaf-badge [base-name] — USAF Heritage Badge Generator

Trigger: "generate badge for [base]" / "/usaf-badge [base name]"
Purpose: Generate an original AI unit badge for a USAF base using the
established pipeline — five-file YAML schema + prompt expansion + image gen.

Repo: X:\essco\usaf-heritage-graphics (navspex/usaf-heritage-graphics)
Pipeline: draft_unit_reimaginings.py + build_tee_prompt.py
Schema: five-file YAML per unit (verified, HEAD includes commits debd26a–54a6667)
Test runs: Travis AFB + Aviano — completed (images in ChatGPT history / ESSCO-POD)
Budget: $20 OpenAI API deposit funded

## Strategy
AI-generated ORIGINAL unit badges — NOT modifications of real patches.
This avoids trademark risk vs modifying actual USAF insignia.
DoD commercial licensing royalty est. 14–18% for official marks.
Our originals = inspired by unit name/mission/history, legally clean.

## Steps

1. **Identify the base** — full name and ICAO/location if known.
   Ask: "Which base?" if not specified.

2. **Look up unit history** (live web search — no training data):
   - Primary mission (fighter / bomber / airlift / training / space / cyber)
   - Iconic aircraft associated with this base
   - Notable combat history or notable unit
   - Base mascot or unofficial motto if any
   - Geographic/regional visual elements (desert / mountain / coastline)

3. **Load or create the five-file YAML schema:**
   ```
   units/[base-slug]/
     unit.yaml          — name, mission, history summary
     visual.yaml        — suggested symbols, colors, style
     prompt_seeds.yaml  — 5 raw concept seeds
     expanded.yaml      — (generated) full prompt expansions
     production.yaml    — (generated) final approved prompts
   ```
   If the unit already exists in the repo: load it. Otherwise create it.

4. **Run prompt expansion** via draft_unit_reimaginings.py:
   Takes the 5 seeds → expands to full image-gen prompts using the
   7-field framework: VISUAL FAMILY / COMPOSITION / PALETTE /
   TYPOGRAPHY / ICONOGRAPHY / COPY / FOOTER VARIANT

5. **Build the tee prompt** via build_tee_prompt.py:
   Final production prompt formatted for image generation.
   Visual treatment: flat print-ready (sign-as-artwork style for Versant 4100)
   NOT photorealistic mockup (Dale's standard for production).

6. **Generate the badge** — pick the best tool available:
   - Primary: Midjourney V7/V8.1 (best heraldic/illustrative quality)
   - Fallback: FLUX.2 via Modal endpoint (already deployed, ~$0.01/img)
   - Budget option: GPT-Image-2 (OpenAI API, $20 deposit)
   Prompt must include: "original design, NOT based on any existing patch or insignia"

7. **Quality check:**
   - Does it read as a real unit badge (not clip art)?
   - Is the text legible (if any)?
   - Does it avoid copying any real USAF insignia?
   - POD-ready? (high contrast, works on dark + light tee backgrounds)

8. **Save outputs:**
   - Image: usaf-heritage-graphics/output/[base-slug]_badge_v1.png
   - Update production.yaml with the winning prompt

9. **Report:**
```
Badge generated: [Base Name]
Mission: [one line]
Prompt used: [key themes]
Output: [file path]

Ready for tee mockup? Say GO and I'll build the Shopify product draft.
▶ Next base? Or move to POD setup?
```

## Production Run (batch — 127 bases)
Once the first 5 are approved:
- Script loops through all units/ YAMLs
- Generates 2-3 variants per base
- Dale approves keepers
- Rejected variants flagged for re-run with adjusted prompt
- Estimated cost: ~$15-40 total via OpenAI API path (~30 min wall-clock)

## Shopify POD Integration (after badge set approved)
- Platform: Printful (account not yet created — essco-2026-blitz B-B3)
- Products: tees + hats (primary), stickers (secondary)
- DoD commercial licensing: decision pending Dale
- Price point: tee ~$29-35, hat ~$24-28 (Printful base cost + margin)

## Hard Rules
- ALWAYS include "original design, NOT based on any existing patch or insignia"
  in every prompt. No exceptions.
- No real unit crests, real patch imagery, or copyrighted military insignia
  as reference images in the prompt.
- Ideogram 2.0 recommended for final production run (best text rendering
  for badge lettering) — but requires key (not yet in stack).
