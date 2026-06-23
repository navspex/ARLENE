# /repo-sync — All-Repo Pull/Push at Session Open/Close

Trigger: Folds into /MR (open) and /MCPU (close) — not a standalone command.
Also callable directly as /repo-sync for a manual sync outside those commands.
Purpose: Keep all navspex repos in sync at every session boundary.
Eliminates the stale local/remote drift that causes collision and confusion.

STATUS: Design agreed 2026-06-18. Three Dale decisions pending before build.
Once decisions land, this script replaces the manual per-repo approach.

## Dale's Three Open Decisions (answer these first)

1. **Uncommitted changes at close:**
   - REPORT-ONLY (recommended) — list them, don't auto-commit
   - AUTO-COMMIT with generic message "wip: session close [date]"
   Decision: ___________

2. **claude-archive (Git-LFS, ~218MB):**
   - SKIP LFS blobs in per-session loop (recommended) — mirror refresh covers backup
   - FULL LFS every open/close (slow, ~2-5 min per session)
   Decision: ___________

3. **workorder-numbering (no remote yet):**
   - CREATE GitHub remote now as navspex/workorder-numbering --private
   - LEAVE local-only for now
   Decision: ___________

## Repo Inventory (all private, navspex org)
| Repo | Local Path | Has Remote | LFS |
|---|---|---|---|
| essco-claude-memory | X:\essco\essco-claude-memory | ✓ | No |
| marvis | X:\essco\marvis | ✓ | No |
| print_essco_aircraft_com | X:\essco\print_essco_aircraft_com | ✓ | No |
| ebay-lister-with-ClaudeCode | X:\essco\ebay-lister-with-ClaudeCode | ✓ | No |
| essco_poster_assets | X:\essco\essco_poster_assets | ✓ | No |
| usaf-heritage-graphics | X:\essco\usaf-heritage-graphics | ✓ | No |
| DISMOUNT | X:\DISMOUNT | ✓ | No |
| ARLENE | X:\essco\ARLENE | ✓ | No |
| claude-archive | X:\essco\claude-archive | ✓ | YES — LFS |
| workorder-numbering | X:\essco\workorder-numbering | ✗ pending | No |

## Script (sync-all-repos.ps1 — build after Dale answers 3 decisions)

```powershell
# sync-all-repos.ps1 — run at session open (pull) and close (push)
# Usage: .\sync-all-repos.ps1 -Mode [open|close]

param([string]$Mode = "open")

$repos = @(
    "X:\essco\essco-claude-memory",
    "X:\essco\marvis",
    "X:\essco\print_essco_aircraft_com",
    "X:\essco\ebay-lister-with-ClaudeCode",
    "X:\essco\essco_poster_assets",
    "X:\essco\usaf-heritage-graphics",
    "X:\DISMOUNT",
    "X:\essco\ARLENE",
    "X:\essco\workorder-numbering"
    # claude-archive: add with LFS-skip flag per Dale decision
)

foreach ($repo in $repos) {
    if (-not (Test-Path "$repo\.git")) {
        Write-Host "SKIP $repo — not a git repo"
        continue
    }

    if ($Mode -eq "open") {
        $result = git -C $repo pull --ff-only 2>&1
        if ($LASTEXITCODE -ne 0) {
            Write-Host "STALE-RISK $repo — $result"
        } else {
            Write-Host "✓ $repo"
        }
    }

    if ($Mode -eq "close") {
        # Report uncommitted changes (REPORT-ONLY per Dale decision)
        $status = git -C $repo status --short
        if ($status) {
            Write-Host "UNCOMMITTED $repo :`n$status"
        }
        # Push any unpushed commits
        $unpushed = git -C $repo log @{u}..HEAD --oneline 2>&1
        if ($unpushed) {
            git -C $repo push
            Write-Host "PUSHED $repo"
        }
    }
}

# Refresh bare mirrors
Write-Host "`nRefreshing backup mirrors..."
Get-ChildItem "X:\backup\navspex\*.git" | ForEach-Object {
    git --git-dir=$_.FullName remote update --prune 2>&1 | Out-Null
    Write-Host "✓ mirror: $($_.Name)"
}
```

## Integration with /MR and /MCPU
Once built, the /MR command adds ONE line before reading memory:
```
powershell X:\essco\sync-all-repos.ps1 -Mode open
```

And /MCPU adds ONE line after committing memory:
```
powershell X:\essco\sync-all-repos.ps1 -Mode close
```

## Divergence Handling (never abort)
- Fast-forward only (--ff-only) prevents surprise merges
- On diverge: flag STALE-RISK [repo] and CONTINUE
- Never block session open on a sync failure
- Diverged repos get flagged for Dale to resolve manually
