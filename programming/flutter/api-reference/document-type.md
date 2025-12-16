---
layout: default-layout
title: EnumDocumentType - Dynamsoft MRZ Scanner Flutter Edition
description: EnumDocumentType of DynamsoftMRZScanner Flutter is an enumeration class that defines the result status of the MRZScanResult.
keywords: document type, id cards, passports
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumDocumentType
---

# EnumDocumentType

`EnumDocumentType` is an enumeration class that defines the type of document to scan, such as ID cards or passports.

## Definition

*Assembly:* dynamsoft_mrz_scanner_bundle_flutter

```dart
public enum EnumDocumentType
{
    all, // supports both ID cards (TD1 and TD2) and passports (TD3)
    id, // only supports ID cards (TD1 and TD2)
    passport // only supports passports (TD3)
}
```

## Members

| Member | Description |
| ------ | ----------- |
| `all` | Supports both ID cards (TD1 and TD2) and passports (TD3) |
| `id` | Only supports ID cards (TD1 and TD2) |
| `passport` | Only supports passports (TD3) |

