---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZScanResult of DynamsoftMRZScanner MAUI is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is a result class that contains the parsed MRZ information, the captured images and the result status. [`MRZScanner.Start`](mrz-scanner.md#start) returns one for every scan.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public class MRZScanResult
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`Data`](#data) | [*MRZData?*](mrz-data.md) | The parsed MRZ data. |
| [`ResultStatus`](#resultstatus) | [*EnumResultStatus*](result-status.md) | The status of the result: finished, canceled or exception. |
| [`ErrorCode`](#errorcode) | *int* | The error code when the scan fails. |
| [`ErrorString`](#errorstring) | *string?* | The error message when the scan fails. |

### Data

The parsed MRZ information as an [`MRZData`](mrz-data.md) object. `null` unless the scan finished with a result.

```csharp
MRZData? Data { get; }
```

### ResultStatus

The status of the result as an [`EnumResultStatus`](result-status.md).

```csharp
EnumResultStatus ResultStatus { get; }
```

- `Finished`: The scan finished.
- `Canceled`: The user closed the scanner before a scan finished.
- `Exception`: The scanner failed to start or failed during the scan.

### ErrorCode

The error code when `ResultStatus` is `Exception`.

```csharp
int ErrorCode { get; }
```

**Remarks**

- **Greater than `0`:** an [`EnumErrorCode`](error-code.md) value (range `1000`–`1999`), such as a denied camera permission.
- **Less than `0`:** an error from Dynamsoft Capture Vision, such as an invalid license or template.
- **`0`:** an error from the MAUI layer, which has no native code. See [Errors reported by the MAUI layer](error-code.md#errors-reported-by-the-maui-layer).

### ErrorString

The error message when `ResultStatus` is `Exception`.

```csharp
string? ErrorString { get; }
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`GetPortraitImage`](#getportraitimage) | Returns the detected portrait image. |
| [`GetDocumentImage`](#getdocumentimage) | Returns the cropped document image for the specified document side. |
| [`GetOriginalImage`](#getoriginalimage) | Returns the original full-frame image for the specified document side. |

Each method returns an [`ImageData`](https://www.dynamsoft.com/capture-vision/docs/mobile/programming/maui/api-reference/core/image-data.html){:target="_blank"} (from the `Dynamsoft.Core.Maui` namespace). Call `ToImageSource()` to show it in a MAUI `Image`. The conversion copies and encodes the image on the calling thread, so run it off the UI thread for large images such as the originals.

### GetPortraitImage

Returns the detected portrait image extracted from the document.

```csharp
ImageData? GetPortraitImage();
```

**Return Value**

The portrait image, or `null` if [`ReturnPortraitImage`](mrz-scanner-config.md#returnportraitimage) is `false` or no portrait was detected.

### GetDocumentImage

Returns the cropped document image for the specified side of the document.

```csharp
ImageData? GetDocumentImage(EnumDocumentSide side);
```

**Parameter(s)**

`side`: An [`EnumDocumentSide`](document-side.md) value specifying which side of the document to retrieve.

**Return Value**

The cropped document image, or `null` in the following cases:

- [`ReturnDocumentImage`](mrz-scanner-config.md#returndocumentimage) is `false`.
- `EnumDocumentSide.Opposite` was requested but the document is single-sided (e.g. passports).
- `EnumDocumentSide.Opposite` was requested but the opposite side wasn't captured, for example because the user finished before flipping the document.

### GetOriginalImage

Returns the original full-frame camera image for the specified side of the document.

```csharp
ImageData? GetOriginalImage(EnumDocumentSide side);
```

**Parameter(s)**

`side`: An [`EnumDocumentSide`](document-side.md) value specifying which side of the document to retrieve.

**Return Value**

The original image, or `null` in the following cases:

- [`ReturnOriginalImage`](mrz-scanner-config.md#returnoriginalimage) is `false` (the default).
- `EnumDocumentSide.Opposite` was requested but the document is single-sided (e.g. passports).
- `EnumDocumentSide.Opposite` was requested but the opposite side wasn't captured.
