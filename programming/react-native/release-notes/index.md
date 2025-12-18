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
