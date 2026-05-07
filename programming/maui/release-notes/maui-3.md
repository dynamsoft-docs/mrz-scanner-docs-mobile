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

## 3.4.1300 (05/07/2026)

### Fixes & Improvements

- Fixed an issue where the portrait extraction task failed to signal completion on TD2 documents, stalling the transition to the result view despite successful portrait detection.

## 3.4.1200 (04/17/2026)

### New

- **Image results**: `MRZScanResult` now returns captured images alongside the parsed MRZ data. Three types of images can be retrieved from the result:
  - **Document image**: A cropped and perspective-corrected image of the document, available via `GetDocumentImage(EnumDocumentSide)`. Returned by default.
  - **Original image**: The raw full-frame camera image captured at the moment of scanning, available via `GetOriginalImage(EnumDocumentSide)`. Disabled by default.
  - **Portrait image**: The detected portrait extracted from the document, available via `GetPortraitImage()`. Returned by default.
  - For two-sided ID cards, images for both sides are accessible by passing `EnumDocumentSide.MRZ` or `EnumDocumentSide.Opposite` to `GetDocumentImage()` and `GetOriginalImage()`.

- **New `EnumDocumentSide` enumeration**: Added [`EnumDocumentSide`]({{ site.maui_api }}document-side.html) with values `MRZ` (the side containing the MRZ) and `Opposite` (the reverse side) to identify which side of a document an image belongs to.

- **Image return configuration**: Added three new `MRZScannerConfig` properties to control which images are included in the scan result:
  - `ReturnDocumentImage` (default: `true`)
  - `ReturnOriginalImage` (default: `false`)
  - `ReturnPortraitImage` (default: `true`)

- **Additional MRZ data fields**: `MRZData` now exposes the following additional parsed fields:
  - `IssuingStateRaw` — the raw issuing state value as encoded in the MRZ, before standardization.
  - `NationalityRaw` — the raw nationality value as encoded in the MRZ, before standardization.
  - `OptionalData1` — the first optional data field (nullable).
  - `OptionalData2` — the second optional data field (nullable).
  - `PersonalNumber` — the personal number field, typically present on TD3 passport documents (nullable).

- **New UI button visibility controls**: Added three new `MRZScannerConfig` properties to control the visibility of toggle buttons in the scanning UI:
  - `IsBeepButtonVisible` — shows or hides the beep sound toggle button (default: `true`).
  - `IsVibrateButtonVisible` — shows or hides the vibration toggle button (default: `true`).
  - `IsFormatSelectorVisible` — shows or hides the document format selector at the bottom of the scanning UI (default: `true`).

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
