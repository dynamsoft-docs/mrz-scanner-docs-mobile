---
layout: default-layout
title: Customizing the MRZ Scanner - Dynamsoft MRZ Scanner MAUI Edition
description: Customize the MRZ scan settings via MRZScannerConfig class when using MRZ Scanner MAUI Edition
keywords: Configure, config, MRZScannerConfig, MAUI, customize
breadcrumbText: Customize MRZ Scanner
noTitleIndex: true
needGenerateH3Content: true
needAutoGenerateSidebar: true
---

# Customizing the MRZ Scanner

When developing with `MRZScanner`, you can add configurations via the `MRZScannerConfig` class. This page will guide you on how to configure the settings.

## MRZScannerConfig Overview

The [**`MRZScannerConfig`**](../api-reference/mrz-scanner-config.md) class is capable of configuring almost all customization options applicable to MRZ scanning use cases with the MRZ Scanner. The MRZ Scanner passes an `MRZScannerConfig` object when starting a scan via `MRZScanner.Start(config)`. `MRZScannerConfig` contains the following properties:

1. **`License`** - the license key is the only property whose ***value must be specified when starting the MRZ Scanner***. If the license is undefined, invalid, or expired, the MRZ Scanner cannot proceed with scanning, and instead displays a pop-up error message instructing the user to contact the app administrator to resolve this license issue.

