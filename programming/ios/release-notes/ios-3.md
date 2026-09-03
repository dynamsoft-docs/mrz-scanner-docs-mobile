---
layout: default-layout
title: iOS Release Notes v3.x - Dynamsoft MRZ Scanner
description: This is the release notes page of Dynamsoft MRZ Scanner for iOS SDK v3.x.
keywords: release notes, ios, 
needAutoGenerateSidebar: true
needGenerateH3Content: false
noTitleIndex: true
---

# Release Notes for iOS SDK - 3.x

## 3.6.2000 (09/08/2026)

The version number jumps from 3.4.1300 to 3.6.2000 to stay aligned with the Dynamsoft Capture Vision base the SDK ships against. There were no public 3.5.x or 3.6.1000 releases.

### New

- **Per-field MRZ validation**: `MRZData.getFieldValidationStatus(_:)` reports whether a parsed field agrees with its check digit, returning a `ValidationStatus` of `.none`, `.succeeded`, or `.failed`. It accepts the same field names as the `MRZData` properties. The composite fields `dateOfBirth`, `dateOfExpire`, and `mrzText` report the worst status among their components.
  - **Behavior change**: scans that fail check-digit validation are no longer discarded. The parsed values are returned with their validation status, so you can accept them, prompt for a re-scan, or request manual correction.

- **Camera permission handling**: `MRZScannerViewController` now gates the camera on `AVCaptureDevice` authorization and never opens it without access. When access is unavailable it shows an alert offering **Open Settings**, then reports the outcome.
  - New `ErrorCode` (`DSMRZErrorCode` in Objective-C): `cameraPermissionDenied` (1001), which the user can resolve through Settings, and `cameraPermissionRestricted` (1002), withheld by device policy. Both are returned by `errorCode` with a status of `.exception`. The bundle owns codes 1000–1999; Capture Vision codes are all `<= 0`.
  - New `isCameraPermissionPromptEnabled` (default: `true`) suppresses the alert for integrators presenting their own UI. Denials are still reported.

- **Scanning progress indicator**: Shown while the scanner is actively processing frames, prompting the user to hold the device steady.

- **Flip document prompt**: For TD1 and TD2 IDs with the portrait on the opposite side, the scanner now prompts the user to flip the document after the MRZ is captured.

### Fixes & Improvements

- Upgraded the Dynamsoft Capture Vision base to 3.6.2000, addressing a known CVE and including crash fixes. Also adds MRZ text-line orientation detection, so an MRZ rotated 180° can be read.
- Constrained the scan region to the guide frame. Previously the full camera preview was analyzed, so a document held outside the guide could be accepted.
- Fixed the scan region staying clipped to an invisible box when the guide frame was hidden with `isGuideFrameVisible = false`. Hiding the frame previously hid only its drawing while its constraints still limited capture; the whole preview is now scanned. The scanning spinner and flip prompt were also reparented so they survive the frame being hidden.
- Fixed small documents going undetected. Capture Vision's automatic quadrilateral filtering rejected documents occupying a small share of the preview once the scan region covered all of it.
- Fixed the guide frame border finishing its fade to white 300 ms before the result view appeared, leaving the success message above a white frame. The green is now held through the handover.
- The `DynamsoftMRZScannerBundle.xcframework` is now published unsigned, so archive builds no longer require access to the signing team that produced it.

## 3.4.1300 (04/27/2026)

### Fixes & Improvements

- Fixed an issue where the portrait extraction task failed to signal completion on TD2 documents, stalling the transition to the result view despite successful portrait detection.

## 3.4.1200 (04/02/2026)

### New

- **Image results**: `MRZScanResult` now returns captured images alongside the parsed MRZ data. Three types of images can be retrieved from the result:
  - **Document image**: A cropped and perspective-corrected image of the document, available via `getDocumentImage(_ side: DocumentSide)`. Returned by default.
  - **Original image**: The raw full-frame camera image captured at the moment of scanning, available via `getOriginalImage(_ side: DocumentSide)`. Disabled by default.
  - **Portrait image**: The detected portrait extracted from the document, available via `getPortraitImage()`. Returned by default.
  - For two-sided ID cards, images for both sides are accessible by passing `DocumentSide.mrz` or `DocumentSide.opposite` to `getDocumentImage()` and `getOriginalImage()`.

- **New `DocumentSide` enumeration**: Added `DSDocumentSide` (`DocumentSide` in Swift) with values `DSDocumentSideMrz` / `.mrz` (the side containing the MRZ) and `DSDocumentSideOpposite` / `.opposite` (the reverse side) to identify which side of a document an image belongs to.

- **Image return configuration**: Added three new `MRZScannerConfig` properties to control which images are included in the scan result:
  - `returnDocumentImage` (default: `true`)
  - `returnOriginalImage` (default: `false`)
  - `returnPortraitImage` (default: `true`)

- **Additional MRZ data fields**: `MRZData` now exposes the following additional parsed fields:
  - `issuingStateRaw` — the raw issuing state value as encoded in the MRZ, before standardization.
  - `nationalityRaw` — the raw nationality value as encoded in the MRZ, before standardization.
  - `optionalData1` — the first optional data field (nullable).
  - `optionalData2` — the second optional data field (nullable).
  - `personalNumber` — the personal number field, typically present on TD3 passport documents (nullable).

- **New UI button visibility controls**: Added three new `MRZScannerConfig` properties to control the visibility of toggle buttons in the scanning UI:
  - `isBeepButtonVisible` — shows or hides the beep sound toggle button (default: `true`).
  - `isVibrateButtonVisible` — shows or hides the vibration toggle button (default: `true`).
  - `isFormatSelectorVisible` — shows or hides the document format selector at the bottom of the scanning UI (default: `true`).

### Changed

- **`MRZScanResult.data` is now nullable**: The `data` property on `MRZScanResult` has been changed from a non-optional type to a nullable type (`nullable DSMRZData*` / `MRZData?` in Swift). Code that accesses `result.data` without a nil check must be updated. See the [upgrade guide](../user-guide/upgrade.md#mrzscanresultdata-is-now-nullable) for migration details.

- **`errorMessage` renamed to `errorString`**: The `errorMessage` property on `MRZScanResult` has been renamed to `errorString`. Update any references in your code. See the [upgrade guide](../user-guide/upgrade.md#errormessage-renamed-to-errorstring) for migration details.

## 3.2.5000 (12/18/2025)

### Fixes & Improvements

- Resolved a libPNG CVE issue by updating the C++ dependencies to use the latest libPNG CVE.

## 3.2.3000 (11/05/2025)

### Fixes & Improvements

- Resolved an issue where scanning could take longer than expected.
- Fixes & Improvements a potential crash that could occur in certain scenarios.

## 3.2.1000 (10/16/2025)

- 🚀 **High-Speed and Precise MRZ Region Detection**: Revolutionary neural network `MRZLocalization` model delivers **42.7% faster processing** with enhanced region detection accuracy for passport and ID workflows

## 3.0.5200 (08/18/2025)

### Fixes & Improvements

- Fixes & Improvements an xcframework signature issue.

## 3.0.5100 (08/05/2025)

### Fixes & Improvements

- Small fixes and tweaks.

## 3.0.5000 (07/29/2025)

### Fixes & Improvements

- **Default Camera Changed**: To address short-distance focusing issues on Pro Max iPhones, the default camera has been switched to `BackDualWideAuto`, enabling automatic switching between the wide and ultra-wide cameras.
- Fixes & Improvements various minor bugs and improved overall stability.

## 3.0.0 (05/15/2025)

### Fixes & Improvements

- Improved the read-rate and the accuracy of MRZ text recognition.
