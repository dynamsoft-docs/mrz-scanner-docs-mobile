---
layout: default-layout
title: MRZScanner Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZScanner of DynamsoftMRZScanner MAUI is the class that implements MRZ scanning features.
keywords: MRZ, scanner, activity
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanner
---

# MRZScanner

`MRZScanner` is the class that provides the ready-to-use MRZ scanning component.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
class MRZScanner
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`Start`](#start) | Launches the MRZ scanner and returns the scan result. |

### Start

Launches the MRZ scanning UI with the provided configuration and returns the result once the scanner closes.

```csharp
static async Task<MRZScanResult> Start(MRZScannerConfig config);
```

**Parameter(s)**

`config`: An [`MRZScannerConfig`](mrz-scanner-config.md) object that defines the scanner settings. The `License` property must be set.

**Return Value**

An [`MRZScanResult`](mrz-scan-result.md) containing the scan outcome. Check `ResultStatus` to determine whether the scan finished successfully, was canceled, or encountered an error.

**Usage Example**

```csharp
using Dynamsoft.MRZScannerBundle.Maui;

var config = new MRZScannerConfig("YOUR-LICENSE-KEY");
var result = await MRZScanner.Start(config);

if (result.ResultStatus == EnumResultStatus.Finished && result.Data is not null)
{
    // Use result.Data to access MRZ fields
}
else if (result.ResultStatus == EnumResultStatus.Canceled)
{
    // User closed the scanner
}
else
{
    // Handle error via result.ErrorString
}
```
