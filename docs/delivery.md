# Update delivery

Players should use the [in-game updater and installation guidance](../README.md).
This page records the publishing contract for maintainers and tool authors.

## Catalogs

| Channel | Path | Readers |
| --- | --- | --- |
| Stable legacy | [`update-manifest.json`](../update-manifest.json) | Older installed players; whole archives only |
| Stable v2 | [`channels/v2/stable/update-manifest.json`](../channels/v2/stable/update-manifest.json) | Upgraded players; remains separate from legacy |
| Historical test fixtures | [`channels/test/update-manifest.json`](../channels/test/update-manifest.json) | Offline synthetic examples; their former assets are not hosted |

GitHub Releases keeps the current Android application update, **1.0.4**. It is
not a complete first installation. Full installer sets are distributed through
the maintainer's mirrors. Patch archives and retained recovery files are served
from `https://updates.elderlich.com/patches/`.

## Retained patch layout

Keep **one cumulative baseline-to-latest patch per platform**, then the adjacent
steps starting at **1.0.3**. Remove
superseded baseline-to-older-target variants; do not keep every historical
starting version's cumulative patch to latest.

| Platform | Cumulative baseline patch | Retained adjacent steps |
| --- | --- | --- |
| Windows | 1.0.0 → 1.0.4 | 1.0.3 → 1.0.3.1 → 1.0.3.2 → 1.0.4 |
| Android | 1.0.0 → 1.0.4 | 1.0.3 → 1.0.3.1 → 1.0.4 |
| Linux | 1.0.2 → 1.0.4 | 1.0.3 → 1.0.3.1 → 1.0.4 |
| macOS | 1.0.2 → 1.0.4 | 1.0.3 → 1.0.3.1 → 1.0.4 |

Windows also retains direct **1.0.3 → 1.0.4** and **1.0.3.1 → 1.0.4** as explicit
compatibility exceptions. Combining those clients' entire adjacent chain would
restore a helper their old launcher cannot start. Keep the
[one-time Windows updater recovery](https://updates.elderlich.com/patches/windows/MDPro4-windows-1.0.3-updater-recovery.zip)
available and direct affected players to the [recovery instructions](../README.md#need-a-recovery).

Android, Linux and macOS apply one offered step per installation/restart.
`platformUpdateRouting` marks those platforms as `staged` for publishing tools;
it does not turn old clients into a combined-chain installer. Their existing
planner offers the next applicable target. Android application/data groups stay
complete; the 1.0.3 → 1.0.3.1 content-only update uses the installed application.

Windows/Android 1.0.1 and 1.0.2 routes are explicitly retired through
`platformRetiredVersions`. Those clients receive the Workshop installation guide
instead of an inapplicable patch. The retained 1.0.0 baseline route remains
valid; this is an exact-version retirement, not a new minimum that would disable
the baseline. A cumulative patch applies to its exact installed baseline;
relabeling it cannot make it universal.

## Publishing and retirement

Use Release Studio for full installation sets and Patch Studio for updates.
Both live under `Tools/Release` in the game project. The retention planner's
`baseline-chain` layout previews this policy; no preview publishes or deletes.

Upload and verify immutable payloads first, validate every supported route, then
publish and read back both catalogs. Remove a remote object only after checking
all catalogs, recovery instructions and release references. Preserve local
players, complete installers, source snapshots and historical indexes separately.

The [Android 1.0.2 recovery APK](https://updates.elderlich.com/patches/android/MDPro4-android-1.0.2-recovery.apk)
and its [instructions](https://updates.elderlich.com/patches/android/README-android-1.0.2-recovery.txt)
are manual repair downloads for affected existing installations. They remain
separate from normal update groups and are not new-player installers.
