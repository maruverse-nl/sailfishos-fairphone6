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

- ✅ **Decision:** build host = **Hetzner Cloud, CPX51** (16 vCPU / 32 GB / 360 GB),
  Ubuntu 22.04, EU. (see `03-cloud-vm-provisioning.md`)
- ✅ Documented why an "Android build" is needed for a non-Android OS (libhybris/Halium
  HAL reuse). (see `04-why-android-hal.md`)

- ⏸️ **PAUSED (owner travelling) — 2026-06-17.** Cloud VM not provisioned yet; owner
  reconsidering cost. All Phase 0 decisions are locked below; nothing is blocked except
  picking/standing up the x86_64 host.

### Build-host options when resuming (cost)

- **Hetzner CX53, short sprint + delete:** hourly billing (€0.057/h), delete VM + any
  volume when done → realistic ~€10–20 total, not a recurring €36/mo. Idle = wasted money.
- **Local x86_64 PC/laptop (€0):** any Intel/AMD box that runs Ubuntu 22.04 with ~300 GB
  free + 16 GB RAM works. Best if owner has/can borrow one. (Apple Silicon ruled out.)
- Filesystem for the build dir: **ext4** (case-sensitive required; ext4 = most-tested).

### Resume checklist (Phase 1 entry)

1. Stand up an x86_64 Ubuntu 22.04 host (cloud CX53 *with IPv4*, or local PC), ext4.
2. Add the Mac SSH key (`fp6 maru`) if remote; share IP. Confirm SSH-driven setup is OK.
3. Then Phase 1: system packages → Sailfish Platform SDK (≥4.3.0.15) → HABUILD Ubuntu
   chroot → HADK env (`VENDOR=fairphone`, `DEVICE=FP6`) → `repo init/sync` of `hybris-23.2`
   + ArianK16a FP6 (`lineage-23.2`) device sources.

### Next planned steps (not yet started)

- [ ] Resolve build-host architecture question with owner.
- [ ] Verify FP6 partition scheme + kernel source (read the LineageOS tree).
- [ ] Stand up the Linux build environment.
- [ ] Install Sailfish Platform SDK + HADK target.
- [ ] `repo` sync the `hybris-23.2` base + FP6 device sources.
