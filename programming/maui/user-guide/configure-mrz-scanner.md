---
layout: default-layout
title: Configure MRZ Scanner - Dynamsoft MRZ Scanner MAUI Edition
description: Configure the MRZ scan settings via MRZScannerConfig class when using MRZ Scanner MAUI Edition
keywords: Configure, config, MRZScannerConfig, MAUI
breadcrumbText: Configure MRZ Scanner
noTitleIndex: true
needGenerateH3Content: true
needAutoGenerateSidebar: true
---

# Configure MRZ Scanner

When developing with `MRZScanner` component, you can add configurations via the `MRZScannerConfig` class. This page will guide you on how to configure the settings.

## Scan Document Type

### Set the document type via APIs

Specifies the type of document to scan, such as ID cards or passports. It also improves the processing speed and the accuracy.

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.DocumentType = EnumDocumentType.Passport;
```

**Related APIs**

- [`DocumentType`]({{ site.maui_api }}mrz-scanner-config.html#documenttype)

### Setup a customized template file

A template file is a JSON file that includes a series of algorithm parameter settings. It is always used to customize the performance for different usage scenarios. [Contact us](https://www.dynamsoft.com/company/customer-service/#contact) to get a customized template for your scanner.

1. Put your JSON file in the **Resources\Raw** folder of your project.

2. Specify the template file via `TemplateFile`

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.TemplateFile = "CustomizedTemplate.json";
```

> [!NOTE] You can also use a JSON string as the template file.

**Related APIs**

- [`TemplateFile`]({{ site.maui_api }}mrz-scanner-config.html#templatefile)

## Configure the UI Elements

<div align="center">
    <p><img src="../../assets/mrz-scanner-ui.png" width="70%" alt="mrz-scanner"></p>
    <p>MRZ Scanner UI Component</p>
</div>

- Close button: Stop MRZ scanning and go back to the previous activity.
- Torch button: A clickable button that can turn on/off the torch.

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.IsTorchButtonVisible = true;
config.IsGuideFrameVisible = true;
config.IsCloseButtonVisible = true;
```

**Related APIs**

- [`IsTorchButtonVisible`]({{ site.maui_api }}mrz-scanner-config.html#istorchbuttonvisible)
- [`IsGuideFrameVisible`]({{ site.maui_api }}mrz-scanner-config.html#isguideframevisible)
- [`IsCloseButtonVisible`]({{ site.maui_api }}mrz-scanner-config.html#isclosebuttonvisible)

### Beep

Let the app to trigger a beep sound when MRZ is scanned successfully.

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.IsBeepEnabled = true;
```

**Related API**

- [`IsBeepEnabled`]({{ site.maui_api }}mrz-scanner-config.html#setbeepenabled)
