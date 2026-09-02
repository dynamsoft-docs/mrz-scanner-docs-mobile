---
layout: default-layout
title: Android Release Notes v3.x - Dynamsoft MRZ Scanner
description: This is the release notes page of Dynamsoft MRZ Scanner for Android SDK v3.x.
keywords: release notes, android, version 3.x,
needAutoGenerateSidebar: true
needGenerateH3Content: false
noTitleIndex: true
---

# Release Notes for Android SDK - 3.x

## 3.6.2000 (09/08/2026)

The version number jumps from 3.4.1300 to 3.6.2000 to stay aligned with the Dynamsoft Capture Vision base the SDK ships against. There were no public 3.5.x or 3.6.1000 releases.

### New

- **Per-field MRZ validation**: `MRZData.getFieldValidationStatus(String)` reports whether a parsed field agrees with its check digit, returning `VS_NONE`, `VS_SUCCEEDED`, or `VS_FAILED`. It accepts the same field names as the `MRZData` getters. The composite fields `dateOfBirth`, `dateOfExpire`, and `mrzText` report the worst status among their components.
  - **Behavior change**: scans that fail check-digit validation are no longer discarded. The parsed values are returned with their validation status, so you can accept them, prompt for a re-scan, or request manual correction.

- **Camera permission handling**: `MRZScannerActivity` now gates the camera on permission and never opens it without access. When access is unavailable it shows a dialog offering **Allow camera access** or **Open Settings**, then reports the outcome.
  - New `EnumErrorCode`: `EC_CAMERA_PERMISSION_DENIED` (1001), which the user can resolve, and `EC_CAMERA_PERMISSION_RESTRICTED` (1002), withheld by device policy. Both are returned by `getErrorCode()` with a status of `RS_EXCEPTION`. The bundle owns codes 1000–1999; Capture Vision codes are all `<= 0`.
  - New `setCameraPermissionPromptEnabled(boolean)` / `isCameraPermissionPromptEnabled()` (default: `true`) suppress the dialog for integrators presenting their own UI. Denials are still reported.

- **Scanning progress indicator**: Shown while the scanner is actively processing frames, prompting the user to hold the device steady.

- **Flip document prompt**: For TD1 and TD2 IDs with the portrait on the opposite side, the scanner now prompts the user to flip the document after the MRZ is captured.

### Fixes & Improvements

- Upgraded the Dynamsoft Capture Vision base to 3.6.2000, addressing a known CVE and including crash fixes. Also adds MRZ text-line orientation detection, so an MRZ rotated 180° can be read.
- Fixed a memory leak that retained the native image buffers of every completed scan, so sustained scanning could eventually exhaust memory. Passing a scan result between activities requires no manual image handling.
- Constrained the scan region to the guide frame. Previously the full camera preview was analyzed, so a document held outside the guide could be accepted. When the guide frame is hidden with `setGuideFrameVisible(false)`, the whole preview is scanned instead, and the prompt text is now hidden along with the frame.
- Fixed small documents going undetected after the user switched format with the bottom selector. The document area threshold was applied only to the format the scanner started with.
- Fixed the guide frame border reverting to white at the moment the success message appeared, so the two now overlap as intended.
- Fixed an `UnsatisfiedLinkError` that could occur when an `MRZScanResult` was restored in a freshly started process.

## 3.4.1300 (05/20/2026)

### Fixes & Improvements

- Improved compatibility with devices running kernels that use a 16 KB memory page size.
- Addressed intermittent crash and hang conditions encountered in certain runtime scenarios.

## 3.4.1200 (04/02/2026)

### New

- **Image results**: `MRZScanResult` now returns captured images alongside the parsed MRZ data. Three types of images can be retrieved from the result:
  - **Document image**: A cropped and perspective-corrected image of the document, available via `getDocumentImage(EnumDocumentSide)`. Returned by default.
  - **Original image**: The raw full-frame camera image captured at the moment of scanning, available via `getOriginalImage(EnumDocumentSide)`. Disabled by default.
  - **Portrait image**: The detected portrait extracted from the document, available via `getPortraitImage()`. Returned by default.
  - For two-sided ID cards, images for both sides are accessible by passing `EnumDocumentSide.DS_MRZ` or `EnumDocumentSide.DS_OPPOSITE` to `getDocumentImage()` and `getOriginalImage()`.

- **New `EnumDocumentSide` enumeration**: Added `EnumDocumentSide` with values `DS_MRZ` (the side containing the MRZ) and `DS_OPPOSITE` (the reverse side) to identify which side of a document an image belongs to.

- **Image return configuration**: Added three new `MRZScannerConfig` methods to control which images are included in the scan result:
  - `setReturnDocumentImage(boolean)` (default: `true`)
  - `setReturnOriginalImage(boolean)` (default: `false`)
  - `setReturnPortraitImage(boolean)` (default: `true`)

- **Additional MRZ data fields**: `MRZData` now exposes the following additional parsed fields:
  - `getIssuingStateRaw()` — the raw issuing state value as encoded in the MRZ, before standardization.
  - `getNationalityRaw()` — the raw nationality value as encoded in the MRZ, before standardization.
  - `getOptionalData1()` — the first optional data field (nullable).
  - `getOptionalData2()` — the second optional data field (nullable).
  - `getPersonalNumber()` — the personal number field, typically present on TD3 passport documents (nullable).

- **New UI button visibility controls**: Added three new `MRZScannerConfig` methods to control the visibility of toggle buttons in the scanning UI:
  - `setBeepButtonVisible(boolean)` — shows or hides the beep sound toggle button (default: `true`).
  - `setVibrateButtonVisible(boolean)` — shows or hides the vibration toggle button (default: `true`).
  - `setFormatSelectorVisible(boolean)` — shows or hides the document format selector at the bottom of the scanning UI (default: `true`).

## 3.2.5000 (12/18/2025)

### Fixes & Improvements

- Resolved a libPNG CVE issue by updating the C++ dependencies to use the latest libPNG CVE.

## 3.2.3000 (11/05/2025)

### Fixed

- Resolved an issue where scanning could take longer than expected.
- Fixed a potential crash that could occur in certain scenarios.

## 3.2.1000 (10/16/2025)

- 🚀 **High-Speed and Precise MRZ Region Detection**: Revolutionary neural network `MRZLocalization` model delivers **42.7% faster processing** with enhanced region detection accuracy for passport and ID workflows

## 3.0.5000 (07/29/2025)

### New

- **Supported 16 KB page sizes**.

### Fixed

- Fixed various minor bugs and improved overall stability.

## 3.0.3100 (05/30/2025)

- Fixed a bug where the camera might not be opened.

## 3.0.0 (05/15/2025)

### Improved

- Improved the read-rate and the accuracy of MRZ text recognition.
