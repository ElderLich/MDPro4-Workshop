# MDPro4 Workshop

Public delivery endpoint for [MDPro4](https://github.com/ElderLich/MDPro4).

## Release channels

- **Stable:** `update-manifest.json`. Use its per-platform versions and download
  routes to find the available release. The supported MDPro4 baseline is 1.0.0.
- **Test:** `channels/test/update-manifest.json`. The prereleases exercise the
  updater using tiny synthetic fixtures. They are not playable game updates and
  must only be applied to disposable test installations.

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
