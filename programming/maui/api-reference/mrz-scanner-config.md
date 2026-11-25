---
layout: default-layout
title: MRZScannerConfig Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZScannerConfig of DynamsoftMRZScanner MAUI is the class that defines the configurations for MRZ scanning.
keywords: MRZ, scanner, config
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScannerConfig
---

# MRZScannerConfig

`MRZScannerConfig` is the class that defines the configurations for MRZ scanning. Pass it to [`MRZScanner.Start`](mrz-scanner.md#start).

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public class MRZScannerConfig
```

## Constructors

```csharp
public MRZScannerConfig(string license);
public MRZScannerConfig();
```

The parameterless constructor leaves [`License`](#license) empty, so set it before calling `Start`. Every other property starts at the default given below.

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`License`](#license) | *string* | Sets or returns the license string. |
| [`TemplateFile`](#templatefile) | *string?* | Sets or returns the template as a file name, a file path or a JSON string. |
| [`DocumentType`](#documenttype) | [*EnumDocumentType*](document-type.md) | Sets or returns the document type to scan, such as ID cards or passports. |
| [`IsTorchButtonVisible`](#istorchbuttonvisible) | *bool* | Sets or returns the visibility of the torch button. |
| [`IsBeepEnabled`](#isbeepenabled) | *bool* | Sets or returns whether a beep plays when an MRZ is scanned. |
| [`IsVibrateEnabled`](#isvibrateenabled) | *bool* | Sets or returns whether the device vibrates when an MRZ is scanned. |
| [`IsCloseButtonVisible`](#isclosebuttonvisible) | *bool* | Sets or returns the visibility of the close button. |
| [`IsGuideFrameVisible`](#isguideframevisible) | *bool* | Sets or returns the visibility of the guide frame. |
| [`IsCameraToggleButtonVisible`](#iscameratogglebuttonvisible) | *bool* | Sets or returns the visibility of the camera toggle button. |
| [`IsBeepButtonVisible`](#isbeepbuttonvisible) | *bool* | Sets or returns the visibility of the beep toggle button. |
| [`IsVibrateButtonVisible`](#isvibratebuttonvisible) | *bool* | Sets or returns the visibility of the vibrate toggle button. |
| [`IsFormatSelectorVisible`](#isformatselectorvisible) | *bool* | Sets or returns the visibility of the document format selector. |
| [`ReturnDocumentImage`](#returndocumentimage) | *bool* | Sets or returns whether the scan result includes the cropped document images. |
| [`ReturnPortraitImage`](#returnportraitimage) | *bool* | Sets or returns whether the scan result includes the portrait image. |
| [`ReturnOriginalImage`](#returnoriginalimage) | *bool* | Sets or returns whether the scan result includes the original full-frame images. |
| [`IsCameraPermissionPromptEnabled`](#iscamerapermissionpromptenabled) | *bool* | Sets or returns whether the scanner shows its own prompt when camera access is unavailable. |

### License

Sets or returns the license string. A valid license is required to scan.

```csharp
string License { get; set; }
```

### TemplateFile

Sets or returns the template as a file name, a file path or a JSON string. `null` by default, which uses the built-in template.

```csharp
string? TemplateFile { get; set; }
```

**Remarks**

- A file name such as `"MyTemplate.json"` is first looked up among the app's MAUI assets (**Resources/Raw**).
- A value that contains a path separator is read as a file path on the device.
- A value starting with `{` or `[` is read as inline JSON. Leading whitespace is ignored.

`Start` doesn't modify this property. If the template can't be loaded, `Start` returns an `Exception` result.

A custom template replaces the built-in ones, so [`DocumentType`](#documenttype) has no effect, and on iOS the format selector is hidden.

### DocumentType

Sets or returns the document type to scan, such as ID cards or passports. `EnumDocumentType.All` by default.

```csharp
EnumDocumentType DocumentType { get; set; }
```

### IsTorchButtonVisible

Sets or returns the visibility of the torch button, which turns the flashlight on or off. Visible by default.

```csharp
bool IsTorchButtonVisible { get; set; }
```

### IsBeepEnabled

Sets or returns whether a beep plays when an MRZ is scanned. Disabled by default.

```csharp
bool IsBeepEnabled { get; set; }
```

### IsVibrateEnabled

Sets or returns whether the device vibrates when an MRZ is scanned. Disabled by default.

```csharp
bool IsVibrateEnabled { get; set; }
```

### IsCloseButtonVisible

Sets or returns the visibility of the close button. Visible by default.

```csharp
bool IsCloseButtonVisible { get; set; }
```

### IsGuideFrameVisible

Sets or returns the visibility of the guide frame. Visible by default. When hidden, the whole camera preview is scanned.

```csharp
bool IsGuideFrameVisible { get; set; }
```

### IsCameraToggleButtonVisible

Sets or returns the visibility of the camera toggle button, which switches between the rear and front cameras. Visible by default.

```csharp
bool IsCameraToggleButtonVisible { get; set; }
```

### IsBeepButtonVisible

Sets or returns the visibility of the beep toggle button. Visible by default.

```csharp
bool IsBeepButtonVisible { get; set; }
```

### IsVibrateButtonVisible

Sets or returns the visibility of the vibrate toggle button. Visible by default.

```csharp
bool IsVibrateButtonVisible { get; set; }
```

### IsFormatSelectorVisible

Sets or returns the visibility of the document format selector, which switches between ID cards, passports, or both. Visible by default.

```csharp
bool IsFormatSelectorVisible { get; set; }
```

### ReturnDocumentImage

Sets or returns whether the scan result includes the cropped document images. Enabled by default.

```csharp
bool ReturnDocumentImage { get; set; }
```

### ReturnPortraitImage

Sets or returns whether the scan result includes the portrait image. Enabled by default.

```csharp
bool ReturnPortraitImage { get; set; }
```

### ReturnOriginalImage

Sets or returns whether the scan result includes the original full-frame images. Disabled by default.

```csharp
bool ReturnOriginalImage { get; set; }
```

### IsCameraPermissionPromptEnabled

Sets or returns whether the scanner shows its own alert or dialog when camera access is unavailable. Enabled by default.

```csharp
bool IsCameraPermissionPromptEnabled { get; set; }
```

**Remarks**

When enabled, the scanner explains the problem and offers a way to grant access: **Open Settings** on iOS; on Android, **Allow camera access** while the permission can still be requested, otherwise **Open Settings**. Set it to `false` to present your own UI instead. If the scan ends without access, the denial is reported either way, as an `Exception` result with [`EnumErrorCode`](error-code.md) `CameraPermissionDenied` (1001) or `CameraPermissionRestricted` (1002).

> [!IMPORTANT]
> The camera never starts without access, whatever this setting.
