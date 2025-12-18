---
layout: default-layout
title: MAUI Release Notes v3.x - Dynamsoft MRZ Scanner
description: This is the release notes page of Dynamsoft MRZ Scanner for MAUI SDK v3.x.
keywords: release notes, MAUI, version 3.x,
needAutoGenerateSidebar: true
needGenerateH3Content: false
noTitleIndex: true
---

# Release Notes for MAUI SDK - 3.x

## 3.2.5000 (12/18/2025)

### Fixes & Improvements

- Resolved a libPNG CVE issue by updating the C++ dependencies to use the latest libPNG CVE.

## 3.2.3000 (11/19/2025)

- Updated the underlying Capture Vision bundle to 3.2.3000 for major improvements in reading accuracy and speed.
- Neural MRZ Localization – The new MRZLocalization model improves region detection accuracy and delivers up to **42.7% faster processing** for MRZ-based document workflows.
- Configurable Localization Control – The new LocalizationModes parameter allows configuration for text line detection.

## 3.0.5200 (08/18/2025)

### Fixes & Improvements

- Fixed a xcframework signature issue.

## 3.0.5100 (08/12/2025)

### Fixes & Improvements

- Small fixes and tweaks.

## 3.0.3100 (05/30/2025)

### Highlighted Features

- Added a new component `MRZScanner`. Users can quickly set up a MRZ scanning app with the build in UI of `MRZScanner`. The following classes are added to use the `MRZScanner` component:
  - [`MRZScanner`]({{ site.maui_api }}mrz-scanner.html): The main class of `MRZScanner` component. It implements MRZ scanning features.
  - [`MRZScannerConfig`]({{ site.maui_api }}mrz-scanner-config.html): The class that provides MRZ scanning configurations.
  - [`MRZScanResult`]({{ site.maui_api }}mrz-scan-result.html): The MRZ scan result class.
