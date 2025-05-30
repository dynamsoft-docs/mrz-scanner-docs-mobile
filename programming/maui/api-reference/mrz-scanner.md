---
layout: default-layout
title: MRZScanner Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZScanner of DynamsoftMRZScanner MAUI is an activity class that implements MRZ scanning features.
keywords: MRZ, scanner, activity
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanner
---

# Class MRZScanner

`MRZScanner` is an activity class that implements MRZ scanning features.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
class MRZScanner
```

## Start

Starts the MRZ scanning process.

```csharp
static async Task<MRZScanResult> Start(MRZScannerConfig config);
```

## How to Use

```csharp
using System.Collections.ObjectModel;
using Dynamsoft.MRZScannerBundle.Maui;

namespace ScanMRZ;

public partial class MainPage : ContentPage
{
    private async void OnScanMRZ(object sender, EventArgs e)
    {
        // The string "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9" here grants a time-limited free trial which requires network connection to work.
        // You can request a 30-day trial license via the Request a Trial License page https://www.dynamsoft.com/customer/license/trialLicense?product=dbr&utm_source=guide&package=maui.
        var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
        var result = await MRZScanner.Start(config);
    }
}
```
