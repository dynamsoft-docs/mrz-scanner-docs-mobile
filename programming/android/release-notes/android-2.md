---
layout: default-layout
title: Android Release Notes v2.x - Dynamsoft MRZ Scanner
description: This is the release notes page of Dynamsoft MRZ Scanner for Android SDK v2.x.
keywords: release notes, android, version 2.x,
needAutoGenerateSidebar: true
needGenerateH3Content: false
noTitleIndex: true
---

# Release Notes for Android SDK - 2.x

## 2.0.0 (01/09/2025)

### New

- Added a new component `MRZScanner`. Users can quickly set up a MRZ scanning app with the build in UI of `MRZScanner`. The following classes are added to use the `MRZScanner` component:
  - [`MRZScannerActivity`]({{ site.android_api }}mrz-scanner-activity.html): The main class of `MRZScanner` component. It is an activity class that implements MRZ scanning features.
  - [`MRZScannerConfig`]({{ site.android_api }}mrz-scanner-config.html): The class that provides MRZ scanning configurations.
  - [`MRZScanResult`]({{ site.android_api }}mrz-scan-result.html): The MRZ scan result class.
