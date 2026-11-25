---
layout: default-layout
title: MRZScanner Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZScanner of DynamsoftMRZScanner MAUI is the class that implements MRZ scanning features.
keywords: MRZ, scanner, start
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanner
---

# MRZScanner

`MRZScanner` is the class that provides the ready-to-use MRZ scanning component. Its static [`Start`](#start) method opens the scanner and returns the result.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public class MRZScanner
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`Start`](#start) | Opens the MRZ scanner and returns the scan result when the scan ends. |

### Start

Opens the MRZ scanner full screen with the provided configuration and returns the result when the scan ends.

```csharp
public static Task<MRZScanResult> Start(MRZScannerConfig config);
```

**Parameter(s)**

`config`: An [`MRZScannerConfig`](mrz-scanner-config.md) object that defines the scanner settings. Its [`License`](mrz-scanner-config.md#license) must be set.

**Return Value**

A task that completes with an [`MRZScanResult`](mrz-scan-result.md). Check its [`ResultStatus`](mrz-scan-result.md#resultstatus) to tell whether the scan finished, was canceled, or failed. Scanning errors are reported through the result rather than thrown.

**Remarks**

- Only one scanner is shown at a time. Calling `Start` while a scan is running, for example from a double tap, returns the running scan's task.
- On iOS, the scanner can't open while another page is still animating, for example right after a `PopModalAsync` that wasn't awaited. `Start` then returns an `Exception` result. See [Starting the Scanner from a Modal Page](../user-guide/index.md#starting-the-scanner-from-a-modal-page).
- On Android, the system can recreate the app's page while the scanner is open. `Start` still completes, so keep the result outside the page. See [Results and Image Lifetime](../user-guide/index.md#results-and-image-lifetime).

## How to Use

```csharp
using Dynamsoft.MRZScannerBundle.Maui;

var config = new MRZScannerConfig("YOUR-LICENSE-KEY");
var result = await MRZScanner.Start(config);

switch (result.ResultStatus)
{
    case EnumResultStatus.Finished when result.Data is { } data:
        // Use the parsed fields, for example data.DocumentNumber.
        break;
    case EnumResultStatus.Canceled:
        // The user closed the scanner.
        break;
    case EnumResultStatus.Exception:
        // result.ErrorCode and result.ErrorString describe the failure.
        break;
}
```
