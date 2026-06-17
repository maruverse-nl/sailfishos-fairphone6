# Sailfish OS — Fairphone 6 port

Tracking repo for porting **Sailfish OS** to the **Fairphone (Gen. 6)** (codename `FP6`,
Snapdragon 7s Gen 3 / SM7635).

## Goal

Produce a bootable Sailfish OS hardware adaptation for the Fairphone 6, built with the
official **HADK** (Hardware Adaptation Development Kit) on top of a matching Halium /
LineageOS base.

## Decisions (locked)

| Decision | Choice | Notes |
|---|---|---|
| Android base | **Android 16 — `hybris-23.2`** | Matches actively-maintained FP6 LineageOS tree. Bleeding-edge for Halium; fallback to `hybris-22.2` (A15) stays open. |
| Build host | **Linux VM / Docker on macOS** | HADK is Linux-only; this Mac cannot build natively. |
| Tracking | This repo under `maruverse-nl` | Per-step log in [`docs/porting-log.md`](docs/porting-log.md). |

## Working rules (from project owner)

1. **No guessing** — every technical claim is confirmed against a cited source.
2. **Ask before moving forward** when unsure.
3. **Track every step** in this repo.

## Status

🟡 **Phase 0 — research & environment setup.** See the porting log for the live step list.

## Docs

- [`docs/00-device-facts.md`](docs/00-device-facts.md) — confirmed hardware & base facts (sourced)
- [`docs/01-build-environment.md`](docs/01-build-environment.md) — Linux build env on macOS
- [`docs/02-base-and-trees.md`](docs/02-base-and-trees.md) — Halium base + device trees
- [`docs/porting-log.md`](docs/porting-log.md) — chronological step log
