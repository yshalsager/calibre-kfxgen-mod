# calibre-kfxgen Docker Mod

Community mod for [LinuxServer.io Calibre](https://github.com/linuxserver/docker-calibre), adding native
[KFX conversion](https://github.com/jloutsch/kfxgen) without Wine or Kindle Previewer. Supports amd64/arm64 and s6 v3.

## Usage

Add to your existing container, keeping its other settings:

```yaml
services:
  calibre:
    image: lscr.io/linuxserver/calibre:latest
    environment:
      - DOCKER_MODS=ghcr.io/yshalsager/calibre-kfxgen-mod:calibre-kfxgen
```

Recreate the container, then select **KFX** in Convert books. **Disable KFX Output** if installed;
it otherwise takes priority. Separate multiple mods with `|`.

Back up `/config` first. The mod installs its bundled plugin at startup, preserving preferences and disabled
plugins. No runtime downloads. To update, pull/recreate the container. To uninstall, remove the mod variable,
recreate the container, then remove kfxgen through Calibre's plugin preferences.

Formatting, Arabic/RTL fidelity, and CLI options differ from Previewer. Test representative books on your Kindle.
Secure the browser desktop according to LinuxServer's documentation.

GPLv3; see [LICENSE](LICENSE). The checksum-verified plugin, pinned source, and upstream license are bundled
in `/defaults/kfxgen/`. Based on the [LinuxServer mod template](https://github.com/linuxserver/docker-mods/tree/template).
