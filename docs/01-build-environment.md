# 01 — Build environment (Linux on macOS)

The Sailfish OS **HADK** toolchain is Linux-only. It uses two chroot/SDK layers:

1. **Platform SDK** (`sfossdk`) — Sailfish/Mer side: `repo`, packaging, image build.
2. **HABUILD SDK** (`habuild_sdk`) — an Ubuntu chroot for building the Android HAL.

This Mac (Apple Silicon, macOS) cannot run these natively. Chosen approach:
**run a Linux x86_64 environment on the Mac.**

## ⚠️ Architecture caveat to resolve first [UNVERIFIED]

The HADK build host is historically **x86_64**. This Mac is Apple Silicon (arm64).
Options, in rough order of reliability — **must confirm before committing**:

- **x86_64 Linux cloud VM** (most reliable; sidesteps arch issues entirely).
- **x86_64 Linux VM on Mac via emulation** (e.g. UTM/QEMU) — works but slow for a
  full Android build.
- **arm64 Ubuntu container + x86_64 emulation** — risky; HADK prebuilts may be x86_64-only.

> Decision recorded: "Linux VM/Docker on Mac". Need to pick x86_64-emulated vs an
> arm64 host with x86_64 build tooling. **Open question for owner — see porting log.**

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
