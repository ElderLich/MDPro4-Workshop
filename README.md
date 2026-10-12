# MDPro4 Workshop

Public delivery endpoint for [MDPro4](https://github.com/ElderLich/MDPro4).

## Release channels

- **Stable:** `update-manifest.json`. The MDPro4 baseline is **1.0.0**. No base client is published yet; this manifest deliberately offers no downloads.
- **Test:** `channels/test/update-manifest.json`. Prereleases exercise the updater using tiny synthetic fixtures. These are not playable game updates and must only be applied to disposable test installations.

MDPro3 releases have been retired from this distribution endpoint. MDPro4 is a fresh installation, including a new Android application identity.

## Build and publish

Use **Release Studio** to compose an immutable release snapshot and create a small platform client plus verified resource packs. Upload the base resource parts to your mirrors and enter direct HTTPS download links. Files are checked by size and SHA-256 regardless of the selected mirror.

Use **Patch Studio** to compare a supported baseline with the new snapshot. The Changes view lists added, modified, and removed files. Only those changes go into the patch. Keep each supported baseline so that clients can update directly after skipping releases; retain routes for every supported platform.

Both applications live in [the main project](https://github.com/ElderLich/MDPro4) under `Tools/Release`. Patch Studio creates a draft, verifies published asset bytes, then updates the explicitly selected Workshop channel. Test prereleases never become the stable update manifest.

GitHub hosts regular patch archives and manifests. Base resources may use multiple mirrors and multipart downloads. Source code and packaging tools remain in the main project.
