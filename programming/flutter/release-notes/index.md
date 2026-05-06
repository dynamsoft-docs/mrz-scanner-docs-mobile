---
layout: default-layout
title: Flutter Release Notes v3.x - Dynamsoft MRZ Scanner
description: This is the release notes page of Dynamsoft MRZ Scanner for Flutter SDK v3.x.
keywords: release notes, flutter, version 3.x,
needAutoGenerateSidebar: true
needGenerateH3Content: false
noTitleIndex: true
---

# Dynamsoft MRZ Scanner Flutter SDK - Release Notes

## 3.4.1200 (04/02/2026)

### New

- **Image results**: `MRZScanResult` now returns captured images alongside the parsed MRZ data. Three types of images can be retrieved from the result:
  - **Document image**: A cropped and perspective-corrected image of the document, available via `mrzSideDocumentImage` and `oppositeSideDocumentImage`. Returned by default.
  - **Original image**: The raw full-frame camera image captured at the moment of scanning, available via `mrzSideOriginalImage` and `oppositeSideOriginalImage`. Disabled by default.
  - **Portrait image**: The detected portrait extracted from the document, available via `portraitImage`. Returned by default.
  - For two-sided ID cards, the MRZ side and opposite side images are available via separate properties on `MRZScanResult`.

- **Image return configuration**: Added three new `MRZScannerConfig` properties to control which images are included in the scan result:
  - `returnDocumentImage` (default: `true`)
  - `returnOriginalImage` (default: `false`)
  - `returnPortraitImage` (default: `true`)

- **Additional MRZ data fields**: `MRZData` now exposes the following additional parsed fields:
  - `issuingStateRaw` — the raw ICAO issuing state code as it appears in the MRZ, before conversion to a full country name.
  - `nationalityRaw` — the raw ICAO nationality code as it appears in the MRZ, before conversion to a full country name.

- **New UI button visibility controls**: Added three new `MRZScannerConfig` properties to control the visibility of toggle buttons in the scanning UI:
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

This is the introductory version of the MRZ Scanner (Flutter Edition) to include the newly designed and developed ready-to-use `MRZScanner` component.

### Highlighted Features

- Automatic detection and parsing of MRZs in passports and IDs
- Support for the following MRTD formats: TD3 (Passport), TD2 (ID), and TD1 (ID)
- Ready-to-use UI to simplify the development process
- Modular, view-based design for easy maintenance and customization
- Added support for 16kb page sizes for Android.

### Fixes & Improvements

- **License Validation Behavior**: Instead of stopping execution immediately on an invalid license module, the library now continues processing and returns results from modules with valid licenses. An error is still reported to indicate the license issue.
