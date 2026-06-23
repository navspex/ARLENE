# /pdf-form [form-name] — Fillable PDF on Demand

Trigger: "build a form" / "I need a PDF form for" / "/pdf-form [name]"
Purpose: Generate new fillable PDF forms using the proven ReportLab pattern
established across 8 delivered ESSCO/Aero-Pro forms.

## Established Patterns (never rebuild from scratch — use these)

### ReportLab Rules (hard-won, never violate)
- All TextFields: `fillColor=white` explicitly — default is blue, always wrong
- Field borders: 0.5pt, lightgrey — visible but not distracting
- Checkboxes: clickable squares, visible borders
- Signature stamps: PNG with transparency — black bg needs brightness<40 → alpha=0
- Blue-field AP stream defect fix: replace '1 g' with '1 1 1 rg' in /AP /N stream
  (pypdf stream edit — applies when fields look blue in Acrobat Pro 11)

### Known Signature Files
- MWB = mwb.jpg (Michael W. Berger) — Training Manager / Chief Inspector
- DWB = dwb-1.png (Dale W. Berger) — used on altimeter cards

### Technician Roster (Aero-Pro Avionics)
Michael Berger, Dale Berger, Daniel Berger, Richard Pierce, Ryan Parker
Aero-Pro CRS#: OQGR779X

### ESSCO Equipment References
- KIP 860: 11"–36" roll width, 600×2400 dpi, 4-color CMYK LED toner
- Laminator fleet: LEDCO Premier III (25"), GBC Pinnacle 27 EZload (27"), Tahsin SM-330
- Versant 4100: production press
- PrimeLink C9070: color production press (NOT B9070)

## Steps

1. **Identify the form** — name and purpose.
   Ask: "What's the form for, and what fields does it need?"
   If Dale has a paper original: "Upload it or describe the layout."

2. **Map to existing pattern** — check if it's similar to a built form:
   - Transponder test → ESSCO_Transponder_Test_Form.pdf pattern
   - Training record → AeroPro_Training_Record_Form.pdf pattern
   - Altimeter card → altimeter_correction_card.pdf pattern
   - Work order numbering → workorder-numbering skill (separate)

3. **Confirm field list** — name, type (text/checkbox/signature/date), required/optional.
   Show Dale a quick spec table before building:
   ```
   Field | Type | Required | Notes
   [name] | TextField | Yes | [note]
   ```

4. **Build the form** using ReportLab:
   - White field backgrounds (ALWAYS)
   - Visible thin-gray borders on text fields
   - Clickable checkbox squares
   - Static PNG signature stamps where applicable
   - ESSCO/Aero-Pro branding in header

5. **Verify** with pypdf + PyMuPDF:
   - All fields white (no blue)
   - Field count matches spec
   - Signature stamps transparent (not black bg)
   - File opens cleanly in Acrobat

6. **Deliver:**
   - Save to /mnt/user-data/outputs/[FormName].pdf
   - If batch (like the 25 training records): deliver as ZIP
   - Report field count + file size

## Output format
```
Form built: [FormName].pdf
Fields: [N] total ([N] text / [N] checkbox / [N] signature)
Size: [N] KB
White-field verified: ✓
Signature stamps: [MWB/DWB/none]

Download above. Open in Acrobat to verify before printing.
▶ Need a batch run (pre-filled copies)? Say how many and I'll generate them.
```

## Quick Reference — Common Field Types
```python
# Text field (white bg — ALWAYS)
form.textfield(name='field_name', tooltip='Label', x=x, y=y,
               width=w, height=h, fillColor=white,
               borderColor=lightgrey, borderWidth=0.5)

# Checkbox
form.checkbox(name='check_name', tooltip='Label', x=x, y=y,
              size=12, fillColor=white, borderColor=lightgrey)

# Signature line (static PNG stamp)
canvas.drawImage('mwb.jpg', x, y, width=w, height=h, mask='auto')
```
