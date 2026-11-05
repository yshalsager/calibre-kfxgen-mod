# calibre-kfxgen Docker Mod

Community [LinuxServer.io Calibre](https://github.com/linuxserver/docker-calibre) mod for native
[KFX conversion](https://github.com/jloutsch/kfxgen), without Wine or Previewer. Supports amd64/arm64 and s6 v3.

Add to your existing Calibre container, keeping its other settings:

```yaml
services:
  calibre:
    image: lscr.io/linuxserver/calibre:latest
    environment:
      - DOCKER_MODS=ghcr.io/yshalsager/calibre-kfxgen-mod:calibre-kfxgen
```

Recreate the container, then select **KFX** in Convert books. Disable **KFX Output** if installed;
it otherwise takes priority. Separate multiple mods with `|`. Secure the desktop per LinuxServer's docs.

At every boot, the mod installs its bundled plugin as `abc` into `/config/.config/calibre`, preserving
preferences and disabled plugins. No runtime downloads. Back up `/config` first. Removing the mod variable
does not uninstall the plugin; remove it through Calibre's plugin preferences afterward.

## Updates and publishing

- Repository: [yshalsager/calibre-kfxgen-mod](https://github.com/yshalsager/calibre-kfxgen-mod); default branch: **`calibre-kfxgen`**.
- Renovate checks before 05:00 Saturday (Africa/Cairo), groups non-major updates, and auto-merges passing
  minor/patch/pin/digest updates after a three-day release age. Major updates stay manual. Keep GitHub
  auto-merge enabled and **test** required. kfxgen's version/checksum/source-commit pins update together.
- Pushes run the smoke test, then LinuxServer's reusable mod builder publishes amd64/arm64 images to GHCR
  using `GITHUB_TOKEN`. Set the package public after its first push. No PAT or registry password needed.
- Tags follow the LS template: `calibre-kfxgen`, `calibre-kfxgen-v<version>`, and commit-qualified variants.
  Recreate your container to apply updates; use a commit-qualified tag to pin the exact mod.

Images are published after a successful Build Image run. For another repository, change the builder's `GITHUB_REPO` and
`ENDPOINT`, the Dockerfile source label, and usage examples.

## Validation and limits

Run `shellcheck root/etc/s6-overlay/s6-rc.d/init-mod-calibre-kfxgen/run tests/smoke.sh` and `bash tests/smoke.sh`.
The Docker test checks a single image layer, s6 startup, PUID/PGID ownership, conversion, and preference
preservation across restart; it cleans up its own containers/images. GUI, arm64 execution, and Kindle
rendering are not covered. Formatting/Arabic/RTL fidelity and CLI options differ from Previewer.

GPLv3; see [LICENSE](LICENSE). Adapted from the [LS mod template](https://github.com/linuxserver/docker-mods/tree/template).
The plugin is verified against its committed SHA-256; the full source is fetched by immutable commit.
Renovate updates both pins. Source and upstream license are bundled in `/defaults/kfxgen/`; no proprietary binaries are included.
