---
layout: default-layout
title: iOS Release Notes v2.x - Dynamsoft MRZ Scanner
description: This is the release notes page of Dynamsoft MRZ Scanner for iOS SDK v2.x.
keywords: release notes, ios, 
needAutoGenerateSidebar: true
needGenerateH3Content: false
noTitleIndex: true
---

# Release Notes for iOS SDK - 2.x

## 2.0.0 (01/09/2025)

### New

- Added a new component `MRZScanner`. Users can quickly set up a MRZ scanning app with the build in UI of `MRZScanner`. The following classes are added to use the `MRZScanner` component:
  - [`DSMRZScannerViewController`]({{ site.ios_api }}mrz-scanner-view-controller.html): The main class of `MRZScanner`. It is an activity class that implements MRZ scanning features.
  - [`DSMRZScannerConfig`]({{ site.ios_api }}mrz-scanner-config.html): The class that provides MRZ scanning configurations.
  - [`DSMRZScanResult`]({{ site.ios_api }}mrz-scan-result.html): The result class.
