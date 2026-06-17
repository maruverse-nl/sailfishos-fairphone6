# 04 — Why we build "Android" for a Linux OS

Sailfish OS is **not Android**. It is a GNU/Linux distro: glibc, RPM packages, a Wayland
compositor (Lipstick) and a Qt/QML UI.

So why does the HADK process build Android components? **Drivers.** The FP6's proprietary
hardware (Snapdragon modem, Adreno GPU, cameras, sensors, Wi-Fi/BT) has no open-source
Linux drivers — only Android binary blobs that target Android's Bionic libc and HAL
interfaces.

Sailfish reuses those Android drivers via **libhybris** / **Halium** instead of rewriting
them:

```
Sailfish OS userspace  (glibc, RPM, Qt/QML, Lipstick UI)   ← the OS (NOT Android)
        │
libhybris  — bridges glibc ⇄ Android bionic                ← the translator
        │
Android HAL + vendor blobs (built from LineageOS FP6 tree)  ← what the "Android build" makes
        │
Linux kernel (FP6, from LineageOS)
```

So the "Android build" is **only the hardware adaptation layer** (kernel + Android HAL +
vendor blobs) for the FP6, taken from ArianK16a's LineageOS tree — not Android-the-OS.
Sailfish's Linux userspace runs on top, with libhybris translating between the two worlds.

This is why a matching base mattered: `hybris-23.2` (HAL side) ↔ `lineage-23.2` (FP6 device
tree). The hard part — working FP6 drivers — already exists in LineageOS; we adapt it.

Sources: https://hadk.sailfishos.org/android/ , https://docs.halium.org/en/latest/project/Planning.html
