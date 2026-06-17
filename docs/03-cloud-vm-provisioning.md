# 03 — Cloud VM provisioning (x86_64 build host)

Build host decision: **x86_64 cloud VM** (Apple Silicon ruled out — see `01`).

## Target spec

| Resource | Target | Why |
|---|---|---|
| Arch | **x86_64** | Hard requirement (Platform SDK is x86_64-only). |
| OS | **Ubuntu 22.04 LTS** (host) | Modern, well-supported. SDK runs in its own chroot; host distro is flexible. The HAL build chroot is Ubuntu 20.04-based and shipped by the SDK. |
| vCPU | 8+ | Android compile is CPU-bound; more cores = faster. |
| RAM | **32 GB** (16 GB min) | A16 AOSP linking is memory-hungry; 16 GB needs large swap. |
| Disk | **300 GB+** | hybris-23.2 (A16) source sync is large + build output. HADK doc floor (30 GB) is for Android 6 only. |

## HADK-stated minimums (for reference — too low for A16)

- "A 64-bit x86 machine with a 64-bit Linux kernel"
- "At least 30 GiB of free disk space" (Android 6 build) — A16 needs far more
- "At least 4 GiB of RAM (the more the better)"
- Source: https://hadk.sailfishos.org/prerequisites/

## Provider

Recommended: **Hetzner Cloud** (EU-based, fits .nl owner; good price/perf for builds).
A shared-vCPU instance around 16 vCPU / 32 GB / ~360 GB disk matches the target. Exact
SKU/price to be confirmed at provision time (specs change). Any x86_64 provider works.

## Workflow

The Mac will drive setup over **SSH**:
1. Owner provisions the VM with Ubuntu 22.04 and adds the Mac's SSH public key.
2. Owner shares the VM IP/hostname.
3. Setup + build commands are run over `ssh` from the Mac (logged here).

## Status

⏳ Awaiting provider choice + VM provisioning by owner.
