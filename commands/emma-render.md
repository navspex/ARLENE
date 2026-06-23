# /emma-render [script] — HeyGen Avatar IV Shot in One Command

Trigger: "render Emma" / "shoot Emma" / "/emma-render [script or file path]"
Purpose: Fire the HeyGen av4 API end-to-end from script to MP4, no manual wiring.

Emma = ESSCO's permanent spokesperson + companion in After Closing Time.
Voice: ElevenLabs "Emma Talks" v3 (voice_id in X:\DISMOUNT\.env)
Image: hero still emma-home-06 (HeyGen asset, verified Gate 0 2026-06-22)
API key: X:\DISMOUNT\.env → HEYGEN_API_KEY
Credits remaining: ~1,674 (as of Gate 0) — ~$27.90 value
Cost: ~$3/min @720p, ~$4/min @1080p (per-second billing)

## Steps

1. **Get the script** — from argument, file path, or ask:
   "Paste the script or give me the file path."

2. **Read credentials** from X:\DISMOUNT\.env:
   - HEYGEN_API_KEY
   - ELEVENLABS_API_KEY (for voice_id lookup if needed)
   - Emma's image_key (from prior upload, or re-upload emma-home-06)

3. **Check credit balance** before rendering:
   ```
   GET https://api.heygen.com/v2/user/remaining_quota
   Authorization: Bearer {HEYGEN_API_KEY}
   ```
   If < 100 credits remaining: warn Dale before proceeding.

4. **Upload image if needed** (emma-home-06):
   ```
   POST https://upload.heygen.com/v1/asset
   Content-Type: multipart/form-data
   → returns image_key
   ```
   Cache the image_key — don't re-upload if already known.

5. **Submit render** via av4 API:
   ```
   POST https://api.heygen.com/v2/video/av4/generate
   {
     "video_title": "[descriptive title]",
     "image_key": "[emma-home-06 key]",
     "script": {
       "type": "text",
       "input": "[script text]",
       "voice_id": "[EL Emma Talks v3 voice ID]"
     },
     "dimension": {"width": 1280, "height": 720}
   }
   ```
   → returns video_id

6. **Poll for completion** (check every 15 seconds):
   ```
   GET https://api.heygen.com/v1/video_status.get?video_id={video_id}
   ```
   States: processing → completed | failed
   Typical time: 60-120 seconds per minute of script.

7. **Download the MP4** when status = completed:
   - Save to X:\DISMOUNT\asmr-fantasy\renders\ (for film shots)
   - OR X:\essco\video-toolkit\renders\ (for MARVIS CTAs)
   - Name format: [project]_[scene]_[date].mp4

8. **Report:**
```
Emma render complete.

Video ID: [id]
Duration: [N]s
Credits used: ~[N] | Remaining: ~[N]
Saved: [full path]

▶ Next: [post-production step — Camtasia composite / Gate 1 review / etc.]
```

## Hard Rules
- NEVER use HeyGen Studio web export for production Emma — MP4 only, no alpha.
  Transparency is added in Camtasia (AI Background Removal, Track 2).
- NEVER use chroma key on white-background Emma output — punches holes through
  skin/teeth/eyes. AI subject detection only.
- For film shots (After Closing Time): always 1080p (dimension 1920x1080).
- For CTA shots: 720p is acceptable for draft; 1080p for final.
- Emma's voice is ElevenLabs "Emma Talks" v3 — NOT HeyGen's built-in voices.
  Use HeyGen's native EL integration (paste EL API key into HeyGen voice settings).
- Gate 0 proof: video_id 8996efbd607a4e0f993605eae50103b7 (40s, 126 credits, emma-home-06)

## Script wrapper (heygen_av4.py already exists)
The wrapper is at X:\DISMOUNT\tools\heygen_av4.py
CLI: python heygen_av4.py --image [key] --voice [voice_id] --script "[text]"
      OR: python heygen_av4.py --image [key] --voice [voice_id] --audio-url [url]
This skill invokes that script via Desktop Commander rather than raw API calls.
