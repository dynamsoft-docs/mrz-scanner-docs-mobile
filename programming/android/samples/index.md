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

## Samples

Four samples scan the MRZ on a passport or ID card using the ready-to-use `MRZScannerActivity`. They come in two pairs — a minimal one and a complete one — each available in Java and Kotlin.

All four are modules of a single Gradle project. Open **`android/samples`** in Android Studio rather than an individual sample folder, then choose the run configuration you want. Each declares its own application ID, so all four can be installed on one device and compared side by side.

### ScanMRZBasic

The smallest thing that scans an MRZ and shows the result: one activity, a button, and the parsed fields on the same screen. This is the app the [MRZ Scanner User Guide](../user-guide/index.md) builds step by step.

- [Java](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZBasic){:target="_blank"}
- [Kotlin](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZBasicKt){:target="_blank"}

### ScanMRZ

A complete app built on the same SDK calls, adding a dedicated result screen, a tabbed pager for the document images from both sides, per-field validation explanations, and camera-permission recovery. The [ScanMRZ Sample Walkthrough](scanmrz-walkthrough.md) goes through it section by section.

- [Java](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ){:target="_blank"}
- [Kotlin](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZKt){:target="_blank"}

Within each pair the Kotlin version is a direct translation that shares the layouts, resources, and screen flow, so pick whichever language matches your project.

> [!NOTE]
> A physical device is required to run any of the samples. The Android Emulator does not expose a camera.

## Next Steps

- [MRZ Scanner User Guide](../user-guide/index.md) — build `ScanMRZBasic` from an empty project.
- [ScanMRZ Sample Walkthrough](scanmrz-walkthrough.md) — how the complete sample builds its result screen.
- [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) — document type, UI elements, feedback, and camera permission.
- [Android API Reference](../api-reference/index.md) — all classes and methods.
