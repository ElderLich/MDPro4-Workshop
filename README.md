# MDPro4 Workshop

Public delivery endpoint for [MDPro4](https://github.com/ElderLich/MDPro4).

## Release channels

- **Stable:** `update-manifest.json`. Use its per-platform versions and download
  routes to find the available release. Windows and Android retain 1.0.0 and
  1.0.1 update routes. Linux and macOS start automatic updates at the fresh full
  1.0.2 installation; obsolete earlier patch routes have been retired.
- **Test:** `channels/test/update-manifest.json`. Historical tiny synthetic fixtures for
  disposable test installations. Their `v1.0.1-test.1` and `v1.0.2-test.1` releases
  are no longer hosted, so these are offline examples, not a working download
  channel. They are not playable game updates.

MDPro3 releases have been retired from this endpoint. MDPro4 is a fresh
installation, including a new Android application identity.

## Build and publish

Use **Release Studio** to compose a complete release snapshot and package an
offline installation. Windows uses a setup EXE with adjacent parts; Android uses
successive APK install parts; Linux and macOS use a graphical setup wizard and
adjacent payload parts. Keep every package's generated files together.

Use **Patch Studio** for later updates. Its Changes view lists added, modified
and removed files. Retain each shipped baseline index so users can update
directly after skipping releases; Windows also supports ordered patch chains.
Keep routes for every supported platform and installed version.

Both applications live in [the main project](https://github.com/ElderLich/MDPro4)
under `Tools/Release`. Prepare sources, review changes, build the patch and use
**Publish update** to verify/upload its payload and update the selected channel.
Platforms can be published incrementally to the same version. Published payloads
remain immutable; the aggregate manifest preserves the other platform routes.

GitHub hosts patch archives and manifests; patch download mirrors are supported.
The production channel is updated only after its packages have been verified.
Test prereleases remain separate from the stable update manifest. Source code
and packaging tools remain in the main project.

## Next update format

The frozen 1.0.2 players need an ordinary compatible update into the planned
1.0.3 player before they can read segmented downloads. Keep the legacy endpoint
and the separate `channels/v2/stable/update-manifest.json` channel independent.
Full Linux/macOS 1.0.2 installers are distributed separately by the maintainer;
no mirror URL is advertised here until that distribution is ready. Historical
indexes and Windows/Android payloads remain preserved.
