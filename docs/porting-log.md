# Porting log

Chronological record of every step. Newest entries at the bottom.

## Phase 0 — research & setup

### 2026-06-17

- ✅ Confirmed FP6 hardware: Snapdragon 7s Gen 3 / **SM7635**, codename `FP6`,
  Android 15 → 16. (sources in `00-device-facts.md`)
- ✅ Confirmed LineageOS device trees exist (`ArianK16a/android_device_fairphone_FP6`),
  branches `lineage-22.2` (A15) and `lineage-23.2` (A16, actively maintained).
- ✅ Confirmed matching Halium bases exist: `mer-hybris/android` `hybris-22.2` and
  `hybris-23.2`.
- ✅ **Decision:** target **Android 16 / `hybris-23.2`**; fallback `hybris-22.2`.
- ✅ **Decision:** build on a **Linux VM/Docker on macOS**.
- ✅ Installed `gh` 2.94.0 on the Mac; created this tracking repo.

- ✅ **Confirmed device available:** owner has a physical Fairphone 6, bootloader
  unlockable. (Needed for flashing/testing, not for the build.)
- ⚠️ **Confirmed (source-backed):** Apple Silicon **cannot** host the HADK build. Platform
  SDK is x86_64-only; Rosetta/Docker/VM emulation on macOS is unsupported and reported to
  fail. The "Linux VM/Docker on Mac" plan is dropped. (see `01-build-environment.md`)

### Open questions (blocking next steps)

1. **x86_64 build host** — must pick: cloud VM (recommended), an existing physical x86_64
   Linux machine, or (not recommended) slow local emulation. **Awaiting owner.**

### Next planned steps (not yet started)

- [ ] Resolve build-host architecture question with owner.
- [ ] Verify FP6 partition scheme + kernel source (read the LineageOS tree).
- [ ] Stand up the Linux build environment.
- [ ] Install Sailfish Platform SDK + HADK target.
- [ ] `repo` sync the `hybris-23.2` base + FP6 device sources.
