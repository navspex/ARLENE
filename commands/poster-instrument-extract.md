# /poster-instrument-extract — PPJ XML → Instrument Database

Trigger: "extract instrument database" / "parse the PPJ XMLs" / "/poster-instrument-extract"
Purpose: Parse all 2,165 PPJ instrument XMLs into a clean JSON database.
This is the foundation for the Poster Builder Phase 2 rule-based layout engine.

Repo: X:\essco\essco_poster_assets (navspex/essco_poster_assets)
App: localhost:8501 (streamlit run app.py from X:\essco\essco_poster_assets)
Output: X:\essco\essco_poster_assets\docs\instrument-database.json

## Known Facts About the XML Structure
- 2,165 records total
- Two dimension sets per instrument:
  - Template = panel cutout dimensions (in POINTS — divide by 72 for inches)
  - WorkView = physical face dimensions (also in POINTS)
- Standard avionics stack width: 6.25 inches = 450 points
- Shape types: circular | rectangular
- Local images in Instruments\ folder — OneMileUp server is DEAD, local copies only
- Some XMLs have encoding issues (not UTF-8) — parser needs error='replace' or similar

## Output Schema (one record per instrument)
```json
{
  "uid": "string",
  "name": "string",
  "manufacturer": "string",
  "type": "string",
  "cutout_w_in": 0.00,
  "cutout_h_in": 0.00,
  "face_w_in": 0.00,
  "face_h_in": 0.00,
  "depth_in": 0.00,
  "image_path": "string",
  "shape": "circular|rectangular"
}
```

## Steps

1. **Confirm repo location** — verify X:\essco\essco_poster_assets exists and is current.
   Run: `git -C X:\essco\essco_poster_assets status`

2. **Write the extraction script** (parse_instruments.py):
   ```python
   import os, json, xml.etree.ElementTree as ET
   from pathlib import Path

   PPJ_DIR = Path(r"X:\essco\essco_poster_assets")  # adjust to actual XML location
   OUTPUT = PPJ_DIR / "docs" / "instrument-database.json"
   PT_PER_INCH = 72

   instruments = []
   errors = []

   for xml_file in PPJ_DIR.rglob("*.xml"):
       try:
           tree = ET.parse(xml_file, parser=ET.XMLParser(encoding='utf-8',
                           errors='replace'))
           root = tree.getroot()
           # Extract fields — adjust element paths to actual PPJ schema
           record = {
               "uid": root.get("uid") or xml_file.stem,
               "name": root.findtext("Name", ""),
               "manufacturer": root.findtext("Manufacturer", ""),
               "type": root.findtext("Type", ""),
               "cutout_w_in": ...,  # Template width in points ÷ 72
               "cutout_h_in": ...,
               "face_w_in": ...,
               "face_h_in": ...,
               "depth_in": ...,
               "image_path": root.findtext("ImagePath", ""),
               "shape": "circular" if ... else "rectangular"
           }
           instruments.append(record)
       except Exception as e:
           errors.append({"file": str(xml_file), "error": str(e)})

   OUTPUT.parent.mkdir(exist_ok=True)
   with open(OUTPUT, "w") as f:
       json.dump({"instruments": instruments, "errors": errors,
                  "count": len(instruments)}, f, indent=2)

   print(f"Extracted {len(instruments)} instruments, {len(errors)} errors")
   ```

3. **Inspect a sample XML first** to confirm the actual element paths.
   Run: `cat [first XML file]` and read the structure before writing the parser.
   The schema above is from memory — verify before assuming field names.

4. **Handle encoding errors gracefully:**
   - Use `errors='replace'` in XMLParser
   - Log malformed files to errors[] array
   - Don't let one bad file abort the whole run

5. **Run the extraction:**
   ```
   python parse_instruments.py
   ```
   Expected: ~2,165 records, some encoding errors (normal)

6. **Validate output:**
   - Total count ≥ 2,000 (flag if much lower)
   - Sample 10 random records — do dimensions look reasonable?
   - 3.125 inches = half of standard 6.25" avionics stack (sanity check)
   - Image paths — do they point to existing files in Instruments\?

7. **Commit to repo:**
   ```
   git -C X:\essco\essco_poster_assets add docs/instrument-database.json
   git -C X:\essco\essco_poster_assets commit -m "feat: PPJ instrument database JSON, [N] records"
   git -C X:\essco\essco_poster_assets push
   ```

8. **Report:**
```
Instrument database extracted.
Records: [N] / 2,165 expected
Encoding errors: [N] files (logged in errors array)
Output: X:\essco\essco_poster_assets\docs\instrument-database.json
Committed: [SHA]

Phase 2 (rule-based layout engine) is now unblocked.
▶ Next: build the layout engine? Or review the database first?
```

## What This Unlocks
- Phase 2: Rule-based layout engine (standard six-pack, avionics stack, engine instruments by zone)
- Phase 3: Agentic photo → XML match → layout → poster
- Customer-facing Poster Builder on print.esscoaircraft.com

## Hard Rule
PPJ dimensions are in POINTS (72pt = 1 inch). Always divide by 72 before storing.
Never store raw point values in the database — inches are the usable unit.
