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

`DSMRZScanResult` is a result class that contains the parsed MRZ information from one scan and the additional information.

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

The parsed MRZ information.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, readonly) DSMRZData* data;
```
2. 
```swift
var data: MRZData {get}
```

### resultStatus

The status of the result, which can be finished, canceled or exception.

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

- `RS_FINISHED`: The MRZ scanning is finished.
- `RS_CANCELED`: The MRZ scanning activity is closed before the process is finished.
- `RS_EXCEPTION`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

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

**Return Value**

An integer representing a `DSErrorCode`.

### errorString

The error message associated with the error code should something go wrong during the MRZ scanning process.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, assign, readonly) NSString * errorMessage;
```
2. 
```swift
var errorMessage: String? { get }
```