2. **`DocumentType`** - specifies the type of document that the MRZ Scanner will recognize. This property accepts values defined in the `EnumDocumentType` such as `EnumDocumentType.All`, `EnumDocumentType.Id`, or `EnumDocumentType.Passport`. It helps the scanner to optimize its processing based on the expected document type. To learn more about the different document types that are supported, please refer to the [Supported Document Types](index.md#supported-document-types) section of the user guide.

3. **`TemplateFile`** - a template file is a JSON file or JSON string that contains a series of algorithm parameter settings (called Capture Vision templates) that is usually used for very specific and customized scanning and parsing scenarios. The `TemplateFile` points to the name of the JSON file. The MRZ Scanner comes with a default template file, but you may choose to use a custom template to target specialized use cases. We recommend contacting the [Dynamsoft Technical Support Team](https://www.dynamsoft.com/company/contact/) for assistance with template customization.

4. **`IsBeepEnabled`** (default value `false`) - a boolean that determines whether a beep sound is triggered upon a successful MRZ scan. When enabled (`true`), the scanner will play a sound to provide audible feedback.

5. **`IsCameraToggleButtonVisible`** (default value `false`) - a boolean that specifies whether the camera toggle button is displayed. This button lets users switch between available cameras (e.g., front and rear).

6. **`IsCloseButtonVisible`** (default value `true`) - a boolean to control the visibility of the close button on the scanner's UI. If `true`, a close button will be displayed allowing users to exit the MRZ scanning interface.

7. **`IsGuideFrameVisible`** (default value `true`) - serves as a toggle to show or hide the guide frame in the UI during scanning. The guide frame assists users in properly aligning the document for optimal MRZ detection. When set to `true`, a visual overlay is displayed on the scanning interface.

8. **`IsTorchButtonVisible`** (default value `true`) - determines whether the torch (flashlight) toggle button is visible on the scanning interface. Set to `true` to allow users to switch the device's flashlight on or off during MRZ scanning.

9. **`IsVibrateEnabled`** (default value `false`) - controls the scanner's ability to make the scanning device vibrate upon a successful MRZ scan. When enabled (`true`), the scanner will vibrate to provide haptic feedback if the device supports it.

10. **`IsBeepButtonVisible`** (default value `true`) - controls whether the beep toggle button is visible in the scanning UI. When visible, users can tap this button to enable or disable the beep sound directly from the scanner interface.

11. **`IsVibrateButtonVisible`** (default value `true`) - controls whether the vibrate toggle button is visible in the scanning UI. When visible, users can tap this button to enable or disable vibration feedback directly from the scanner interface.

12. **`IsFormatSelectorVisible`** (default value `true`) - controls whether the document format selector is displayed at the bottom of the scanning UI. The format selector allows users to switch between scanning ID cards, passports, or both.

13. **`ReturnDocumentImage`** (default value `true`) - controls whether a cropped document image is included in the scan result. When enabled, the result's `GetDocumentImage()` method will return the document image for each scanned side.

14. **`ReturnOriginalImage`** (default value `false`) - controls whether the original full-frame camera image is included in the scan result. When enabled, the result's `GetOriginalImage()` method will return the unprocessed camera frame for each scanned side.

15. **`ReturnPortraitImage`** (default value `true`) - controls whether the detected portrait image is included in the scan result. When enabled, the result's `GetPortraitImage()` method will return the portrait extracted from the document.

Next, we go over the different ways that these properties can be used to customize the scanner with a few examples.

## Setting the MRZ Document Type

### Using the API

Specifies the type of document to scan, such as ID cards or passports. It also improves the processing speed and accuracy.

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.DocumentType = EnumDocumentType.Passport;
```

### Using a Customized Template File

A template file is a JSON file that includes a series of algorithm parameter settings. It is used to customize the performance for different usage scenarios. [Contact us](https://www.dynamsoft.com/company/customer-service/#contact) to get a customized template for your scanner.

1. Add your customized template JSON file to the **Resources/Raw** folder of your MAUI project. It will be included automatically as a `MauiAsset`.

2. Specify the template file via the `TemplateFile` property:

   ```csharp
   var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
   config.TemplateFile = "CustomizedTemplate.json";
   ```

> [!NOTE]
> You can also pass a JSON string directly as the value of `TemplateFile`.

**Related APIs**

- [`DocumentType`](../api-reference/mrz-scanner-config.md#documenttype)
- [`TemplateFile`](../api-reference/mrz-scanner-config.md#templatefile)

## Configure the UI Elements

<div align="center">
    <p><img src="../../assets/mrz-scanner-ui-341100.png" width="80%" alt="mrz-scanner"></p>
    <p>MRZ Scanner UI</p>
</div>

The MRZ Scanner UI includes the following configurable elements:

- **Close button**: Dismisses the scanner and returns the user to the previous screen.
- **Torch button**: Turns the device flashlight on or off to improve scanning in low-light conditions.
- **Camera toggle button**: Switches between the front and rear cameras for flexible document placement.
- **Beep button**: Lets users enable or disable the audible beep that plays on a successful scan.
- **Vibrate button**: Lets users enable or disable haptic vibration feedback on a successful scan.
- **Guide frame**: A viewfinder overlay that guides users in positioning the document within the camera frame.
- **Format selector**: A bottom control bar for selecting the target document type — ID card, passport, or both.

All UI elements are visible by default (except the camera toggle button). Use the following configuration to hide any elements that are not needed for your use case:

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.IsCloseButtonVisible = false;
config.IsTorchButtonVisible = false;
config.IsCameraToggleButtonVisible = false;
config.IsBeepButtonVisible = false;
config.IsVibrateButtonVisible = false;
config.IsFormatSelectorVisible = false;
config.IsGuideFrameVisible = false;
```

**Related APIs**

- [`IsCloseButtonVisible`](../api-reference/mrz-scanner-config.md#isclosebuttonvisible)
- [`IsTorchButtonVisible`](../api-reference/mrz-scanner-config.md#istorchbuttonvisible)
- [`IsCameraToggleButtonVisible`](../api-reference/mrz-scanner-config.md#iscameratogglebuttonvisible)
- [`IsBeepButtonVisible`](../api-reference/mrz-scanner-config.md#isbeepbuttonvisible)
- [`IsVibrateButtonVisible`](../api-reference/mrz-scanner-config.md#isvibratebuttonvisible)
- [`IsFormatSelectorVisible`](../api-reference/mrz-scanner-config.md#isformatselectorvisible)
- [`IsGuideFrameVisible`](../api-reference/mrz-scanner-config.md#isguideframevisible)

## Enabling Haptic and Audio Feedback

The MRZ Scanner can play a beep sound or vibrate the device upon a successful scan. Both are disabled by default.

> [!NOTE]
> `IsBeepEnabled` and `IsVibrateEnabled` control the feedback *behavior*. To hide the buttons that allow users to toggle these behaviors from the scanning UI, use `IsBeepButtonVisible` and `IsVibrateButtonVisible`.

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.IsBeepEnabled = true;     // play a beep upon a successful MRZ scan
config.IsVibrateEnabled = true;  // vibrate the device upon a successful MRZ scan
```

**Related APIs**

- [`IsBeepEnabled`](../api-reference/mrz-scanner-config.md#isbeepenabled)
- [`IsVibrateEnabled`](../api-reference/mrz-scanner-config.md#isvibrateenabled)
- [`IsBeepButtonVisible`](../api-reference/mrz-scanner-config.md#isbeepbuttonvisible)
- [`IsVibrateButtonVisible`](../api-reference/mrz-scanner-config.md#isvibratebuttonvisible)

## Configure Scan Result Images

By default, the scan result includes a cropped document image and a portrait image. You can control which images are returned to reduce memory usage or processing overhead for your use case.

```csharp
var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
config.ReturnDocumentImage = true;   // Cropped document image (default: true).
config.ReturnPortraitImage = true;   // Portrait image (default: true).
config.ReturnOriginalImage = false;  // Original full-frame image (default: false).
```

Once configured, use the following methods on `MRZScanResult` to access the images:

- `GetDocumentImage(EnumDocumentSide)` - returns the cropped document image for the specified side.
- `GetOriginalImage(EnumDocumentSide)` - returns the original full-frame image for the specified side.
- `GetPortraitImage()` - returns the detected portrait image.

**Related APIs**

- [`ReturnDocumentImage`](../api-reference/mrz-scanner-config.md#returndocumentimage)
- [`ReturnPortraitImage`](../api-reference/mrz-scanner-config.md#returnportraitimage)
- [`ReturnOriginalImage`](../api-reference/mrz-scanner-config.md#returnoriginalimage)
- [`GetDocumentImage`](../api-reference/mrz-scan-result.md#getdocumentimage)
- [`GetOriginalImage`](../api-reference/mrz-scan-result.md#getoriginalimage)
- [`GetPortraitImage`](../api-reference/mrz-scan-result.md#getportraitimage)

## Further Customization

If you have other customization requirements on the `MRZScanner` component, please contact the [Dynamsoft support team](https://www.dynamsoft.com/company/contact/).
