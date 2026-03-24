---
layout: default-layout
title: DSMRZScanResult Class - Dynamsoft MRZ Scanner iOS Edition
description: DSMRZScanResult of DynamsoftMRZScanner iOS is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: DSMRZScanResult
---

# DSMRZScanResult

`DSMRZScanResult` is a result class that contains the parsed MRZ information and the corresponding additional information.

## Definition

*Assembly:* DynamsoftMRZScanner.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@interface DSMRZScanResult : NSObject
```
2. 
```swift
class MRZScanResult : NSObject
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`data`](#data) | [*DSMRZData*](mrz-data.md) | The parsed MRZ information. |
| [`resultStatus`](#resultstatus) | *DSResultStatus* | The status of the result, which can be finished, canceled or exception. |
| [`errorCode`](#errorcode) | *NSInteger* | The error code should something go wrong during the MRZ scanning process. |
| [`errorString`](#errorstring) | *NSString \** | The error message associated with the error code should something go wrong during the MRZ scanning process. |

### data

The parsed MRZ information as a [`MRZData`](mrz-data.md) object.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, nullable, readonly) DSMRZData* data;
```
2.
```swift
var data: MRZData? {get}
```

### resultStatus

The status of the result represented as an [`EnumResultStatus`](result-status.md).

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, assign, readonly) DSResultStatus resultStatus;
```
2. 
```swift
var resultStatus: ResultStatus {get}
```

**Return Value**

The status of the result, which can be finished, canceled or exception.

- `DSResultStatusFinished`: The MRZ scanning is finished.
- `DSResultStatusCanceled`: The MRZ scanning activity is closed before the process is finished.
- `DSResultStatusException`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### errorCode

The error code should something go wrong during the MRZ scanning process.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, assign, readonly) NSInteger errorCode;
```
2. 
```swift
var errorCode: Int { get }
```

### errorString

The error message associated with the error code should something go wrong during the MRZ scanning process.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, assign, readonly) NSString * errorString;
```
2.
```swift
var errorString: String? { get }
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`getPortraitImage`](#getportraitimage) | Returns the captured portrait image. |
| [`getDocumentImage`](#getdocumentimage) | Returns the captured document image for the specified side. |
| [`getOriginalImage`](#getoriginalimage) | Returns the original frame image for the specified side. |

### getPortraitImage

Returns the captured portrait image, if available.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
- (nullable DSImageData *)getPortraitImage;
```
2.
```swift
func getPortraitImage() -> ImageData?
```

**Return Value**

An `ImageData` object containing the portrait image, or `nil` if no portrait was captured or `returnPortraitImage` was disabled in [`MRZScannerConfig`](mrz-scanner-config.md).

### getDocumentImage

Returns the captured document image for the specified side.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
- (nullable DSImageData *)getDocumentImage:(DSDocumentSide)side;
```
2.
```swift
func getDocumentImage(_ side: DocumentSide) -> ImageData?
```

**Parameters**

`side`: A [`DocumentSide`](document-side.md) value specifying which side of the document to retrieve. Use `.mrz` for the side containing the MRZ, or `.opposite` for the other side.

**Return Value**

An `ImageData` object containing the document image, or `nil` if the image was not captured or `returnDocumentImage` was disabled in [`MRZScannerConfig`](mrz-scanner-config.md).

### getOriginalImage

Returns the original full-frame image for the specified side.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
- (nullable DSImageData *)getOriginalImage:(DSDocumentSide)side;
```
2.
```swift
func getOriginalImage(_ side: DocumentSide) -> ImageData?
```

**Parameters**

`side`: A [`DocumentSide`](document-side.md) value specifying which side of the document to retrieve.

**Return Value**

An `ImageData` object containing the original frame image, or `nil` if the image was not captured or `returnOriginalImage` was disabled in [`MRZScannerConfig`](mrz-scanner-config.md).
