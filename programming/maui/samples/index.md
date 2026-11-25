---
layout: default-layout
title: Demo & Samples - Dynamsoft MRZ Scanner MAUI Edition
description: The index of Dynamsoft MRZ Scanner MAUI demo & samples.
keywords: demo, sample, index, MAUI
needAutoGenerateSidebar: true
noTitleIndex: false
---

# Demo and Samples

## MRZ Scanner Demo

- [View in Google Play Store](https://play.google.com/store/apps/details?id=com.dynamsoft.mrzscanner){:target="_blank"}
- [View in the App Store](https://apps.apple.com/us/app/dynamsoft-mrz-scanner/id6736854735){:target="_blank"}
- [Download APK](https://download2.dynamsoft.com/mrzscanner/android/DynamsoftMRZScannerDemoAndroid.apk)

## Samples

Two samples scan the MRZ on a passport or ID card using the ready-to-use `MRZScanner`: a minimal one and a complete one. Both run on Android and iOS.

They're separate projects in one repository. In Visual Studio Code, choose which one to run with **.NET MAUI: Select Startup Project**, or open a sample's own folder. Each declares its own application ID, so both can be installed on one device.

### ScanMRZBasic

The smallest thing that scans an MRZ and shows the result: one page, a button, and the parsed fields on the same page. This is the app the [MRZ Scanner User Guide](../user-guide/index.md) builds step by step.

- [C#](https://github.com/Dynamsoft/mrz-scanner-mobile-maui/tree/main/ScanMRZBasic){:target="_blank"}

### ScanMRZ

A complete app built on the same SDK calls, adding a dedicated result page, document images from both sides, per-field validation explanations, and camera-permission recovery. The [ScanMRZ Demo App](scanmrz-walkthrough.md) goes through it section by section.

- [C#](https://github.com/Dynamsoft/mrz-scanner-mobile-maui/tree/main/ScanMRZ){:target="_blank"}

> [!NOTE]
> A physical device is required to run either sample. Simulators and most emulators don't expose a camera.

## Next Steps

- [MRZ Scanner User Guide](../user-guide/index.md) — build `ScanMRZBasic` from an empty project.
- [ScanMRZ Demo App](scanmrz-walkthrough.md) — how the complete sample builds its result page.
- [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) — document type, UI elements, feedback, and camera permission.
- [MAUI API Reference](../api-reference/index.md) — all classes and methods.
