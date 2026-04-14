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

`MRZScannerConfig` is the class that defines the configurations for MRZ scanning. It is passed to `MRZScanner.Start()` to configure the scanner before launching.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
class MRZScannerConfig
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`License`](#license) | *string* | Sets or returns the license string. |
| [`TemplateFile`](#templatefile) | *string?* | Sets or returns the template with a file path or a JSON string. |
| [`DocumentType`](#documenttype) | [*EnumDocumentType*](document-type.md) | Sets or returns the document type to scan, such as ID cards or passports. |
| [`IsTorchButtonVisible`](#istorchbuttonvisible) | *bool* | Sets or returns the visibility of the torch button. |
| [`IsBeepEnabled`](#isbeepenabled) | *bool* | Sets or returns whether the beep sound is enabled when a MRZ is scanned. |
| [`IsVibrateEnabled`](#isvibrateenabled) | *bool* | Sets or returns whether the vibration is enabled when a MRZ is scanned. |
| [`IsCloseButtonVisible`](#isclosebuttonvisible) | *bool* | Sets or returns the visibility of the close button. |
| [`IsGuideFrameVisible`](#isguideframevisible) | *bool* | Sets or returns the visibility of the guide frame on the display. |
| [`IsCameraToggleButtonVisible`](#iscameratogglebuttonvisible) | *bool* | Sets or returns the visibility of the camera toggle button. |
| [`IsBeepButtonVisible`](#isbeepbuttonvisible) | *bool* | Sets or returns the visibility of the beep toggle button. |
| [`IsVibrateButtonVisible`](#isvibratebuttonvisible) | *bool* | Sets or returns the visibility of the vibrate toggle button. |
| [`IsFormatSelectorVisible`](#isformatselectorvisible) | *bool* | Sets or returns the visibility of the document format selector. |
| [`ReturnDocumentImage`](#returndocumentimage) | *bool* | Sets or returns whether to return a cropped document image in the scan result. |
| [`ReturnPortraitImage`](#returnportraitimage) | *bool* | Sets or returns whether to return a cropped portrait image in the scan result. |
| [`ReturnOriginalImage`](#returnoriginalimage) | *bool* | Sets or returns whether to return the original full-frame image in the scan result. |

### License

Sets or returns the license string.

```csharp
string License { get; set; }
```

### TemplateFile

Sets or returns the template with a file path or a JSON string.

```csharp
string? TemplateFile { get; set; }
```

### DocumentType

Sets or returns the document type to scan, such as ID cards or passports.

```csharp
EnumDocumentType DocumentType { get; set; }
```

### IsTorchButtonVisible

Sets or returns the visibility of the torch button. Users can tap the torch button to turn on or off the torch.

```csharp
bool IsTorchButtonVisible { get; set; }
```

### IsBeepEnabled

Sets or returns whether the beep sound is enabled when a MRZ is scanned.

```csharp
bool IsBeepEnabled { get; set; }
```

### IsVibrateEnabled

Sets or returns whether the vibration is enabled when a MRZ is scanned.

```csharp
bool IsVibrateEnabled { get; set; }
```

### IsCloseButtonVisible

Sets or returns the visibility of the close button.

```csharp
bool IsCloseButtonVisible { get; set; }
```

### IsGuideFrameVisible

Sets or returns the visibility of the guide frame on the display.

```csharp
bool IsGuideFrameVisible { get; set; }
```

### IsCameraToggleButtonVisible

Sets or returns the visibility of the camera toggle button that allows users to switch between the rear and front cameras.

```csharp
bool IsCameraToggleButtonVisible { get; set; }
```

### IsBeepButtonVisible

Sets or returns the visibility of the beep toggle button. Users can tap the beep button to turn on or off the beep sound.

```csharp
bool IsBeepButtonVisible { get; set; }
```

### IsVibrateButtonVisible

Sets or returns the visibility of the vibrate toggle button. Users can tap the vibrate button to turn on or off vibration feedback.

```csharp
bool IsVibrateButtonVisible { get; set; }
```

### IsFormatSelectorVisible

Sets or returns the visibility of the document format selector that allows users to switch between scanning modes (ID cards, passports, or both).

```csharp
bool IsFormatSelectorVisible { get; set; }
```

### ReturnDocumentImage

Sets or returns whether to return a cropped document image in the scan result. Enabled by default.

```csharp
bool ReturnDocumentImage { get; set; }
```

### ReturnPortraitImage

Sets or returns whether to return a cropped portrait image in the scan result. Enabled by default.

```csharp
bool ReturnPortraitImage { get; set; }
```

### ReturnOriginalImage

Sets or returns whether to return the original full-frame image in the scan result. Disabled by default.

```csharp
bool ReturnOriginalImage { get; set; }
```
