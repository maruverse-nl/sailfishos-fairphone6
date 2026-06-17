# 01 — Build environment (Linux on macOS)

The Sailfish OS **HADK** toolchain is Linux-only. It uses two chroot/SDK layers:

1. **Platform SDK** (`sfossdk`) — Sailfish/Mer side: `repo`, packaging, image build.
2. **HABUILD SDK** (`habuild_sdk`) — an Ubuntu chroot for building the Android HAL.

This Mac (Apple Silicon, macOS) cannot run these natively. Chosen approach:
**run a Linux x86_64 environment on the Mac.**

## ⚠️ RESOLVED: Apple Silicon cannot host the build (must use x86_64 Linux)

**Confirmed against sources (2026-06-17):** the Sailfish Platform SDK / HADK host is
**Intel x86_64 only**. Apple Silicon is explicitly *not supported*.

- Sailfish OS docs: "The Sailfish Platform SDK runs on Linux machines with Intel x86
  CPUs, Apple silicon is not supported."
- Jolla developer (vige) on the forum: "You can't run x86 vms on arm. Rosetta does not
  help with that." Docker emulation on macOS for this is unsupported, no plans (still
  true as of Feb 2026). Contributors report `exec format error` under
  Parallels/UTM/Docker.
- The community `sailfishos-open/docker-sailfishos-builder` only packages RPMs — it does
  **not** build HAL/HADK adaptations, so it does not help here.

> The earlier "Linux VM/Docker on Mac" decision is therefore **not viable** for the HAL
> build. We need a genuine x86_64 Linux host. Re-deciding with the owner — see porting log.

### Viable x86_64 options

- **x86_64 cloud VM** (recommended): no emulation, fast, disposable. Needs ~16 GB+ RAM,
  ~200 GB+ disk. Cheap providers (e.g. Hetzner) fit well.
- **Physical x86_64 Linux box** (if owner has one): ideal, no recurring cost.
- **x86_64 full-system emulation on the Mac (UTM/QEMU)**: technically possible, officially
  unsupported, and *very* slow for a full Android build — not recommended.

## Baseline requirements (from HADK)

- Ubuntu LTS (commonly 20.04/22.04) x86_64 inside the VM/container
- 16 GB+ RAM, ~200+ GB free disk
- The Sailfish OS **Platform SDK** + **HADK target** tarballs
- `ubu-chroot` / `habuild` Ubuntu chroot for the HAL build

## Plan

1. Stand up an x86_64 Ubuntu LTS environment (VM or container) — TBD per caveat above.
2. Install the Platform SDK per the HADK "Setting up the SDKs" chapter.
3. Set HADK env vars (`$VENDOR=fairphone`, `$DEVICE=FP6`, release version) — exact values
   to be confirmed against HADK + device tree.
4. `repo init`/`sync` the `hybris-23.2` manifest.
5. Continue into the HAL build chapter.

## Sources

- https://hadk.sailfishos.org/setupsdk/
- https://hadk.sailfishos.org/prerequisites/
- https://hadk.sailfishos.org/android/
