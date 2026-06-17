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

## Provider — Hetzner (chosen)

**Hetzner Cloud** (EU-based, fits .nl owner; good price/perf for builds).

Confirmed instance (from owner's live console, post June-2026 price change):

| Plan | vCPU | RAM | SSD | €/h | €/mo | Note |
|---|---|---|---|---|---|---|
| **CX53** ✅ | 16 (Intel/AMD) | 32 GB | 320 GB | 0.057 | 35.68 | **chosen** — best value, hits target |
| CPX52 | 12 (AMD) | 24 GB | 480 GB | 0.195 | 121.59 | the ~€120 plan owner first saw — overpriced for us |
| CPX62 | 16 (AMD) | 32 GB | 640 GB | 0.252 | 157.29 | dedicated-AMD; only if we need lots more disk |

OS image: **Ubuntu 22.04**. Location: EU.

**Cost control:** Hetzner bills hourly, capped at monthly price; you only pay while the
server exists. A port is days–weeks → delete when a flashable image exists. At €0.057/h,
~1 week ≈ €9.50, ~2 weeks ≈ €19.

**Disk caveat:** CX53 has 320 GB (vs 360 GB target). A16 source + build should fit but may
be tight. If so, attach a Hetzner Volume (~€0.0052/GB/mo, e.g. +100 GB ≈ €5/mo) — no server
resize needed. Watch `df -h` during/after `repo sync`.

## Networking: add IPv4 (€0.50/mo) — do NOT go IPv6-only

Verified 2026-06-17:
- **github.com has no AAAA record** (confirmed from the Mac + current reports) → no native
  IPv6 git clone. Our entire `repo sync` (hundreds of repos, 100 GB+) comes from GitHub.
- **Hetzner has no official NAT64**; IPv6-only reaches IPv4 via third-party `nat64.net`,
  which is rate-limited/flaky — fatal for a long, GitHub-heavy sync.
- IPv4 on Hetzner = **€0.50/mo** (~€0.0007/h) → negligible vs the VM.
- (The Mac itself has working IPv6, so SSH-in would work either way; the build is the
  blocker, not SSH.)

**Decision:** provision CX53 **with a primary IPv4**.

## Workflow

The Mac will drive setup over **SSH**:
1. Owner provisions the VM with Ubuntu 22.04 and adds the Mac's SSH public key.
2. Owner shares the VM IP/hostname.
3. Setup + build commands are run over `ssh` from the Mac (logged here).

## Status

⏳ Awaiting provider choice + VM provisioning by owner.
