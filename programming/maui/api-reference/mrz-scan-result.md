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

`MRZScanResult` is a result class that contains the parsed MRZ information and the corresponding additional information.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
class MRZScanResult
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`Data`](#data) | [*MRZData?*](mrz-data.md) | The parsed MRZ data. |
| [`ResultStatus`](#resultstatus) | [*EnumResultStatus*](result-status.md) | The status of the result, which can be finished, canceled or exception. |
| [`ErrorCode`](#errorcode) | *int* | The error code should something go wrong during the MRZ scanning process. |
| [`ErrorString`](#errorstring) | *string?* | The error message associated with the error code should something go wrong during the MRZ scanning process. |

### Data

The parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```csharp
MRZData? Data { get; }
```

### ResultStatus

The status of the result represented as an [`EnumResultStatus`](result-status.md).

```csharp
EnumResultStatus ResultStatus { get; }
```

- `Finished`: The MRZ scanning is finished.
- `Canceled`: The MRZ scanning activity is closed before the process is finished.
- `Exception`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### ErrorCode

The error code should something go wrong during the MRZ scanning process.

```csharp
int ErrorCode { get; }
```

### ErrorString

The error message associated with the error code should something go wrong during the MRZ scanning process.

```csharp
string? ErrorString { get; }
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`GetPortraitImage`](#getportraitimage) | Returns the detected portrait image. |
| [`GetDocumentImage`](#getdocumentimage) | Returns the cropped document image for the specified document side. |
| [`GetOriginalImage`](#getoriginalimage) | Returns the original full-frame image for the specified document side. |

### GetPortraitImage

Returns the detected portrait image extracted from the document.

```csharp
ImageData? GetPortraitImage();
```

**Return Value**

An `ImageData` object containing the portrait image, or `null` if not available. Call `.ToImageSource()` on the result to convert it to a MAUI `ImageSource`.

Returns `null` in the following cases:

- `ReturnPortraitImage` was set to `false` in [`MRZScannerConfig`](mrz-scanner-config.md).
- No portrait was detected in the document.

### GetDocumentImage

Returns the cropped document image for the specified side of the document.

```csharp
ImageData? GetDocumentImage(EnumDocumentSide documentSide);
```

**Parameter(s)**

`documentSide`: An [`EnumDocumentSide`](document-side.md) value specifying which side of the document to retrieve the image for.

**Return Value**

An `ImageData` object containing the cropped document image, or `null` if not available. Call `.ToImageSource()` on the result to convert it to a MAUI `ImageSource`.

Returns `null` in the following cases:

- `ReturnDocumentImage` was set to `false` in [`MRZScannerConfig`](mrz-scanner-config.md).
- `EnumDocumentSide.Opposite` was requested but the document is single-sided (e.g. passports).
- `EnumDocumentSide.Opposite` was requested but the opposite side could not be captured.

### GetOriginalImage

Returns the original full-frame camera image for the specified side of the document. Disabled by default — enable by setting `ReturnOriginalImage = true` in [`MRZScannerConfig`](mrz-scanner-config.md).

```csharp
ImageData? GetOriginalImage(EnumDocumentSide documentSide);
```

**Parameter(s)**

`documentSide`: An [`EnumDocumentSide`](document-side.md) value specifying which side of the document to retrieve the image for.

**Return Value**

An `ImageData` object containing the original full-frame image, or `null` if not available. Call `.ToImageSource()` on the result to convert it to a MAUI `ImageSource`.

Returns `null` in the following cases:

- `ReturnOriginalImage` was set to `false` in [`MRZScannerConfig`](mrz-scanner-config.md) (this is the default).
- `EnumDocumentSide.Opposite` was requested but the document is single-sided (e.g. passports).
- `EnumDocumentSide.Opposite` was requested but the opposite side could not be captured.
