---
layout: default-layout
title: EnumDocumentSide - Dynamsoft MRZ Scanner Android Edition
description: EnumDocumentSide of DynamsoftMRZScanner Android is an enumeration class that defines the side of a document when retrieving scanned images.
keywords: document side, MRZ, portrait, image
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumDocumentSide
---

# EnumDocumentSide

`EnumDocumentSide` is an enumeration class that defines the side of a document when retrieving scanned images from a [`MRZScanResult`](mrz-scan-result.md).

## Definition

*Assembly:* MRZScannerBundle.aar

*Namespace:* com.dynamsoft.mrzscannerbundle.ui

```java
public enum EnumDocumentSide {
    DS_MRZ,
    DS_OPPOSITE
}
```

## Members

| Member | Description |
| ------ | ----------- |
| `DS_MRZ` | The side of the document that contains the MRZ. |
| `DS_OPPOSITE` | The opposite side of the document, which does not contain the MRZ. |
