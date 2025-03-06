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

## 2.0.1 (03/06/2025)

### New

- Added a new method [`setTemplateFile`](../api-reference/mrz-scanner-config.md#settemplatefile) to the class [`MRZScannerConfig`](../api-reference/mrz-scanner-config.md#settemplatefile). You can use this method to set the template file via either a file path or a JSON string. Method [`getTemplateFile`](../api-reference/mrz-scanner-config.md#settemplatefile) is added as well to get what you set.

### Deprecated

- The methods `set/getTemplateFilePath` are deprecated. Please use [`set/getTemplateFile`](../api-reference/mrz-scanner-config.md#templatefile) instead.

## 2.0.0 (01/09/2025)

### New

- Added a new component `MRZScanner`. Users can quickly set up a MRZ scanning app with the build in UI of `MRZScanner`. The following classes are added to use the `MRZScanner` component:
  - [`MRZScannerActivity`]({{ site.android_api }}mrz-scanner-activity.html): The main class of `MRZScanner` component. It is an activity class that implements MRZ scanning features.
  - [`MRZScannerConfig`]({{ site.android_api }}mrz-scanner-config.html): The class that provides MRZ scanning configurations.
  - [`MRZScanResult`]({{ site.android_api }}mrz-scan-result.html): The MRZ scan result class.
