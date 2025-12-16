---
layout: default-layout
title: Customize MRZ Scanner - Dynamsoft MRZ Scanner Flutter Edition
description: Customize the MRZ scan settings via MRZScannerConfig class when using MRZ Scanner Flutter Edition
keywords: Customize, config, MRZScannerConfig, Flutter
breadcrumbText: Customize MRZ Scanner
noTitleIndex: true
needGenerateH3Content: true
needAutoGenerateSidebar: true
---

# Customizing the MRZ Scanner

When developing with `MRZScanner` component (see the [User Guide](index.md)), you can add extra configurations via the `MRZScannerConfig` class. Before diving into the different ways in which you can customize the MRZ Scanner, let's first break down the different properties of the `MRZScannerConfig` class.

## `MRZScannerConfig` Overview

The [**`MRZScannerConfig`**](../api-reference/mrz-scanner-config.md) class is capable of configuring almost all customization options applicable to MRZ scanning use cases with the MRZ Scanner. The MRZ Scanner uses passes an `MRZScannerConfig` object to the constructor when creating an MRZ Scanner instance. `MRZScannerConfig` contains the following properties:

1. **`license`** - the license key is the only property whose ***value must be specified when instantiating the MRZ Scanner instance***. If the license is undefined, invalid, or expired, the MRZ Scanner cannot proceed with scanning, and instead displays a pop-up error message instructing the user to contact the app administrator to resolve this license issue.

2. **`documentType`** - specifies the type of document that the MRZ Scanner will recognize. This property accepts values defined in the EnumDocumentType such as `EnumDocumentType.all`, `EnumDocumentType.id`, or `EnumDocumentType.passport`. It helps the scanner to optimize its processing based on the expected document type. To learn more about the different document types that are supported, please refer to the [Supported Document Types](index.md#supported-machine-readable-travel-document-types) section of the user guide.

3. **`templateFile`** - a template file is a JSON file or JSON string that contains a series of algorithm parameter settings (called Capture Vision templates) that is usually used for very specific and customized scanning and parsing scenarios. The `templateFile` points to the location of the JSON file. The MRZ Scanner comes with a default template file, but you may choose to use a custom template to target specialized use cases. We recommend contacting the [Dynamsoft Technical Support Team](https://www.dynamsoft.com/company/contact/) for assistance with template customization.

4. **`isBeepEnabled`** (default value `false`) - a boolean that determines whether a beep sound is triggered upon a successful MRZ scan. When enabled (true), the scanner will play a sound to provide audible feedback.

5. **`isCameraToggleButtonVisible`** (default value `false`) - a boolean that specifies whether the camera toggle button is displayed. This button lets users switch between available cameras (e.g., front and rear).

6. **`isCloseButtonVisible`** (default value `true`) - a boolean to control the visibility of the close button on the scanner's UI. If true, a close button will be displayed allowing users to exit the MRZ scanning interface.

7. **`isGuideFrameVisible`** (default value `true`) -  serves as a toggle to show or hide the guide frame in the UI during scanning. The guide frame assists users in properly aligning the document for optimal MRZ detection. When set to true, a visual overlay is displayed on the scanning interface.

8. **`isTorchButtonVisible`** (default value `true`) - determines whether the torch (flashlight) toggle button is visible on the scanning interface. Set to true to allow users to switch the device's flashlight on or off during MRZ scanning.

9. **`isVibrateEnabled`** (default value `false`) - controls the scanner's ability to make the scanning device vibrate upon a successful MRZ scan. When enabled (true), the scanner will vibrate to provide haptic feedback if the device supports it.

Next, we go over the different ways that these properties can be used to customize the scanner with a few examples.


## Setting the MRZ Document Type

The MRZ Scanner reads all three MRZ formats, but it can optionally restrict the MRZ format that it reads. For example, you may want to configure MRZ scanner to only read **TD1** and passport (**TD3**) document types, while **ignoring TD2** documents. Here is a simple edit to the launchMrzScanner function that we implemented in the [User Guide](index.md) that sets the specific MRZ formats to read using the `documentType` property:

```dart
void _launchMrzScanner() async {
  var config = MRZScannerConfig(
    license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
    documentType: EnumDocumentType.passport, // configuring the scanner to just recognize passports
  );
  MRZScanResult mrzScanResult = await MRZScanner.launch(config);
  if(mrzScanResult.status == EnumResultStatus.finished) {
    MRZData data = mrzScanResult.mrzData!;
    // do something with the data
  }
}
```

## Customizing the UI Elements

<div align="center">
    <p><img src="../../assets/mrz-scanner-ui.png" width="70%" alt="mrz-scanner"></p>
    <p>MRZ Scanner UI</p>
</div>

The MRZ Scanner UI comes with three main elements that you can configure:

- *Close button*: Stop MRZ scanning and go back to the previous activity.
- *Torch button*: A clickable button that can turn on/off the torch.
- *Guide Frame*: A visual overlap to assist users in lining up the MRZ document in the camera frame.

```dart
void _launchMrzScanner() async {
  var config = MRZScannerConfig(
    license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
    isCloseButtonVisible: false, // hiding the close button
    isTorchButtonVisible: false, // hiding the torch button
    isGuideFrameVisible: false, // hiding the guide frame
  );
  MRZScanResult mrzScanResult = await MRZScanner.launch(config);
  if(mrzScanResult.status == EnumResultStatus.finished) {
    MRZData data = mrzScanResult.mrzData!;
    // do something with the data
  }
}
```

## Enabling Audio and Haptic Feedback

The MRZ Scanner library also offers the option to enable audio and haptic feedback upon a successful MRZ scan. Through the `isBeepEnabled` and `isVibrateEnabled` properties, you can choose to play a sound or make the device vibrate once the MRZ is recognized.

```dart
void _launchMrzScanner() async {
  var config = MRZScannerConfig(
    license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
    isBeepEnabled: true, // play a beep upon a successful MRZ scan
    isVibrateEnabled: true, // make the phone vibrate upon a successful MRZ scan
  );
  MRZScanResult mrzScanResult = await MRZScanner.launch(config);
  if(mrzScanResult.status == EnumResultStatus.finished) {
    MRZData data = mrzScanResult.mrzData!;
    // do something with the data
  }
}
```
