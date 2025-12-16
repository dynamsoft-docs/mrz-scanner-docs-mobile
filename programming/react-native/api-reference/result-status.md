---
layout: default-layout
title: EnumResultStatus - Dynamsoft MRZ Scanner React Native Edition
description: EnumResultStatus of DynamsoftMRZScanner React Native is an enumeration class that defines the result status of the MRZScanResult.
keywords: MRZScanner, MRZScanResult
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumResultStatus
---

# EnumResultStatus

`EnumResultStatus` is a enumeration class that defines the result status of the `MRZScanResult`.

## Definition

*Assembly:* dynamsoft-capture-vision-react-native

```ts
enum EnumResultStatus
{
    RS_FINISHED,
    RS_CANCELED,
    RS_EXCEPTION
}
```

## Members

| Member | Description |
| ------ | ----------- |
| `RS_FINISHED` | The MRZ scanning process was a success and the MRZ result has been received. |
| `RS_CANCELED` | The MRZ scanning process was cancelled by the user (usually by closing the UI using the close button). |
| `RS_EXCEPTION` | Something went wrong during the barcode decoding process and an exception has been thrown. |
