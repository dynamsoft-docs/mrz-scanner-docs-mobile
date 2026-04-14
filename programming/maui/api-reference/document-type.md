---
layout: default-layout
title: EnumDocumentType - Dynamsoft MRZ Scanner MAUI Edition
description: EnumDocumentType of DynamsoftMRZScanner MAUI is an enumeration that defines the type of document to scan, such as ID cards or passports.
keywords: document type, id cards, passports
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumDocumentType
---

# EnumDocumentType

`EnumDocumentType` is an enumeration that defines the type of document to scan, such as ID cards or passports.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public enum EnumDocumentType
{
    All = 0,
    Id = 1,
    Passport = 2
}
```

## Members

| Member | Value | Description |
| ------ | ----- | ----------- |
| `All` | 0 | The scanner recognizes all supported MRZ document types (TD1, TD2, TD3). |
| `Id` | 1 | The scanner only recognizes ID card formats (TD1 and TD2). |
| `Passport` | 2 | The scanner only recognizes passport format (TD3). |
