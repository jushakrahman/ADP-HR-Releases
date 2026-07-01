# ADP HR Screener — Releases

Public **release-only** channel for the ADP HR Resume Screener desktop app. The
application source lives in a separate private repository; this repo exists so
that HR machines (which are not authenticated to the private source) can reach
the update manifest and download installers.

## What is tracked here

Only lightweight metadata files are committed to git:

- **`version.json`** — the update manifest the app polls on startup. It advertises
  the latest available version and the installer download URL.
- **`README.md`** — this file.
- **`.gitignore`** — ensures installer binaries are never committed to git.

## Installers are Release assets, never git files

The installer (`ADP_HR_Screener_Setup.exe`, ~160 MB) is uploaded as a
**GitHub Release asset**, not committed to the repository. `.gitignore` blocks
`*.exe` so a binary can't be added by mistake.

## `version.json` format

```json
{
  "latest_version": "1.1.2",
  "download_url": "https://github.com/jushakrahman/ADP-HR-Releases/releases/download/v1.1.2/ADP_HR_Screener_Setup.exe",
  "release_notes": "Short human-readable summary of the release.",
  "minimum_version": "1.0.0"
}
```

- `latest_version` — newest published version; the app compares this to its own.
- `download_url` — direct URL to that version's Release asset.
- `release_notes` — shown to the user in the update banner.
- `minimum_version` — installs below this are treated as a mandatory update.

## Release procedure (strict order)

1. Build the installer from the private source repo (`build.bat` → `makensis`).
2. Create GitHub Release **`vX.Y.Z`** here and upload `ADP_HR_Screener_Setup.exe`
   as its asset. Verify the asset URL returns HTTP 200.
3. **Only then** update `version.json` on `main` to the new `latest_version` +
   `download_url` and push.

Never bump `version.json` before the asset exists, and never publish the private
source binaries or source `version.json` through this channel.
