---
layout: default-layout
title: DSDocumentType - Dynamsoft MRZ Scanner iOS Edition
description: DSDocumentType of DynamsoftMRZScanner iOS is an enumeration class that defines the result status of the MRZScanResult.
keywords: document type, id cards, passports
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: DSDocumentType
---

# DSDocumentType

`DSDocumentType` is an enumeration class that defines the type of document to scan, such as ID cards or passports.

## Definition

*Assembly:* DynamsoftMRZScanner.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
typedef NS_ENUM(NSInteger, DSDocumentType)
{
    DSDocumentTypeAll = 0
    DSDocumentTypeId= 1
    DSDocumentTypePassport = 2
};
```
2. 
```swift
@objc public enum DocumentType: Int {
    case all = 0
    case id = 1
    case passport = 2
}
```
