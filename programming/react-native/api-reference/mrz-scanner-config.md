---
layout: default-layout
title: MRZScannerConfig Class - Dynamsoft MRZ Scanner React Native Edition
description: MRZScannerConfig of DynamsoftMRZScanner React Native is the class that defines the configurations for MRZ scanning.
keywords: MRZ, scanner, config, React Native
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScannerConfig
---

# MRZScanConfig

`MRZScanConfig` is responsible for the configuration of the MRZ Scanner, from assigning the MRZ Scanner license to configuring the supported document types, along with other customizations.

> [!NOTE]
> If you are wondering about the different ways you can customize the MRZ Scanner, please refer to the [MRZ Scanner Customization Guide](../user-guide/customize-mrz-scanner.md).

## Definition

*Assembly:* dynamsoft-capture-vision-react-native

```ts
interface MRZScannerConfig
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`license`](#license) | *String* | Represents the MRZ Scanner license string. |
| [`templateFile`](#templatefile) | *String* | Specifies the template configuration that defines the various MRZ Scanner parameters. |
| [`documentType`](#documenttype) | [*EnumDocumentType*](document-type.md) | Specifies the type of document (ID or Passport) that the MRZ Scanner will recognize. |
| [`isTorchButtonVisible`](#istorchbuttonvisible) | *bool* | Represents the visibility status of the torch button. |
| [`isBeepEnabled`](#isbeepenabled) | *bool* | Determines whether a beep sound is triggered upon a successful MRZ scan. |
| [`isCloseButtonVisible`](#isclosebuttonvisible) | *bool* | Represents the visibility status of the close button. |
| [`isGuideFrameVisible`](#isguideframevisible) | *bool* | Represents the visibility status of the guide frame on the display. |
| [`isCameraToggleButtonVisible`](#Iscameratogglebuttonvisible) | *bool* | Specifies whether the camera toggle button is displayed or not. |
| [`isVibrateEnabled`](#isvibrateenabled) | *bool* | Controls the scanner's ability to make the scanning device vibrate upon a successful MRZ scan. |

### license

The license key is the only property whose ***value must be specified when instantiating the MRZ Scanner instance***. If the license is undefined, invalid, or expired, the MRZ Scanner cannot proceed with scanning, and instead displays a pop-up error message instructing the user to contact the app administrator to resolve this license issue.

```ts
license?: string
```

### templateFile

Specifies the template configuration with a file path or a JSON string that defines the various MRZ Scanner parameters. These specialized templates are usually used for very specific and customized scanning scenarios. 

```ts
templateFile?: string
```

**Remarks**

The MRZ Scanner comes with a default template file, but you may choose to use a custom template to target specialized use cases. We recommend contacting the [Dynamsoft Technical Support Team](https://www.dynamsoft.com/company/contact/) for assistance with template customization.

### templateNodeRequire

Provides a Node.js 'require' function to load the template file when running in a Node environment. This facilitates importing external template configuration files.

```ts
templateNodeRequire?: NodeRequire
```

Remarks

For most typical cases, this 

### documentType

Specifies the type of document that the MRZ Scanner will recognize, represented as a [`EnumDocumentType`](document-type.md). This property accepts values defined in the EnumDocumentType such as `EnumDocumentType.DT_ALL` (TD1/2/3), `EnumDocumentType.DT_ID` (TD1/2), or `EnumDocumentType.DT_PASSPORT` (TD3).

```ts
documentType?: EnumDocumentType
```

**Remarks**

If you would like to learn more about the supported document types, please refer to the [Supported Document Types](../user-guide/index.md#supported-machine-readable-travel-document-types) section of the user guide.

### isTorchButtonVisible

Determines whether the torch (flashlight) toggle button is visible on the scanning interface. Set to true to allow users to switch the device's flashlight on or off during MRZ scanning.

```ts
isTorchButtonVisible?: boolean
```

### isBeepEnabled

Determines whether a beep sound is triggered upon a successful MRZ scan. When enabled (true), the scanner will play a sound to provide audible feedback.

```ts
isBeepEnabled?: boolean
```

### isCloseButtonVisible

Controls the visibility of the close button on the scanner's UI. If true, a close button will be displayed allowing users to exit the MRZ scanning interface.

```ts
isCloseButtonVisible?: boolean
```

### isGuideFrameVisible

Represents the visibility status of the guide frame on the display.

```ts
isGuideFrameVisible?: boolean
```

### isCameraToggleButtonVisible

Specifies whether the camera toggle button is displayed. This button lets users switch between available cameras (e.g., front and rear).

```ts
isCameraToggleButtonVisible?: boolean
```

### isVibrateEnabled

Controls the scanner's ability to make the scanning device vibrate upon a successful MRZ scan. When enabled (true), the scanner will vibrate to provide haptic feedback if the device supports it.

```ts
isVibrateEnabled?: boolean
```
