# Update delivery

Players should use the [in-game updater and installation guidance](../README.md).
This page records the publishing contract for maintainers and tool authors.

## Catalogs

| Channel | Path | Readers |
| --- | --- | --- |
| Stable legacy | [`update-manifest.json`](../update-manifest.json) | Older installed players; compatible whole archives through 1.0.6 |
| Stable v2 | [`channels/v2/stable/update-manifest.json`](../channels/v2/stable/update-manifest.json) | Upgraded players; whole archives or verified segments, including the shared 1.0.5.1 resource step |
| Historical test fixtures | [`channels/test/update-manifest.json`](../channels/test/update-manifest.json) | Offline synthetic examples; their former assets are not hosted |

GitHub Releases provides the current Android application update, **1.0.6**.
It is not a complete first installation. Retain the **1.0.5 APK** while the
supported **1.0.4 → 1.0.5** groups reference it; publishing a newer APK does not
remove that dependency. The shared **1.0.5.1** resource step retains the 1.0.5
application and requires no extra APK.

Full installer sets are distributed through the maintainer's mirrors. Patch
archives are served from `https://updates.elderlich.com/patches/`.

## Retained patch layout

Keep **one cumulative original-baseline-to-latest patch per platform**, plus
adjacent steps starting at **1.0.4**. Remove superseded baseline-to-older-target
variants. Retain complete installed-file indexes independently of payloads.

| Platform | Cumulative baseline patch | Adjacent steps in both channels |
| --- | --- | --- |
| Windows | 1.0.0 → 1.0.6 | 1.0.4 → 1.0.5 → 1.0.5.1 → 1.0.6 |
| Android | 1.0.0 → 1.0.6 | 1.0.4 → 1.0.5 → 1.0.5.1 → 1.0.6 |
| Linux | 1.0.2 → 1.0.6 | 1.0.4 → 1.0.5 → 1.0.5.1 → 1.0.6 |
| macOS | 1.0.2 → 1.0.6 | 1.0.4 → 1.0.5 → 1.0.5.1 → 1.0.6 |

Each platform has exactly **two routes into 1.0.6**: its original baseline and
**1.0.5.1**. There is no direct 1.0.5 → 1.0.6 bridge. Both channels retain
**1.0.4 → 1.0.5 → 1.0.5.1** so supported installations can reach those routes.
The shared 1.0.5.1 resource ZIP is an ordinary archive and is valid in legacy
as well as v2.

Legacy uses whole archives throughout. V2 uses the same whole baseline
archives; Windows, Android and macOS may use verified segments from 1.0.5.1.
Linux uses a **whole TAR.GZ** for both routes because its installed reader
cannot safely prepare segments. A future Linux update must account for both
original and rebuilt players reporting 1.0.6; the version alone does not prove
segmented-download capability.

Keep legacy and v2 schemas separate. Do not place segments in legacy, relabel a
delta for another starting version, or combine a whole archive and its segments
in the same update group. A cumulative delta is not a full installer.

Windows can download and apply its offered ordered chain in one transaction.
Android, Linux and macOS apply one offered step per installation/restart.
`platformUpdateRouting` marks those platforms as `staged` for publishing tools;
it does not turn old clients into combined-chain installers. Android application
and resource rows must stay together for every group that requires both.

The rebuilt Android application remains version 1.0.6 with Android versionCode
8 and the same application ID and signer. Its newly composed patch metadata is
paired with that APK. Android resource directories and runtime bundles remain
the verified 1.0.6 content. Original Linux 1.0.6 installations reinstall the
existing baseline into a fresh directory and then take the cumulative update;
there is no same-version automatic edge or manual player-overlay package.

## Retired installations

The original baselines remain supported: **Windows/Android 1.0.0** and
**Linux/macOS 1.0.2**. Intermediate installations below **1.0.4** are retired
and require a full installation. `platformRetiredVersions` records the exact
intermediate versions between each platform's baseline and current release;
Linux/macOS versions below 1.0.2 remain outside their supported baseline policy.
`releasePageUrl` directs affected users to the Workshop installation guide.

Do not raise every platform's minimum to 1.0.4: that would incorrectly disable
the retained original-baseline cumulative patches. No Windows 1.0.3 or 1.0.3.1
direct compatibility exception remains. The one-time Windows updater recovery
and the older Android 1.0.2 recovery are retired; affected users follow the
full-installation guidance in the README.

## Publishing and retirement

Use Release Studio for full installation sets and Patch Studio for updates.
Both live under `Tools/Release` in the game project. The retention planner's
`baseline-chain` layout honors the manifest's exact retired versions and keeps
the remaining adjacent chain. A preview neither publishes nor deletes.

Normally, published payload bytes are immutable. For this explicitly approved
same-version repair, both catalogs were disabled before replacing the affected
Android files under their original names. Keep the old local payloads and
receipts. Verify the new stored size/SHA-256, deterministic ETag and normal
canonical public response; if an edge still serves the old identity, purge only
the exact affected URLs and verify again before restoring either catalog.

Upload and verify all referenced payloads and the signed APK, validate every
supported route, then publish both catalogs and this guidance in one Workshop
commit. Read back the exact commit and canonical URLs. No full installer was
rebuilt as part of this repair. Remove a remote object only after checking
all channels, historical release catalogs, release descriptions and maintained
documentation. In particular, retain the 1.0.5 Android APK while its staged route
is supported. Preserve local players, complete installers, source dependencies
and historical indexes separately from remote retirement.

Workshop's test channel is a historical offline fixture. Three/five-package
download and five-patch application coverage runs through the maintained
production-code fixtures in the game repository. Catalog and payload identity
checks establish publication consistency; they do not claim installation and
launch acceptance on every native platform or user device.
