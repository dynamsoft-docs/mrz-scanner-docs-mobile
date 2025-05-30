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

`MRZScannerConfig` is the class that defines the configurations for MRZ scanning. It is  via the input value of `MRZScannerActivity.ResultContract`

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
final class MRZScannerConfig
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`License`](#license) | *string* | Represents the license string. |
| [`TemplateFile`](#templatefile) | *string* | Represents the template with a file path or a JSON string. |
| [`DocumentType`](#documenttype) | [*EnumDocumentType*](document-type.md) | Represents the type of document to scan, such as ID cards or passports. |
| [`IsTorchButtonVisible`](#istorchbuttonvisible) | *bool* | Represents the visibility status of the torch button. |
| [`IsBeepEnabled`](#isbeepenabled) | *bool* | Represents the play status of the beep sound when a MRZ is scanned. |
| [`IsCloseButtonVisible`](#isclosebuttonvisible) | *bool* | Represents the visibility status of the close button. |
| [`IsGuideFrameVisible`](#isguideframevisible) | *bool* | Represents the visibility status of the guide frame on the display. |
| [`IsCameraToggleButtonVisible`](#Iscameratogglebuttonvisible) | *bool* | Represents the visibility status of the camera toggle button. |

### License

Represents the license string.

```csharp
string License { get; set; };
```

### TemplateFile

Represents the template with a file path or a JSON string.

```csharp
string? TemplateFile { get; set; };
```

### DocumentType

Represents the type of document to scan, such as ID cards or passports.

```csharp
EnumDocumentType DocumentType { get; set; };
```

### IsTorchButtonVisible

Represents the visibility status of the torch button.

```csharp
bool IsTorchButtonVisible { get; set; };
```

### IsBeepEnabled

Represents the play status of the beep sound when a MRZ is scanned.

```csharp
bool IsBeepEnabled { get; set; };
```

### IsCloseButtonVisible

Represents the visibility status of the close button.

```csharp
bool IsCloseButtonVisible { get; set; };
```

### IsGuideFrameVisible

Represents the visibility status of the guide frame on the display.

```csharp
bool IsGuideFrameVisible { get; set; };
```

### IsCameraToggleButtonVisible

Represents the visibility status of the camera toggle button.

```csharp
bool IsCameraToggleButtonVisible { get; set; };
```
