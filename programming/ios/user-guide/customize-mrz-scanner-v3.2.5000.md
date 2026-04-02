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

5. **`isCameraToggleButtonVisible`** (default value `false`) - a boolean that specifies whether the camera toggle button is displayed. This button lets users switch between available cameras (e.g., front and rear).

6. **`isCloseButtonVisible`** (default value `true`) - a boolean to control the visibility of the close button on the scanner's UI. If true, a close button will be displayed allowing users to exit the MRZ scanning interface.

7. **`isGuideFrameVisible`** (default value `true`) -  serves as a toggle to show or hide the guide frame in the UI during scanning. The guide frame assists users in properly aligning the document for optimal MRZ detection. When set to true, a visual overlay is displayed on the scanning interface.

8. **`isTorchButtonVisible`** (default value `true`) - determines whether the torch (flashlight) toggle button is visible on the scanning interface. Set to true to allow users to switch the device's flashlight on or off during MRZ scanning.

9. **`isVibrateEnabled`** (default value `false`) - controls the scanner's ability to make the scanning device vibrate upon a successful MRZ scan. When enabled (true), the scanner will vibrate to provide haptic feedback if the device supports it.

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
config.documentType = [.passport]
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
   1. 
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
    <p><img src="../../assets/mrz-scanner-ui.png" width="70%" alt="mrz-scanner"></p>
    <p>MRZ Scanner UI Components</p>
</div>

- Close button: Stop MRZ scanning and go back to the previous activity.
- Torch button: A clickable button that can turn on/off the torch.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
config.isCloseButtonVisible = false;
config.isGuideFrameVisible = false;
config.isTorchButtonVisible = false;
```
2. 
```swift
let config = MRZScannerConfig()
config.isCloseButtonVisible = false
config.isGuideFrameVisible = false
config.isTorchButtonVisible = false
```

**Related APIs**

- [`torchButtonVisible`]({{ site.ios_api }}mrz-scanner-config.html#torchbuttonvisible)
- [`closeButtonVisible`]({{ site.ios_api }}mrz-scanner-config.html#closebuttonvisible)

## Enabling Haptic and Audio Feedback

The MRZ Scanner library also offers the option to enable audio and haptic feedback upon a successful MRZ scan. Through the `isBeepEnabled` and `isVibrateEnabled` properties, you can choose to play a sound or make the device vibrate once the MRZ is recognized.

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
1. 
```swift
let config = MRZScannerConfig()
config.isBeepEnabled = true
config.isVibrateEnabled = true
```

**Related API**

- [`isBeepEnabled`]({{ site.ios_api }}mrz-scanner-config.html#isbeepenabled)
- [`isVibrateEnabled`]({{ site.ios_api }}mrz-scanner-config.html#isvibrateenabled)

## Further Customization

If you have other customization requirements on the `MRZScanner` component, you can modify it with the [open source code on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/).
