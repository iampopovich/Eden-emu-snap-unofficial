# Eden Fresh (snap)

Unofficial snap of [Eden](https://git.eden-emu.dev/eden-emu/eden), the open-source
Nintendo Switch emulator. It repackages the official Linux AppImage
(`Eden-Linux-<ver>-<arch>-clang-pgo.AppImage`) for amd64 and arm64.

    sudo snap install eden-fresh
    sudo snap connect eden-fresh:joystick

## Automatic updates

`.github/workflows/build-and-publish.yml` runs every 12 hours:

1. `check-upstream` reads the newest stable release from the Eden Forgejo API and
   compares it with the version in the `stable` channel of `eden-fresh`.
2. If they differ, `build-and-publish` builds the snap on native amd64 and arm64
   runners, smoke-tests `eden-cli --version` and uploads to `edge`.
3. `promote-to-stable` promotes `edge` to `stable`.

The version is taken from the upstream release tag at build time
(`adopt-info`), so new releases need no commit here.

Required repository secret: `SNAPCRAFT_STORE_CREDENTIALS`
(`snapcraft export-login --snaps=eden-fresh --channels=edge,stable creds.txt`).

## Notes

- Config, saves and shader cache live in `~/snap/eden-fresh/common`, so they
  survive snap updates.
- Like the upstream AppImage, the UI runs through XWayland by default. Set
  `I_WANT_A_BROKEN_WAYLAND_UI=1` to try native Wayland.
- No firmware, keys or games are included.
