---
layout: default-layout
title: React Native Release Notes v3.x - Dynamsoft MRZ Scanner
description: This is the release notes page of Dynamsoft MRZ Scanner for React Native SDK v3.x.
keywords: release notes, MAUI, version 3.x,
needAutoGenerateSidebar: true
needGenerateH3Content: false
noTitleIndex: true
---

# Dynamsoft MRZ Scanner React Native SDK - Release Notes

## 3.4.1300 (04/28/2026)

### New

- **Image results**: `MRZScanResult` now returns captured images alongside the parsed MRZ data. Five new properties are exposed on the result, each typed as `ImageSourcePropType` so it can be passed directly to a React Native `<Image>` element via its `source` prop:
  - `mrzSideDocumentImage` — a cropped and perspective-corrected image of the side containing the MRZ. Returned by default.
  - `oppositeSideDocumentImage` — a cropped and perspective-corrected image of the reverse side, relevant for two-sided documents such as TD1 ID cards. Returned by default.
  - `mrzSideOriginalImage` — the raw full-frame camera image captured for the MRZ side. Disabled by default.
  - `oppositeSideOriginalImage` — the raw full-frame camera image captured for the opposite side. Disabled by default.
  - `portraitImage` — the detected portrait extracted from the document. Returned by default.

- **Image return configuration**: Added three new `MRZScanConfig` properties to control which images are included in the scan result:
  - `returnDocumentImage` (default: `true`)
  - `returnOriginalImage` (default: `false`)
  - `returnPortraitImage` (default: `true`)

- **Additional MRZ data fields**: `MRZData` now exposes the following additional parsed fields:
  - `issuingStateRaw` — the raw ICAO issuing state code as encoded in the MRZ, before standardization.
  - `nationalityRaw` — the raw ICAO nationality code as encoded in the MRZ, before standardization.
  - `optionalData1` — the first optional data field on the MRZ document.
  - `optionalData2` — the second optional data field on the MRZ document.
  - `personalNumber` — the personal number field, typically present on TD1 and TD3 documents.

- **New UI button visibility controls**: Added three new `MRZScanConfig` properties to control the visibility of toggle buttons in the scanning UI:
  - `isBeepButtonVisible` — shows or hides the beep sound toggle button (default: `true`).
  - `isVibrateButtonVisible` — shows or hides the vibration toggle button (default: `true`).
  - `isFormatSelectorVisible` — shows or hides the document format selector at the bottom of the scanning UI (default: `true`).

## 3.2.5000 (12/18/2025)

### Fixes & Improvements

- Resolved a libPNG CVE issue by updating the C++ dependencies to use the latest libPNG CVE.

## 3.2.3000 (11/06/2025)

### Highlighted Features

- Updated the underlying Capture Vision bundle to 3.2.3000 for major improvements in reading accuracy and speed.
- Neural MRZ Localization – The new MRZLocalization model improves region detection accuracy and delivers up to **42.7% faster processing** for MRZ-based document workflows.
- Configurable Localization Control – The new LocalizationModes parameter allows configuration for text line detection.

## 3.0.5200 (08/28/2025)

This is the introductory version of the MRZ Scanner (React Native Edition) to include the newly designed and developed ready-to-use `MRZScanner` component.

### Highlighted Features

- Automatic detection and parsing of MRZs in passports and IDs
- Support for the following MRTD formats: TD3 (Passport), TD2 (ID), and TD1 (ID)
- Ready-to-use UI to simplify the development process
- Modular, view-based design for easy maintenance and customization
- Added support for 16kb page sizes for Android.

### Fixes & Improvements

- **License Validation Behavior**: Instead of stopping execution immediately on an invalid license module, the library now continues processing and returns results from modules with valid licenses. An error is still reported to indicate the license issue.
