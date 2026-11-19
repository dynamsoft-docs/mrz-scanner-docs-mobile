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

<!-- TODO before the docs go live: replace MM/DD/2026 below with the release date, and update the matching link text and anchor in release-notes/index.md. -->

## 3.6.2000 (MM/DD/2026)

The version jumps from 3.4.1310 to 3.6.2000 to align with the native MRZ Scanner and Dynamsoft Capture Vision.

> [!IMPORTANT]
> **The minimum iOS version is now 16.0** (was 13.0), as required by the scanner's Core ML models. See the [Upgrade Guide](../user-guide/upgrade.md).

### New

- **Per-field MRZ validation**: `MRZData.GetFieldValidationStatus(string)` returns `VS_NONE`, `VS_SUCCEEDED` or `VS_FAILED` for a field's check digit.
  - **Behavior change**: scans that fail check-digit validation are now returned with their status instead of discarded.
- **Camera permission handling**: the scanner never opens the camera without access, and offers **Open Settings** when it's unavailable.
  - New `EnumErrorCode`: `CameraPermissionDenied` (1001) and `CameraPermissionRestricted` (1002).
  - New `MRZScannerConfig.IsCameraPermissionPromptEnabled` (default: `true`) turns off the scanner's own prompt.
- **Scanning progress indicator**: prompts the user to hold the device steady while frames are processed.
- **Flip document prompt**: asks the user to flip TD1 and TD2 IDs whose portrait is on the other side.
- **`MRZScannerConfig()`**: a parameterless constructor, as in the native libraries.

### Fixes & Improvements

- Upgraded the native MRZ Scanner and Capture Vision to 3.6.2000, including a CVE fix and crash fixes.
- The scan region is now the guide frame; with the frame hidden, the whole preview is scanned.
- Fixed small documents going undetected.
- `TemplateFile` is no longer overwritten by `Start`, and accepts inline JSON with leading whitespace.
- `MRZScanner.Start` no longer opens a second scanner on a repeated call, and returns an `Exception` result instead of waiting forever when the scanner can't open.
- Android: the result still arrives when the app's activity is recreated during a scan.
- iOS: the scanner opens over modal pages and can't be swiped away; `PersonalNumber`, `OptionalData1` and `OptionalData2` return `""` instead of `null`; results arrive sooner and memory is released after each scan.

> [!NOTE]
> Apps built with Xcode 27 must adopt the iOS UIScene lifecycle, which the current MAUI template includes.

## 3.4.1310 (05/20/2026)

### Fixes & Improvements

- Improved compatibility with devices running kernels that use a 16 KB memory page size.
- Addressed intermittent crash and hang conditions encountered in certain runtime scenarios.

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
