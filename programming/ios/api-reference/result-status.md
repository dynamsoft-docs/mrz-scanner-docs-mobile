---
layout: default-layout
title: DSResultStatus - Dynamsoft MRZ Scanner iOS Edition
description: DSResultStatus of DynamsoftMRZScanner iOS is an enumeration class that defines the result status of the MRZScanResult.
keywords: scanner, activity, startCapturing, license 
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: DSResultStatus
---

# DSResultStatus

`DSResultStatus` is an enumeration that defines the result status of the `MRZScanResult`.

## Definition

*Assembly:* DynamsoftMRZScannerBundle.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
typedef NS_ENUM(NSInteger, DSResultStatus)
{
        DSResultStatusFinished = 0,
        DSResultStatusCanceled = 1,
        DSResultStatusException = 2
};
```
2. 
```swift
@objc public enum ResultStatus: Int {
        case finished = 0
        case canceled = 1
        case exception = 2
}
```
