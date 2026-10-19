# MDPro4 Workshop

Public delivery endpoint for [MDPro4](https://github.com/ElderLich/MDPro4).

## Release channels

- **Stable, legacy readers:** `update-manifest.json`.
- **Stable, upgraded readers:** `channels/v2/stable/update-manifest.json`.
  Keep these endpoints separate; the legacy manifest must remain compatible
  with the older installed players.
- **Test:** `channels/test/update-manifest.json` contains historical tiny
  synthetic fixtures. Their releases are no longer hosted; these are offline
  examples, not playable game updates.

Use each manifest's per-platform versions and routes to identify the available
release. Windows and Android retain historical update routes from 1.0.0 and
1.0.1. Linux and macOS start automatic updates at the fresh full 1.0.2 baseline.
MDPro3 releases have been retired; MDPro4 is a fresh installation, including a
new Android application identity.

Patch targets are curated to the newest four releases (currently 1.0.4,
1.0.3.2, 1.0.3.1 and 1.0.3), with older bridge routes retained when needed.
Every supported installed version still has a direct route to 1.0.4; this
includes Windows/Android 1.0.0 installations. A cumulative patch requires
its exact installed baseline and does not replace a full offline installer.
Superseded cumulative variants to older targets are retired after their catalog
references are removed. Every supported baseline keeps its latest cumulative
patch, alongside the retained recent patch targets and updater recovery.
Offline installers, local compiled players and historical indexes remain separate.

## Build and publish

Use **Release Studio** to compose a complete release snapshot and package an
offline installation. Windows uses a setup EXE with adjacent parts; Android uses
successive APK install parts; Linux and macOS use a graphical setup wizard and
adjacent payload parts. Keep every package's generated files together.

Use **Patch Studio** for later updates. Review added, modified and removed files
before building. Retain shipped baseline indexes so users can update after
skipping releases. Both applications live under `Tools/Release` in the main
project. Full installers are distributed separately through the maintainer's
external mirrors.

Patch payloads use `https://updates.elderlich.com/patches/<platform>/<filename>`.
GitHub hosts Workshop discovery metadata and the Android application update APK.
Upload and verify the selected payloads, review the prepared channel, then
publish that channel. Platforms can be published incrementally; existing routes
for other platforms remain available. Published payload bytes are immutable.

## Compatible Windows updates

Windows players through 1.0.3.2 need whole ZIP downloads, including on the v2
channel. Their IL2CPP disk-space probe cannot prepare segmented archives.
Windows 1.0.4 contains the corrected native disk-space check. The first update
from a legacy reader must also remain a compatible complete archive.

If Windows 1.0.3 or 1.0.3.1 reports `FileVersionInfo`, close the game, extract the
[one-time updater recovery](https://updates.elderlich.com/patches/windows/MDPro4-windows-1.0.3-updater-recovery.zip)
beside `MDPro4.exe`, run `Repair-Windows-1.0.3-Updater.cmd`, then reopen and update.
If recovery is already configured, fully restart the game and retry the update.
The recovery package and historical routes remain available for those clients.
