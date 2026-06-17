# 00 — Device & base facts (confirmed)

All facts below are confirmed against the cited sources. Anything not yet verified is
marked **[UNVERIFIED]** and must be confirmed before it is relied on.

## Fairphone 6 hardware

| Property | Value | Source |
|---|---|---|
| Marketing name | Fairphone (Gen. 6) | Fairphone support FAQ |
| Codename | `FP6` | ArianK16a device tree |
| SoC | Qualcomm Snapdragon 7s Gen 3 | CNX Software, Wikipedia |
| SoC platform id | **SM7635** | ArianK16a device tree README |
| RAM / storage | 8 GB / 256 GB UFS (+ microSD) | CNX Software |
| Display | 6.31" OLED FHD+ 1116×2484, 120 Hz | CNX Software |
| Battery | 4415 mAh, removable | ArianK16a device tree README |
| Ships with | Android 15 | Wikipedia / Fairphone |
| Now on | Android 16 (rolled out 2026-03-16) | Wikipedia |
| Released | 2025-06-25 | Wikipedia |

## LineageOS base (device trees by ArianK16a)

Branches present in `ArianK16a/android_device_fairphone_FP6`:

- `lineage-23.2` (Android 16) — updated 2026-06-07 (actively maintained) ← **chosen target**
- `lineage-23.1`, `lineage-23.0` (Android 16 line)
- `lineage-22.2` (Android 15) — updated 2025-09-12 (fallback option)
- feature branches: `libperfmgr`, `alertslider`, `sepolicy`, `prebuiltaudio`

There is a published OTA release: `arian-ota/ota` tag `23.2-FP6-36c89234`
(LineageOS 23.2 = Android 16, build 2026-06-11, security patch 2026-06-01, VANILLA variant).

## Sailfish OS / Halium base

`mer-hybris/android` branches (newest first):

- `hybris-23.2` — updated 2026-05-14 (Android 16) ← **chosen base**
- `hybris-22.2` — updated 2026-03-08 (Android 15)
- `hybris-21.0` — 2025-09-17
- `hybris-20.0` — 2024-07-03
- `hybris-18.1` — 2022-09-13

The HADK process reuses the LineageOS device tree on top of a matching `hybris-*` base,
so base version is constrained by which Lineage branches the FP6 tree has. Both 22.2 and
23.2 are available on both sides → either is viable; **23.2 chosen**.

## Sources

- https://www.cnx-software.com/2025/06/26/fairphone-gen-6-sustainable-repairable-6-31-inch-android-15-smartphone-with-snapdragon-7s-gen-3-soc/
- https://en.wikipedia.org/wiki/Fairphone_6
- https://support.fairphone.com/hc/en-us/articles/24463093338898-The-Fairphone-Gen-6-Frequently-Asked-Questions-FAQ
- https://github.com/ArianK16a/android_device_fairphone_FP6 (branches: /branches/all)
- https://github.com/arian-ota/ota/releases/tag/23.2-FP6-36c89234
- https://github.com/mer-hybris/android/branches/all
- https://hadk.sailfishos.org/
- https://docs.halium.org/en/latest/project/Planning.html

## Still to verify [UNVERIFIED]

- [ ] Exact kernel source repo + branch used by the FP6 LineageOS tree
- [ ] FP6 partition layout / A-B vs A-only, dynamic partitions, vendor_boot/init_boot usage
- [ ] Whether `hybris-23.2` has been used for any successful SFOS/Halium device port yet
- [ ] Bootloader unlock procedure + status for this specific unit
- [ ] Whether GSI/Treble or a recovery-based install path is preferred for FP6
