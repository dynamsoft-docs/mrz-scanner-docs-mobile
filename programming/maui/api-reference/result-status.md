---
layout: default-layout
title: EnumResultStatus - Dynamsoft MRZ Scanner MAUI Edition
description: EnumResultStatus of DynamsoftMRZScanner MAUI is an enumeration that defines the result status of the MRZScanResult.
keywords: MRZScanner, MRZScanResult, result status
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumResultStatus
---

# EnumResultStatus

`EnumResultStatus` is an enumeration that defines the result status of an [`MRZScanResult`](mrz-scan-result.md).

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public enum EnumResultStatus
{
    Finished = 0,
    Canceled = 1,
    Exception = 2
}
```

## Members

| Member | Value | Description |
| ------ | ----- | ----------- |
| `Finished` | 0 | The scan finished. The parsed fields are in [`Data`](mrz-scan-result.md#data). |
| `Canceled` | 1 | The user closed the scanner before a scan finished. |
| `Exception` | 2 | The scanner failed to start or failed during the scan. See [`ErrorCode`](mrz-scan-result.md#errorcode) and [`ErrorString`](mrz-scan-result.md#errorstring). |
