# /kosh-post [tribe] — KOSH Campaign Draft on Demand

Trigger: Dale says "/kosh-post [tribe]" or "draft a KOSH post for [tribe]"
Purpose: Fire the full MARVIS content chain for one OSH26 homage post,
for a specific flying tribe, in one command.

OSH26: July 20-26 2026, Wittman Regional (KOSH), #OSH26
Campaign thesis: Homage to every flying tribe. ESSCO since 1955, here to 2055.
Engine repo: X:\essco\marvis | Preview output: X:\queue\awaiting-approval

## Valid Tribes
warbird | vintage | ga | homebuilt | experimental | business | turbine |
rotorcraft | lsa | ultralight | seaplane | drone | military

## Steps

1. **Confirm tribe** — if not specified, ask: "Which tribe? (warbird / GA / homebuilt /
   rotorcraft / LSA / other)"

2. **Check the ledger** — read X:\essco\marvis\queue\ledger.md for recent posts.
   Don't repeat a tribe that shipped in the last 7 days.

3. **Run /trend-scout** for the tribe:
   - Specific aircraft or era most iconic for this tribe
   - One story hook (sound / smell / feel / moment — NOT a product pitch)
   - Verified fact about this aircraft/era (check live web source)

4. **Draft the post** per BRAND DOCTRINE:
   - HOMAGE BODY: celebrate the aircraft/era/engine note. No selling. No ESSCO.
     Make it feel like the operator wrote it about their own airplane.
   - SOFT HERITAGE CLOSE: "ESSCO Aircraft — here since 1955, here to 2055.
     Because of aviators like you. [check us out if you need us]"
   - #OSH26 + 1-2 relevant hashtags
   - Platforms: FB (longer, story) + IG (tighter, visual) + X (punchy, 1-2 lines)

5. **Run /editor 6-axis gate** (PASS = 10-12/12):
   - Hook strength (0-2)
   - Specificity (0-2)
   - Voice/ESSCO alignment (0-2)
   - Phase appropriateness (0-2)
   - Payoff (0-2)
   - RESONANCE/FEELS (0-2) — FLOOR: resonance-0 cannot PASS. Forced rewrite.

   If REWRITE: rewrite and re-gate. If KILL: tell Dale why and ask for a different hook.

6. **Attach media** via /stock-source:
   - Search Openverse for commercial-OK licensed still (cc0 / pdm / by / by-sa)
   - Download + save to the preview package
   - Write provenance.json
   - TRUST FLOOR: real licensed photos for specific named aircraft ONLY.
     LTX-2 / AI gen = abstract mood only, never a named airframe.

7. **Assemble preview** via /post-assembler:
   - Build X:\queue\awaiting-approval\[FB/IG/X]-[date]-osh26-[tribe]\preview.html
   - Embed media inline (src="media/...")
   - Write ledger entry (LED-xxxx)
   - Stamp UTM: utm_content=LED-xxxx

8. **Report to Dale:**
```
KOSH [TRIBE] post drafted.

/editor: [score]/12 — [PASS/REWRITE count]
Media: [source, license, size]
Preview: X:\queue\awaiting-approval\[folder]\preview.html
Ledger: LED-[xxxx]

Open the preview and give the verdict.
▶ Active: Review KOSH [tribe] preview
```

## Hard Rules
- POSTING GATE: nothing publishes. MARVIS creates Postiz DRAFT only.
  Dale publishes from Postiz UI.
- Never fabricate aircraft facts. Verify one live source per claim.
- Visual doctrine v2.1: hook-first, before→after, no bland default PDF covers.
- Resonance floor: if it doesn't make an aircraft owner feel something, rewrite it.
- OSH26 deadline: July 20. Each week = one tribe category. Time is real.
