---
layout: default-layout
title: Customizing the MRZ Scanner - Dynamsoft MRZ Scanner iOS Edition
description: Customize the MRZ scan settings via MRZScannerConfig class when using MRZ Scanner iOS Edition
keywords: Configure, config, MRZScannerConfig, iOS, customize
breadcrumbText: Customize MRZ Scanner
noTitleIndex: true
needGenerateH3Content: true
needAutoGenerateSidebar: true
---

# Customizing the MRZ Scanner

When developing with `MRZScannerViewController`, you can add configurations via the `MRZScannerConfig` class. This page will guide you on how to configure the settings.

## `MRZScannerConfig` Overview

The [**`MRZScannerConfig`**](../api-reference/mrz-scanner-config.md) class is capable of configuring almost all customization options applicable to MRZ scanning use cases with the MRZ Scanner. The MRZ Scanner uses passes an `MRZScannerConfig` object to the constructor when creating an MRZ Scanner instance. `MRZScannerConfig` contains the following properties:

1. **`license`** - the license key is the only property whose ***value must be specified when instantiating the MRZ Scanner instance***. If the license is undefined, invalid, or expired, the MRZ Scanner cannot proceed with scanning, and instead displays a pop-up error message instructing the user to contact the app administrator to resolve this license issue.

2. **`documentType`** - specifies the type of document that the MRZ Scanner will recognize. This property accepts values defined in the EnumDocumentType such as `EnumDocumentType.All`, `EnumDocumentType.Id`, or `EnumDocumentType.Passport`. It helps the scanner to optimize its processing based on the expected document type. To learn more about the different document types that are supported, please refer to the [Supported Document Types](index.md#supported-machine-readable-travel-document-types) section of the user guide.

3. **`templateFile`** - a template file is a JSON file or JSON string that contains a series of algorithm parameter settings (called Capture Vision templates) that is usually used for very specific and customized scanning and parsing scenarios. The `templateFile` points to the location of the JSON file. The MRZ Scanner comes with a default template file, but you may choose to use a custom template to target specialized use cases. We recommend contacting the [Dynamsoft Technical Support Team](https://www.dynamsoft.com/company/contact/) for assistance with template customization.

4. **`isBeepEnabled`** (default value `false`) - a boolean that determines whether a beep sound is triggered upon a successful MRZ scan. When enabled (true), the scanner will play a sound to provide audible feedback.

5. **`isCameraToggleButtonVisible`** (default value `true`) - a boolean that specifies whether the camera toggle button is displayed. This button lets users switch between available cameras (e.g., front and rear).

6. **`isCloseButtonVisible`** (default value `true`) - a boolean to control the visibility of the close button on the scanner's UI. If true, a close button will be displayed allowing users to exit the MRZ scanning interface.

7. **`isGuideFrameVisible`** (default value `true`) -  serves as a toggle to show or hide the guide frame in the UI during scanning. The guide frame assists users in properly aligning the document for optimal MRZ detection. When set to true, a visual overlay is displayed on the scanning interface.

8. **`isTorchButtonVisible`** (default value `true`) - determines whether the torch (flashlight) toggle button is visible on the scanning interface. Set to true to allow users to switch the device's flashlight on or off during MRZ scanning.

9. **`isVibrateEnabled`** (default value `false`) - controls the scanner's ability to make the scanning device vibrate upon a successful MRZ scan. When enabled (true), the scanner will vibrate to provide haptic feedback if the device supports it.

10. **`isBeepButtonVisible`** (default value `true`) - controls whether the beep toggle button is visible in the scanning UI. When visible, users can tap this button to enable or disable the beep sound directly from the scanner interface.

11. **`isVibrateButtonVisible`** (default value `true`) - controls whether the vibrate toggle button is visible in the scanning UI. When visible, users can tap this button to enable or disable vibration feedback directly from the scanner interface.

12. **`isFormatSelectorVisible`** (default value `true`) - controls whether the document format selector is displayed at the bottom of the scanning UI. The format selector allows users to switch between scanning ID cards, passports, or both.

13. **`returnDocumentImage`** (default value `true`) - controls whether a cropped document image is included in the scan result. When enabled, the result's `getDocumentImage(_:)` method will return the document image for each scanned side.

14. **`returnOriginalImage`** (default value `false`) - controls whether the original full-frame camera image is included in the scan result. When enabled, the result's `getOriginalImage(_:)` method will return the unprocessed camera frame for each scanned side.

15. **`returnPortraitImage`** (default value `true`) - controls whether the detected portrait image is included in the scan result. When enabled, the result's `getPortraitImage()` method will return the portrait extracted from the document.

Next, we go over the different ways that these properties can be used to customize the scanner with a few examples.

## Setting the MRZ Document Type

### Using the API

Specifies the type of document to scan, such as ID cards or passports. It also improves the processing speed and the accuracy.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
config.documentType = DSDocumentTypePassport;
```
2. 
```swift
let config = MRZScannerConfig()
config.documentType = .passport
```

### Using a customized template file

A template file is a JSON file that includes a series of algorithm parameter settings. It is always used to customize the performance for different usage scenarios. [Contact us](https://www.dynamsoft.com/company/customer-service/#contact) to get a customized template for your scanner.

1. Create a `DynamsoftResources` folder in the finder. Under the `DynamsoftResources` folder create a new folder, `Templates`.

2. Put your customized template json file under the `Templates` folder.

3. Rename the `DynamsoftResources` folder's extension name to .bundle and drag the `DynamsoftResources.bundle` into your project on Xcode. Select Create groups for the Added folders option.

4. Specify the template file via `templateFile` property

   <div class="sample-code-prefix"></div>
   >- Objective-C
   >- Swift
   >
   >1. 
   ```objc
   DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
   config.templateFile = @"CustomizedTemplate.json";
   ```
   2. 
   ```swift
   let config = MRZScannerConfig()
   config.templateFile = "CustomizedTemplate.json"
   ```

> [!NOTE] You can also use a JSON string as the template file.

**Related APIs**

- [`documentType`]({{ site.ios_api }}mrz-scanner-config.html#documenttype)
- [`templateFile`]({{ site.ios_api }}mrz-scanner-config.html#templatefile)

## Configure the UI Elements

<div align="center">
    <p><img src="../../assets/mrz-scanner-ui-341100.png" width="70%" alt="mrz-scanner"></p>
    <p>MRZ Scanner UI Components</p>
</div>

The MRZ Scanner UI includes the following configurable elements:

- **Close button**: Dismisses the scanner and returns the user to the previous screen.
- **Torch button**: Turns the device flashlight on or off to improve scanning in low-light conditions.
- **Camera toggle button**: Switches between the front and rear cameras for flexible document placement.
- **Beep button**: Lets users enable or disable the audible beep that plays on a successful scan.
- **Vibrate button**: Lets users enable or disable haptic vibration feedback on a successful scan.
- **Prompt text**: A status label that updates dynamically to guide users through each step of the scanning process.
- **Guide frame**: A viewfinder overlay that guides users in positioning the document within the camera frame.
- **Format selector**: A bottom control bar for selecting the target document type — ID card, passport, or both.

All UI elements are visible by default. Use the following configuration to hide any elements that are not needed for your use case:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
config.isCloseButtonVisible = false;
config.isTorchButtonVisible = false;
config.isCameraToggleButtonVisible = false;
config.isBeepButtonVisible = false;
config.isVibrateButtonVisible = false;
config.isFormatSelectorVisible = false;
config.isGuideFrameVisible = false;
```
2. 
```swift
let config = MRZScannerConfig()
config.isCloseButtonVisible = false
config.isTorchButtonVisible = false
config.isCameraToggleButtonVisible = false
config.isBeepButtonVisible = false
config.isVibrateButtonVisible = false
config.isFormatSelectorVisible = false
config.isGuideFrameVisible = false
```

**Related APIs**

- [`isCloseButtonVisible`](../api-reference/mrz-scanner-config.md#isclosebuttonvisible)
- [`isTorchButtonVisible`](../api-reference/mrz-scanner-config.md#istorchbuttonvisible)
- [`isCameraToggleButtonVisible`](../api-reference/mrz-scanner-config.md#iscameratogglebuttonvisible)
- [`isBeepButtonVisible`](../api-reference/mrz-scanner-config.md#isbeepbuttonvisible)
- [`isVibrateButtonVisible`](../api-reference/mrz-scanner-config.md#isvibratebuttonvisible)
- [`isFormatSelectorVisible`](../api-reference/mrz-scanner-config.md#isformatselectorvisible)
- [`isGuideFrameVisible`](../api-reference/mrz-scanner-config.md#isguideframevisible)

## Enabling Haptic and Audio Feedback

The MRZ Scanner can play a beep sound or vibrate the device upon a successful scan. Both are disabled by default.

> [!NOTE] The `isBeepEnabled` and `isVibrateEnabled` settings control the feedback *behavior*. To hide the buttons that allow users to toggle these behaviors from the scanning UI, use `isBeepButtonVisible` and `isVibrateButtonVisible`.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
config.isBeepEnabled = true;
config.isVibrateEnabled = true;
```
2. 
```swift
let config = MRZScannerConfig()
config.isBeepEnabled = true
config.isVibrateEnabled = true
```

**Related APIs**

- [`isBeepEnabled`](../api-reference/mrz-scanner-config.md#isbeepenabled)
- [`isVibrateEnabled`](../api-reference/mrz-scanner-config.md#isvibrateenabled)
- [`isBeepButtonVisible`](../api-reference/mrz-scanner-config.md#isbeepbuttonvisible)
- [`isVibrateButtonVisible`](../api-reference/mrz-scanner-config.md#isvibratebuttonvisible)

## Configure Scan Result Images

By default, the scan result includes a cropped document image and a portrait image. You can control which images are returned to reduce memory usage or processing overhead for your use case.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
config.returnDocumentImage = true;   // Cropped document image (default: true).
config.returnPortraitImage = true;   // Portrait image (default: true).
config.returnOriginalImage = false;  // Original full-frame image (default: false).
```
2. 
```swift
let config = MRZScannerConfig()
config.returnDocumentImage = true   // Cropped document image (default: true).
config.returnPortraitImage = true   // Portrait image (default: true).
config.returnOriginalImage = false  // Original full-frame image (default: false).
```

Once configured, use the following methods on `MRZScanResult` to access the images:

- `getDocumentImage(_:)` - returns the cropped document image for the specified side.
- `getOriginalImage(_:)` - returns the original full-frame image for the specified side.
- `getPortraitImage()` - returns the detected portrait image.

**Related APIs**

- [`returnDocumentImage`](../api-reference/mrz-scanner-config.md#returndocumentimage)
- [`returnPortraitImage`](../api-reference/mrz-scanner-config.md#returnportraitimage)
- [`returnOriginalImage`](../api-reference/mrz-scanner-config.md#returnoriginalimage)
- [`getDocumentImage`](../api-reference/mrz-scan-result.md#getdocumentimage)
- [`getOriginalImage`](../api-reference/mrz-scan-result.md#getoriginalimage)
- [`getPortraitImage`](../api-reference/mrz-scan-result.md#getportraitimage)

## Further Customization

If you have other customization requirements on the `MRZScanner` component, you can modify it with the [open source code on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/).
