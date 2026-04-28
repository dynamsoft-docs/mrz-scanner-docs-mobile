---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner React Native Edition
description: MRZScanResult of DynamsoftMRZScanner React Native is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is the most basic class to represent the full MRZ result object. It comes with a result status and the parsed MRZ info as a MRZData object.

## Definition

*Assembly:* dynamsoft-mrz-scanner-bundle-react-native

```ts
interface MRZScanResult
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`data`](#data) | [*MRZData*](mrz-data.md) | Represents the parsed MRZ data. |
| [`resultStatus`](#resultstatus) | [*EnumResultStatus*](result-status.md) | Represents the status of the result, which can be finished, canceled or exception. |
| [`errorCode`](#errorcode) | *number* | Represents the error code should something go wrong during the MRZ scanning process. |
| [`errorString`](#errorstring) | *string* | Represents the error message associated with the error code should something go wrong during the MRZ scanning process. |
| [`portraitImage`](#portraitimage) | *ImageSourcePropType* | The extracted portrait photo from the document. |
| [`mrzSideDocumentImage`](#mrzsidedocumentimage) | *ImageSourcePropType* | The cropped, perspective-corrected image of the MRZ side of the document. |
| [`oppositeSideDocumentImage`](#oppositesidedocumentimage) | *ImageSourcePropType* | The cropped, perspective-corrected image of the opposite (non-MRZ) side of the document. |
| [`mrzSideOriginalImage`](#mrzsideoriginalimage) | *ImageSourcePropType* | The full camera frame captured for the MRZ side. |
| [`oppositeSideOriginalImage`](#oppositesideoriginalimage) | *ImageSourcePropType* | The full camera frame captured for the opposite side. |

### data

Represents the parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```ts
data?: MRZData
```

### resultStatus

Represents the status of the result, which can be finished, canceled or exception.

```ts
resultStatus: EnumResultStatus
```

**Remarks**

The result status can be one of three things:

- `RS_FINISHED`: The MRZ scanning is finished.
- `RS_CANCELED`: The MRZ scanning activity is closed before the process is finished.
- `RS_EXCEPTION`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### errorCode

Returns the error code when an exception occurs. This value is only valid when resultStatus is `RS_EXCEPTION`.

```ts
errorCode?: number
```

### errorString

Returns the error message associated with the error code when an exception occurs. This value is only valid when resultStatus is `RS_EXCEPTION`.

```ts
errorString?: string
```

### portraitImage

Returns the extracted portrait photo from the document as an `ImageSourcePropType` — it can be passed directly to a React Native `<Image>` element via its `source` prop. Returns `undefined` if portrait capture was disabled via `MRZScanConfig.returnPortraitImage` or if no portrait was detected.

```ts
portraitImage?: ImageSourcePropType
```

### mrzSideDocumentImage

Returns the cropped, perspective-corrected image of the side containing the MRZ as an `ImageSourcePropType`. Returns `undefined` if document image capture was disabled via `MRZScanConfig.returnDocumentImage`.

```ts
mrzSideDocumentImage?: ImageSourcePropType
```

### oppositeSideDocumentImage

Returns the cropped, perspective-corrected image of the opposite (non-MRZ) side of the document as an `ImageSourcePropType`. Relevant for two-sided documents such as TD1 ID cards. Returns `undefined` if document image capture was disabled via `MRZScanConfig.returnDocumentImage` or if no opposite side was captured.

```ts
oppositeSideDocumentImage?: ImageSourcePropType
```

### mrzSideOriginalImage

Returns the full camera frame captured for the MRZ side as an `ImageSourcePropType`. Returns `undefined` if original image capture was not enabled via `MRZScanConfig.returnOriginalImage`.

```ts
mrzSideOriginalImage?: ImageSourcePropType
```

### oppositeSideOriginalImage

Returns the full camera frame captured for the opposite side as an `ImageSourcePropType`. Returns `undefined` if original image capture was not enabled via `MRZScanConfig.returnOriginalImage` or if no opposite side was captured.

```ts
oppositeSideOriginalImage?: ImageSourcePropType
```
