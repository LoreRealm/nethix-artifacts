# Nethix Artifacts

Built binaries and packages for the [Nethix distro](https://github.com/LoreRealm/nethix), published as release assets so the main repo doesn't carry build outputs.

## Contents

- `nethix-installer-binaries.zip` — summon (GUI installer), summon-exec (privileged backend), forge (hw-detect), scri (AUR bridge) — published by the main repo's CI on green builds
- binhost packages — planned, from `binhost/` nightly builds
- kernel tarball mirrors — planned, if pinning here beats re-downloading

## Consumption

    https://github.com/LoreRealm/nethix-artifacts/releases/latest/download/nethix-installer-binaries.zip

The ISO builder (`iso/build.sh` in the main repo) fetches this for prebuilt installer binaries; users wanting a standalone tool can grab the zip directly.
