---
layout: default-layout
title: EnumResultStatus - Dynamsoft MRZ Scanner iOS Edition
description: EnumResultStatus of DynamsoftMRZScanner iOS is an enumeration class that defines the result status of the MRZScanResult.
keywords: scanner, activity, startCapturing, license 
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumResultStatus
---

# EnumResultStatus

`EnumResultStatus` is a enumeration class that defines the result status of the `MRZScanResult`.

## Definition

*Assembly:* DynamsoftMRZScanner.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
typedef NS_ENUM(NSInteger, DSResultStatus)
{
    DSResultStatusFinished = 0
    DSResultStatusCanceled = 1
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
