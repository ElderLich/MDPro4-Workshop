# Update delivery

Both published catalogs target **1.0.7** on Windows, Android, Linux and macOS. Use ReleaseStudio for complete installers and PatchStudio for patch releases. The 1.0.6 → 1.0.7 steps and original-baseline cumulative updates are available on both channels. [Player installation and update guide](../README.md)

| Catalog | Readers and payloads |
| --- | --- |
| [Stable legacy](../update-manifest.json) | Installed clients that require whole archives. The first update that introduces segmented-download support must be a whole archive. |
| [Stable v2](../channels/v2/stable/update-manifest.json) | Clients that can verify archive segments. Use whole archives when an installed reader requires them. |

If an installation uses a custom test catalog, close the game and remove just the `MDPro4UpdateManifestUrl` override from `Data/config.conf`. Reopen it to check the default channel.

## Supported routes

Keep a cumulative route to the latest release from Windows/Android **1.0.0** and Linux/macOS **1.0.2**. Also retain the adjacent chain starting at **1.0.4**, including the shared **1.0.5 → 1.0.5.1** resources. Use whole archives for Linux 1.0.6 readers until every shipped variant can handle the proposed format.

Windows can queue multiple releases for one installer handoff. Android, Linux and macOS install one offered release per restart. Keep each Android resource and APK pair together, including the 1.0.5 APK while a supported step references it. Exact retired intermediate versions receive the full-installation guide.

## Before publishing

1. Build the incremental and original-baseline packages from the correct installed-file indexes. Keep the legacy and v2 catalogs separate.
2. Verify each required archive, segment and APK at its public URL, including its size and checksum. Run the client planner against both catalogs for every supported starting version.
3. Publish the verified catalog changes. Read them back and recheck reachability before retiring superseded public payloads.

The game checkout contains repeatable three- and five-package tests. Run these from its root:

```powershell
python -B -m unittest discover -s Tools/Release/tests -p test_client_update_download.py -v
python -B -m unittest discover -s Tools/Release/tests -p test_multi_release_updates.py -v
```

These exercise ordered downloads, interruption, verified-cache reuse and the final installation handoff. The five-patch fixture also checks the installed bytes and rollback. Use finished players for platform installation and relaunch acceptance.
