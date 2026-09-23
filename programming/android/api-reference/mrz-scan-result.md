---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner Android Edition
description: MRZScanResult of DynamsoftMRZScanner Android is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is a result class that contains the parsed MRZ information and the corresponding additional information.

## Definition

*Assembly:* MRZScannerBundle.aar

*Namespace:* com.dynamsoft.mrzscannerbundle.ui

```java
class MRZScanResult
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`getData`](#getdata) | Returns the parsed MRZ data. |
| [`getResultStatus`](#getresultstatus) | Returns the result status, which can be finished, canceled or exception. |
| [`getErrorCode`](#geterrorcode) | Returns the error code should something go wrong during the MRZ scanning process. |
| [`getErrorString`](#geterrorstring) | Returns the error message associated with the error code should something go wrong during the MRZ scanning process. |
| [`getDocumentImage`](#getdocumentimage) | Returns the cropped document image for the specified document side. |
| [`getOriginalImage`](#getoriginalimage) | Returns the original full-frame image for the specified document side. |
| [`getPortraitImage`](#getportraitimage) | Returns the detected portrait image. |

### getData

Returns the parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```java
MRZData getData();
```

**Return Value**

A [`MRZData`](mrz-data.md) object that contains the parsed MRZ data.

### getResultStatus

Returns the status of the result, which can be finished, canceled or exception.

```java
@EnumResultStatus
int getResultStatus();
```

**Return Value**

The status of the result, which can be finished, canceled or exception.

- `RS_FINISHED`: The MRZ scanning is finished.
- `RS_CANCELED`: The MRZ scanning activity is closed before the process is finished.
- `RS_EXCEPTION`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### getErrorCode

Returns the error code should something go wrong during the MRZ scanning process. Only meaningful when [`getResultStatus`](#getresultstatus) returns `RS_EXCEPTION`.

```java
int getErrorCode();
```

**Return Value**

An integer error code, drawn from one of two spaces:

- Values **greater than 0** belong to the MRZ Scanner bundle's own [`EnumErrorCode`](error-code.md), which claims the range 1000 to 1999.
- Values **less than or equal to 0** come from Dynamsoft Capture Vision and are not listed in [`EnumErrorCode`](error-code.md).

The two spaces cannot collide, so the sign alone tells you which one a given value belongs to.

The bundle currently defines two codes of its own, both concerning camera access: `EC_CAMERA_PERMISSION_DENIED` (1001) and `EC_CAMERA_PERMISSION_RESTRICTED` (1002). See [`EnumErrorCode`](error-code.md) for how they differ and which one is worth acting on.

### getErrorString

Returns the error message associated with the error code should something go wrong during the MRZ scanning process.

```java
String getErrorString();
```

**Return Value**

A string representing the message of a `EnumErrorCode`.

### getDocumentImage

Returns the cropped document image for the specified side of the document.

```java
@Nullable
ImageData getDocumentImage(EnumDocumentSide documentSide);
```

**Parameter(s)**

`documentSide`: An [`EnumDocumentSide`](document-side.md) value specifying which side of the document to retrieve the image for.

**Return Value**

An `ImageData` object containing the cropped document image, or `null` if not available.

The return value will be `null` in the following cases:

- `setReturnDocumentImage(false)` was set in the configuration.
- `EnumDocumentSide.DS_OPPOSITE` was requested but the document is single-sided (e.g. passports).
- `EnumDocumentSide.DS_OPPOSITE` was requested but the opposite side could not be captured, for example due to a capture failure or because the user exited the scanning process before the second side was scanned.

### getOriginalImage

Returns the original full-frame camera image for the specified side of the document. Disabled by default — enable by calling `setReturnOriginalImage(true)` in the configuration.

```java
@Nullable
ImageData getOriginalImage(EnumDocumentSide documentSide);
```

**Parameter(s)**

`documentSide`: An [`EnumDocumentSide`](document-side.md) value specifying which side of the document to retrieve the image for.

**Return Value**

An `ImageData` object containing the original full-frame image, or `null` if not available.

The return value will be `null` in the following cases:

- `setReturnOriginalImage(false)` was set in the configuration (this is the default).
- `EnumDocumentSide.DS_OPPOSITE` was requested but the document is single-sided (e.g. passports).
- `EnumDocumentSide.DS_OPPOSITE` was requested but the opposite side could not be captured, for example due to a capture failure or because the user exited the scanning process before the second side was scanned.

### getPortraitImage

Returns the detected portrait image extracted from the document. Returns `null` if the image is not available, which may happen if `setReturnPortraitImage(false)` was set in the configuration, or if no portrait was detected.

```java
@Nullable
ImageData getPortraitImage();
```

**Return Value**

An `ImageData` object containing the portrait image, or `null` if not available.
