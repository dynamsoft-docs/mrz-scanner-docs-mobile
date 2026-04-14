---
layout: default-layout
title: EnumDocumentSide - Dynamsoft MRZ Scanner MAUI Edition
description: EnumDocumentSide of DynamsoftMRZScanner MAUI is an enumeration that defines which side of a document is referenced when retrieving images from a scan result.
keywords: document side, mrz, portrait
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumDocumentSide
---

# EnumDocumentSide

`EnumDocumentSide` is an enumeration that defines which side of a document is referenced when retrieving images from an [`MRZScanResult`](mrz-scan-result.md).

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public enum EnumDocumentSide
{
    MRZ = 0,
    Opposite = 1
}
```

## Members

| Member | Value | Description |
| ------ | ----- | ----------- |
| `MRZ` | 0 | The side of the document that contains the MRZ. |
| `Opposite` | 1 | The other side of the document. Only applicable when the MRZ and the portrait are on opposite sides (e.g., certain ID cards). |
