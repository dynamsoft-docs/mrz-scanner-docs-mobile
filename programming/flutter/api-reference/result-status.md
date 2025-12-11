---
layout: default-layout
title: EnumResultStatus - Dynamsoft MRZ Scanner Flutter Edition
description: EnumResultStatus of DynamsoftMRZScanner Flutter is an enumeration class that defines the result status of the MRZScanResult.
keywords: MRZScanner, MRZScanResult
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumResultStatus
---

# EnumResultStatus

`EnumResultStatus` is a enumeration class that defines the result status of the `MRZScanResult`.

## Definition

*Assembly:* dynamsoft_mrz_scanner_bundle_flutter

```dart
enum EnumResultStatus
{
    finished,
    canceled,
    exception
}
```

## Members

| Member | Description |
| ------ | ----------- |
| `finished` | The MRZ scanning process was a success and the MRZ result has been received. |
| `canceled` | The MRZ scanning process was cancelled by the user (usually by closing the UI using the close button). |
| `exception` | Something went wrong during the barcode decoding process and an exception has been thrown. |
