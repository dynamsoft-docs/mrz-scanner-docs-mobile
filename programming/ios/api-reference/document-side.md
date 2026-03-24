---
layout: default-layout
title: DSDocumentSide - Dynamsoft MRZ Scanner iOS Edition
description: DSDocumentSide of DynamsoftMRZScanner iOS is an enumeration that defines which side of a document is referenced when retrieving images from a scan result.
keywords: document side, mrz, portrait
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: DSDocumentSide
---

# DSDocumentSide

`DSDocumentSide` is an enumeration that defines which side of a document is referenced when retrieving images from an [`MRZScanResult`](mrz-scan-result.md).

## Definition

*Assembly:* DynamsoftMRZScanner.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
typedef NS_ENUM(NSInteger, DSDocumentSide)
{
    DSDocumentSideMRZ = 0,
    DSDocumentSideOpposite = 1
};
```
2.
```swift
@objc public enum DocumentSide: Int {
    case mrz = 0
    case opposite = 1
}
```

## Members

| Member | Value | Description |
| ------ | ----- | ----------- |
| `DSDocumentSideMRZ` / `mrz` | 0 | The side of the document that contains the MRZ. |
| `DSDocumentSideOpposite` / `opposite` | 1 | The other side of the document. Only applicable when the MRZ and the portrait are on opposite sides (e.g., certain ID cards). |
