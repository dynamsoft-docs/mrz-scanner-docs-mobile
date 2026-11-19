---
layout: default-layout
title: MRZScanner API reference - Dynamsoft MRZ Scanner MAUI Edition
description: This is the main page of MRZScanner API Reference for MAUI Language.
keywords: MRZScanner, api reference, MAUI
---

# MRZScanner API References

`MRZScanner` is a ready-to-use component for scanning the MRZ on passports and ID cards. A single call to [`MRZScanner.Start`](mrz-scanner.md#start) opens the built-in scanner UI on Android and iOS and returns the result when the scanner closes.

New to the SDK? The [MRZ Scanner User Guide](../user-guide/index.md) builds a working scanner from an empty project, and [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) covers the configuration options in context.

## Classes

| Class | Description |
| ----- | ----------- |
| [`MRZScanner`](mrz-scanner.md) | The main class of the `MRZScanner` component. Its `Start` method opens the scanner and returns the result. |
| [`MRZScannerConfig`](mrz-scanner-config.md) | The class that provides MRZ scanning configurations. |
| [`MRZScanResult`](mrz-scan-result.md) | The MRZ scan result class. |
| [`MRZData`](mrz-data.md) | The class that contains the parsed MRZ information. |
| [`EnumResultStatus`](result-status.md) | An enumeration that describes the result status. |
| [`EnumDocumentType`](document-type.md) | An enumeration that defines the type of document to scan, such as ID cards or passports. |
| [`EnumDocumentSide`](document-side.md) | An enumeration that defines which side of a document is referenced when retrieving images from a scan result. |
| [`EnumErrorCode`](error-code.md) | An enumeration that defines the error codes owned by the MRZ Scanner bundle. |
