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

## 3.4.1000 (03/26/2026)

### New

- **Image results**: `MRZScanResult` now returns captured images alongside the parsed MRZ data. Three types of images can be retrieved from the result:
  - **Document image**: A cropped and perspective-corrected image of the document, available via `getDocumentImage(_ side: DocumentSide)`. Returned by default.
  - **Original image**: The raw full-frame camera image captured at the moment of scanning, available via `getOriginalImage(_ side: DocumentSide)`. Disabled by default.
  - **Portrait image**: The detected portrait extracted from the document, available via `getPortraitImage()`. Returned by default.
  - For two-sided ID cards, images for both sides are accessible by passing `DocumentSide.mrz` or `DocumentSide.opposite` to `getDocumentImage()` and `getOriginalImage()`.

- **New `DocumentSide` enumeration**: Added `DSDocumentSide` (`DocumentSide` in Swift) with values `DSDocumentSideMRZ` / `.mrz` (the side containing the MRZ) and `DSDocumentSideOpposite` / `.opposite` (the reverse side) to identify which side of a document an image belongs to.

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

- **`MRZScanResult.data` is now nullable**: The `data` property on `MRZScanResult` has been changed from a non-optional type to a nullable type (`nullable DSMRZData*` / `MRZData?` in Swift). Code that accesses `result.data` without a nil check must be updated. See the [upgrade guide](../user-guide/upgrade.md#mrzscannresultdata-is-now-nullable) for migration details.

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
