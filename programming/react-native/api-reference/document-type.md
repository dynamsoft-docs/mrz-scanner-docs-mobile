---
layout: default-layout
title: EnumDocumentType - Dynamsoft MRZ Scanner React Native Edition
description: EnumDocumentType of DynamsoftMRZScanner React Native is an enumeration class that defines the result status of the MRZScanResult.
keywords: document type, id cards, passports
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumDocumentType
---

# EnumDocumentType

`EnumDocumentType` is an enumeration class that defines the type of document to scan, such as ID cards or passports.

## Definition

*Assembly:* dynamsoft-capture-vision-react-native

```ts
export enum EnumDocumentType
{
    DT_ALL, // supports both ID cards (TD1 and TD2) and passports (TD3)
    DT_ID, // only supports ID cards (TD1 and TD2)
    DT_PASSPORT // only supports passports (TD3)
}
```

## Members

| Member | Description |
| ------ | ----------- |
| `DT_ALL` | Supports both ID cards (TD1 and TD2) and passports (TD3) |
| `DT_ID` | Only supports ID cards (TD1 and TD2) |
| `DT_PASSPORT` | Only supports passports (TD3) |

