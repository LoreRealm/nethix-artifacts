# Nethix Artifacts

Built binaries and packages for the [Nethix distro](https://github.com/LoreRealm/Nethix), published as release assets so the main repo doesn't carry build outputs.

## Detailed package list

Each component is published in the `nethix-installer-binaries.zip` release asset as a standalone ELF binary under `/usr/local/bin` on the live ISO.

| Package | Type | Purpose |
|---|---|---|
| `summon` | GUI application | Graphical ISO installer (Iced-based), runs as the live user, orchestrates partitioning + package installation |
| `summon-exec` | Privileged helper/backend | The root-privileged counterpart to summon — performs the actual disk writes, chroot setup, and bootloader installation that summon requests |
| `forge` | Hardware-detection CLI | Nethix's hardware detection tool (CachyOS-chwd equivalent), probes GPU/CPU/peripherals and emits sensible package + USE flag selections |
| `scri` | CLI tool | AUR bridge (planned full-paru-style helper) — for now handles fetching/converting AUR PKGBUILDs to Nethix-compatible package builds |

### Per-binary roles in the install flow

- **summon** presents the GUI wizard; user picks disk, locale, kernel flavor, DE (Plasma planned), and package set.
- **summon-exec** is spawned by summon with elevated privileges (pkexec-style) to do destructive steps: partition, format, untar stage3/binhost packages, install kernel + Limine/systemd-boot, and create the user.
- **forge** runs during live boot and post-install to detect hardware (GPU vendor → Mesa/NVIDIA packages, CPU arch → -march optimization tier, audio/bluetooth/peripherals → package and USE flag recommendations). Its output feeds both summon's package selection and the installed system's `/etc/portage/package.use`.
- **scri** is the package-layer bridge for anything outside the Nethix overlay: fetch PKGBUILDs from the AUR, convert them into overlay-compatible ebuilds or pkgbuild-runner jobs, and (per roadmap) scan for suspicious/unmaintained PKGBUILDs before offering the build to the user.

## Contents

- `nethix-installer-binaries.zip` — the four binaries above, published by the main repo's CI (`build-installer` workflow) on green builds
- binhost packages — planned, from `binhost/` nightly builds
- kernel tarball mirrors — planned, if pinning here beats re-downloading (currently the ISO builder pulls `linux-cachyos-7.3.6-1.tar.gz` from the CachyOS GitHub release directly)

## Consumption

    https://github.com/LoreRealm/nethix-artifacts/releases/latest/download/nethix-installer-binaries.zip

The ISO builder (`iso/build.sh` in the main repo) fetches this for prebuilt installer binaries; users wanting a standalone tool can grab the zip directly.

## Publishing

Assets land here automatically via the main repo's `build-installer` GitHub Actions workflow on green CI builds — do not commit binaries directly to this repo, they'll be overwritten on the next release cycle.
