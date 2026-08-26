---
layout: default-layout
title: Demo & Samples - Dynamsoft MRZ Scanner Android Edition
description: The index of Dynamsoft MRZ Scanner Android demo & samples.
keywords: demo, sample, index, Android
needAutoGenerateSidebar: true
noTitleIndex: false
---

# Demo and Samples

## MRZScanner Demo

- [View in Google Play Store](https://play.google.com/store/apps/details?id=com.dynamsoft.mrzscanner){:target="_blank"}
- [Download APK](https://download2.dynamsoft.com/mrzscanner/android/DynamsoftMRZScannerDemoAndroid.apk)

## ScanMRZ Sample

Scan the MRZ code from a passport or ID card and extract the information using the ready-to-use component(`MRZScannerActivity`)

Check code on GitHub

- [Java](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ){:target="_blank"}
- [Kotlin](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZKt){:target="_blank"}

The two samples are equivalent. `ScanMRZKt` is a direct translation of `ScanMRZ` that shares its layouts, resources, and screen flow, so pick whichever language matches your project.

Both are modules of a single Gradle project. Open **`android/samples`** in Android Studio rather than an individual sample folder, then choose the `ScanMRZ` or `ScanMRZKt` run configuration. They declare different application IDs, so both can be installed on the same device and compared side by side.

> [!NOTE]
> A physical device is required to run either sample. The Android Emulator does not expose a camera.
