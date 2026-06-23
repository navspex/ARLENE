# /modal-deploy [tool] — One-Command Modal Endpoint Deploy

Trigger: "deploy [tool] to Modal" / "/modal-deploy [tool-name]"
Purpose: Deploy a video-toolkit Modal endpoint with the correct Windows
environment fixes, quality presets, and log-to-file approach — without
rediscovering the charmap gotcha every time.

Workspace: dale-41444
Toolkit: X:\essco\video-toolkit (also cloned at X:\DISMOUNT for film work)
.env: X:\essco\video-toolkit\.env (gitignored — all endpoint URLs live here)
Modal token: C:\Users\Simulator\.modal.toml (CLI auth, do NOT expose)

## Deployed Endpoints (as of 2026-06-22)
| Tool | Modal App Name | Status |
|---|---|---|
| flux2 | video-toolkit-flux2 | ✅ LIVE |
| qwen3-tts | video-toolkit-qwen3-tts-qwen3tts-generate | ✅ LIVE |
| upscale (RealESRGAN) | video-toolkit-upscale-upscaler-upscale | ✅ LIVE |
| sadtalker | video-toolkit-sadtalker-sadtalkergen-generate | ✅ LIVE |
| dewatermark | video-toolkit-dewatermark-dewatermark-dewatermark | ✅ LIVE |
| image-edit | video-toolkit-image-edit-imageeditor-edit | ✅ LIVE |
| ltx2 (video) | video-toolkit-ltx2-ltx2-generate | ✅ LIVE |
| music-gen | — | ⏭️ SKIPPED (free acemusic cloud API used instead) |

## Steps

1. **Identify the tool** to deploy. Check the table above — if already LIVE, skip deploy.
   Ask: "This endpoint is already live. Re-deploy to update, or use the existing one?"

2. **Set Windows UTF-8 environment** (REQUIRED — prevents charmap crash):
   Run in PowerShell BEFORE modal deploy:
   ```powershell
   $env:PYTHONIOENCODING = "utf-8"
   $env:PYTHONUTF8 = 1
   chcp 65001
   [Console]::OutputEncoding = [System.Text.Encoding]::UTF8
   ```

3. **Deploy with log redirect** (REQUIRED — prevents MCP timeout on long builds):
   ```powershell
   cd X:\essco\video-toolkit
   modal deploy docker/modal-[toolname]/app.py *> deploy_[toolname].log
   ```
   Long builds (ltx2 = ~11 min, 55GB) continue server-side even if terminal appears idle.

4. **Poll the log** for completion:
   ```powershell
   Get-Content deploy_[toolname].log -Wait | Select-String "endpoint|Error|✓"
   ```
   Look for: endpoint URL line → copy to .env

5. **Write endpoint URL to .env:**
   ```
   MODAL_[TOOLNAME]_ENDPOINT_URL=https://dale-41444--video-toolkit-[appname].modal.run
   ```
   File: X:\essco\video-toolkit\.env (gitignored — secrets stay off git)

6. **Smoke test** the new endpoint:
   ```python
   # Quick test — replace with tool-specific call
   import requests, json
   response = requests.post(ENDPOINT_URL, json={"prompt": "test", "seed": 42})
   print(response.status_code, len(response.content))
   ```
   Verify: non-empty response, no error JSON.

7. **Report:**
```
Deployed: [tool]
Modal app: [app-name]
Endpoint: [URL] (written to .env)
Smoke test: [pass/fail + detail]
Cost estimate: ~$[N]/run

▶ Next: [what this enables — e.g. "LTX-2 video ready. Run at max-quality config."]
```

## Max-Quality Config (Dale-directed — always use this for production)
- GPU: H100 SXM (80GB) — NOT A100 or L40S
- LTX-2: ~40 steps, 1024x576 minimum, RealESRGAN upscale to 4K
- FLUX.2: full precision (not quantized)
- Upscale: RealESRGAN after every video render
- Cost: Modal H100 ~$4.29/hr, per-second billing, $30/mo free credit = cents/clip

## HF Token (for ltx2 and future large model deploys)
- Required for: ltx2 (Gemma 3 12B + LTX-2.3 22B weights via HF)
- Modal secret name: huggingface-token (key: HF_TOKEN)
- Dale created this 2026-06-22 — already wired into ltx2 deploy
- For new tools needing HF: verify secret exists: `modal secret list`

## Known Gotchas
1. **charmap crash** — PowerShell cp1252 chokes on Modal's Unicode progress bars.
   Fix: always set PYTHONIOENCODING + chcp 65001 BEFORE running modal deploy.
2. **Modal builder transient error** — occasional "external shut-down" during image build.
   Fix: retry `modal deploy ...` — it resumes from cache.
3. **Long deploy appears frozen** — ltx2 takes ~11 min, flux2 ~4 min.
   Fix: log-to-file approach. The deploy runs server-side; don't kill it.
4. **Web endpoints are public** — no Modal token needed in .env for calling endpoints.
   The .modal.toml CLI auth is separate from the HTTP endpoint auth.
5. **R2 boto3 warning** — "boto3 not installed, skipping R2" = direct-download fallback.
   Optional: `pip install boto3` in the toolkit env to route large transfers via R2.
