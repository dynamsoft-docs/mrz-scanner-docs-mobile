---
layout: default-layout
title: MRZScanConfig Class - Dynamsoft MRZ Scanner React Native Edition
description: MRZScanConfig of DynamsoftMRZScanner React Native is the class that defines the configurations for MRZ scanning.
keywords: MRZ, scanner, config, React Native
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanConfig
---

# MRZScanConfig

`MRZScanConfig` is responsible for the configuration of the MRZ Scanner, from assigning the MRZ Scanner license to configuring the supported document types, along with other customizations.

> [!NOTE]
> If you are wondering about the different ways you can customize the MRZ Scanner, please refer to the [MRZ Scanner Customization Guide](../user-guide/customize-mrz-scanner.md).

## Definition

*Assembly:* dynamsoft-mrz-scanner-bundle-react-native

```ts
interface MRZScanConfig
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`license`](#license) | *string* | Represents the MRZ Scanner license string. |
| [`templateFile`](#templatefile) | *string* | Specifies the template configuration that defines the various MRZ Scanner parameters. |
| [`templateNodeRequire`](#templatenoderequire) | *NodeRequire* | Supplies a `require`-resolved template object when loading the template from a bundled JSON asset. |
| [`documentType`](#documenttype) | [*EnumDocumentType*](document-type.md) | Specifies the type of document (ID or Passport) that the MRZ Scanner will recognize. |
| [`isTorchButtonVisible`](#istorchbuttonvisible) | *boolean* | Represents the visibility status of the torch button. |
| [`isBeepEnabled`](#isbeepenabled) | *boolean* | Determines whether a beep sound is triggered upon a successful MRZ scan. |
| [`isBeepButtonVisible`](#isbeepbuttonvisible) | *boolean* | Represents the visibility status of the beep toggle button. |
| [`isCloseButtonVisible`](#isclosebuttonvisible) | *boolean* | Represents the visibility status of the close button. |
| [`isGuideFrameVisible`](#isguideframevisible) | *boolean* | Represents the visibility status of the guide frame on the display. |
| [`isCameraToggleButtonVisible`](#iscameratogglebuttonvisible) | *boolean* | Specifies whether the camera toggle button is displayed or not. |
| [`isVibrateEnabled`](#isvibrateenabled) | *boolean* | Controls the scanner's ability to make the scanning device vibrate upon a successful MRZ scan. |
| [`isVibrateButtonVisible`](#isvibratebuttonvisible) | *boolean* | Represents the visibility status of the vibrate toggle button. |
| [`isFormatSelectorVisible`](#isformatselectorvisible) | *boolean* | Represents the visibility status of the document format selector. |
| [`returnDocumentImage`](#returndocumentimage) | *boolean* | Specifies whether the scanner captures and returns a cropped document image. |
| [`returnOriginalImage`](#returnoriginalimage) | *boolean* | Specifies whether the scanner captures and returns the full camera frame. |
| [`returnPortraitImage`](#returnportraitimage) | *boolean* | Specifies whether the scanner extracts and returns a portrait image from the document. |

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

Supplies a template configuration as a `require`-resolved object, which is the standard way to load a bundled JSON asset in a React Native app (Metro resolves `require('./my-template.json')` at build time). The MRZ Scanner stringifies the resolved object and uses it as the template when `templateFile` is not set.

```ts
templateNodeRequire?: NodeRequire
```

**Remarks**

Use `templateFile` when you have a template as a JSON string or a file path, and `templateNodeRequire` when you want Metro to bundle a template JSON alongside your app code. If both are provided, `templateFile` takes precedence.

### documentType

Specifies the type of document that the MRZ Scanner will recognize, represented as a [`EnumDocumentType`](document-type.md). This property accepts values defined in the EnumDocumentType such as `EnumDocumentType.DT_ALL` (TD1/2/3), `EnumDocumentType.DT_ID` (TD1/2), or `EnumDocumentType.DT_PASSPORT` (TD3).

```ts
documentType?: EnumDocumentType
```

**Remarks**

If you would like to learn more about the supported document types, please refer to the [Supported Document Types](../user-guide/index.md#supported-document-types) section of the user guide.

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

### isBeepButtonVisible

Determines whether the beep toggle button is visible on the scanning interface. When visible, users can enable or disable the beep sound themselves during scanning.

```ts
isBeepButtonVisible?: boolean
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

### isVibrateButtonVisible

Determines whether the vibrate toggle button is visible on the scanning interface. When visible, users can enable or disable haptic feedback themselves during scanning.

```ts
isVibrateButtonVisible?: boolean
```

### isFormatSelectorVisible

Determines whether the document format selector is visible on the scanning interface. When visible, users can switch between document types (ID / Passport) during scanning.

```ts
isFormatSelectorVisible?: boolean
```

### returnDocumentImage

Specifies whether the scanner captures and returns a cropped, perspective-corrected document image in [`MRZScanResult`](mrz-scan-result.md). When enabled, both the MRZ side and opposite side images (where available) are returned via `mrzSideDocumentImage` and `oppositeSideDocumentImage`.

```ts
returnDocumentImage?: boolean
```

### returnOriginalImage

Specifies whether the scanner captures and returns the full camera frame in [`MRZScanResult`](mrz-scan-result.md). When enabled, the raw frames for both sides (where available) are returned via `mrzSideOriginalImage` and `oppositeSideOriginalImage`.

```ts
returnOriginalImage?: boolean
```

### returnPortraitImage

Specifies whether the scanner extracts and returns a portrait image from the document in [`MRZScanResult`](mrz-scan-result.md), available via `portraitImage`.

```ts
returnPortraitImage?: boolean
```
