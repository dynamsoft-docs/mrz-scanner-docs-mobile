---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner Flutter Edition
description: MRZScanResult of DynamsoftMRZScanner Flutter is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is the most basic class to represent the full MRZ result object. It comes with a result status and the parsed MRZ info as a MRZData object.

## Definition

*Assembly:* dynamsoft_mrz_scanner_bundle_flutter

```dart
class MRZScanResult
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`mrzData`](#mrzdata) | [*MRZData?*](mrz-data.md) | Represents the parsed MRZ data. |
| [`status`](#status) | [*EnumResultStatus*](result-status.md) | Represents the status of the result, which can be finished, canceled or exception. |
| [`errorCode`](#errorcode) | *int?* | Represents the error code should something go wrong during the MRZ scanning process. |
| [`errorMessage`](#errormessage) | *String?* | Represents the error message associated with the error code should something go wrong during the MRZ scanning process. |
| [`portraitImage`](#portraitimage) | *Uint8List?* | The extracted portrait photo from the document. |
| [`mrzSideDocumentImage`](#mrzsidedocumentimage) | *Uint8List?* | The cropped, perspective-corrected image of the MRZ side of the document. |
| [`oppositeSideDocumentImage`](#oppositesidedocumentimage) | *Uint8List?* | The cropped, perspective-corrected image of the opposite (non-MRZ) side of the document. |
| [`mrzSideOriginalImage`](#mrzsideoriginalimage) | *Uint8List?* | The full camera frame captured for the MRZ side. |
| [`oppositeSideOriginalImage`](#oppositesideoriginalimage) | *Uint8List?* | The full camera frame captured for the opposite side. |

### mrzData

Represents the parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```dart
MRZData? mrzData;
```

### status

Represents the status of the result, which can be finished, canceled or exception. The status comes in the form of a [`EnumResultStatus`](result-status.md).

```dart
EnumResultStatus status;
```

**Remarks**

The result status can be one of three things:

- `finished`: The MRZ scanning is finished.
- `canceled`: The MRZ scanning activity is closed before the process is finished.
- `exception`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### errorCode

Returns the error code when an exception occurs. This value is only valid when resultStatus is `exception`.

```dart
int? errorCode;
```

### errorMessage

Returns the error message associated with the error code when an exception occurs. This value is only valid when resultStatus is `exception`.

```dart
String? errorMessage;
```

### portraitImage

Returns the extracted portrait photo as a raw image byte array. Returns `null` if portrait capture was disabled via `MRZScannerConfig.returnPortraitImage` or if no portrait was detected.

```dart
Uint8List? portraitImage;
```

### mrzSideDocumentImage

Returns the cropped, perspective-corrected image of the side containing the MRZ as a raw image byte array. Returns `null` if document image capture was disabled via `MRZScannerConfig.returnDocumentImage`.

```dart
Uint8List? mrzSideDocumentImage;
```

### oppositeSideDocumentImage

Returns the cropped, perspective-corrected image of the opposite (non-MRZ) side of the document as a raw image byte array. Relevant for two-sided documents such as TD1 ID cards. Returns `null` if document image capture was disabled via `MRZScannerConfig.returnDocumentImage` or if no opposite side was captured.

```dart
Uint8List? oppositeSideDocumentImage;
```

### mrzSideOriginalImage

Returns the full camera frame captured for the MRZ side as a raw image byte array. Returns `null` if original image capture was not enabled via `MRZScannerConfig.returnOriginalImage`.

```dart
Uint8List? mrzSideOriginalImage;
```

### oppositeSideOriginalImage

Returns the full camera frame captured for the opposite side as a raw image byte array. Returns `null` if original image capture was not enabled via `MRZScannerConfig.returnOriginalImage` or if no opposite side was captured.

```dart
Uint8List? oppositeSideOriginalImage;
```
