---
layout: default-layout
title: MRZScannerConfig Class - Dynamsoft MRZ Scanner Android Edition
description: MRZScannerConfig of DynamsoftMRZScanner Android is the class that defines the configurations for MRZ scanning.
keywords: MRZ, scanner, config 
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScannerConfig
---

# MRZScannerConfig

`MRZScannerConfig` is the class that defines the configurations for MRZ scanning. It is set via the input value of `MRZScannerActivity.ResultContract`

## Definition

*Assembly:* DynamsoftMRZScanner.aar

*Namespace:* com.dynamsoft.mrzscanner.ui

```java
final class MRZScannerConfig
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`setLicense`](#setlicense) | Sets the license string. |
| [`setTemplateFile`](#settemplatefile) | Sets the template with a file path or a JSON string. |
| [`setTorchButtonVisible`](#settorchbuttonvisible) | Sets whether to display the torch button when scanning. |
| [`setBeepEnabled`](#setbeepenabled) | Sets whether the beep sound is triggered or not when a MRZ is scanned. |
| [`setVibrateEnabled`](#setvibrateenabled) | Sets whether the vibration is triggered or not when a MRZ is scanned. |
| [`setCloseButtonVisible`](#setclosebuttonvisible) | Sets the visibility status of the close button for users to close the scanner page. |
| [`setDocumentType`](#setdocumenttype) | Sets the type of document to scan, such as ID cards or passports. |
| [`getLicense`](#getlicense) | Returns the license string. |
| [`getTemplateFile`](#gettemplatefile) | Returns the template with a file path or a JSON string. |
| [`isTorchButtonVisible`](#istorchbuttonvisible) | Returns the visibility status of the torch button. |
| [`isBeepEnabled`](#isbeepenabled) | Returns the play status of the beep sound when a MRZ is scanned. |
| [`isVibrateEnabled`](#isvibrateenabled) | Returns the play status of the vibration when a MRZ is scanned. |
| [`isCloseButtonVisible`](#isclosebuttonvisible) | Returns the visibility status of the close button. |
| [`getDocumentType`](#getdocumenttype) | Returns the type of document to scan, such as ID cards or passports. |
| [`isGuideFrameVisible`](#isguideframevisible) | Returns the visibility status of the guide frame on the display. |
| [`setGuideFrameVisible`](#setguideframevisible) | Sets the visibility of the guide frame on the display. |

The following methods are deprecated:

| Method | Description |
| ------ | ----------- |
| [`setTemplateFilePath`](#settemplatefilepath) | Sets the local file path for the JSON parameters template file. |
| [`getTemplateFilePath`](#gettemplatefilepath) | Returns the local path of the settings template file. |

### setLicense

Set the license.

```java
void setLicense(String license);
```

**Parameter(s)**

`license`: The license key to be used for initialization.

### setTemplateFile

Sets the template with a file path or a JSON string.

```java
void setTemplateFile(String templateFile);
```

**Parameter(s)**

`templateFile`: The file path or a JSON string.

### setTorchButtonVisible

Sets whether to display the torch button when scanning. Users can click the torch button to turn on/off the torch.

```java
void setTorchButtonVisible(boolean torchButtonVisible);
```

**Parameter(s)**

`torchButtonVisible`: A boolean value that determines whether to display the torch button.

### setBeepEnabled

Sets whether the beep sound is triggered or not when a MRZ is scanned.

```java
void setBeepEnabled(boolean beepEnabled);
```

**Parameter(s)**

`beepEnabled`: A boolean value that determines whether to enable the beep sound.

### setVibrateEnabled

Sets whether the vibration is triggered or not when a MRZ is scanned.

```java
void setVibrateEnabled(boolean vibrateEnabled);
```

**Parameter(s)**

`vibrateEnabled`: A boolean value that determines whether to enable the vibration.

### setCloseButtonVisible

Sets the visibility status of the close button for users to close the scanner page.

```java
void setCloseButtonVisible(boolean closeButtonVisible);
```

**Parameter(s)**

`closeButtonVisible`: A boolean value that determines whether to display the close button.

### setDocumentType

Sets the type of document to scan, such as ID cards or passports.

```java
void setDocumentType(EnumDocumentType type);
```

**Parameter(s)**

`type`: The type of document to scan, such as ID cards or passports.

### getLicense

Returns the license string.

```java
String getLicense();
```

**Return Value**

The license key to be used for initialization.

### getTemplateFile

Returns the template with a file path or a JSON string.

```java
String getTemplateFile();
```

**Return Value**

A file path or a JSON string.

### isTorchButtonVisible

Returns whether the button is visible.

```java
boolean isTorchButtonVisible();
```

**Return Value**

A boolean value that determines whether the torch button is displayed.

### isBeepEnabled

Returns whether the beep sound is enabled.

```java
boolean isBeepEnabled();
```

**Return Value**

A boolean value that determines whether the beep sound is enabled.

### isVibrateEnabled

Returns whether the vibration is enabled.

```java
boolean isVibrateEnabled();
```

**Return Value**

A boolean value that determines whether the vibration is enabled.

### isCloseButtonVisible

Returns whether the close button is visible.

```java
boolean isCloseButtonVisible();
```

**Return Value**

A boolean value that determines whether the close button is displayed.

### getDocumentType

Returns the type of document to scan, such as ID cards or passports.

```java
EnumDocumentType getDocumentType();
```

**Return Value**

The type of document to scan, such as ID cards or passports.

### isGuideFrameVisible

Returns the visibility status of the guide frame on the display.

```java
boolean isGuideFrameVisible();
```

**Return Value**

A boolean value that determines whether the guide frame is displayed.

### setGuideFrameVisible

Set whether to display the guide frame.

```java
void setGuideFrameVisible(boolean guideFrameVisible);
```

**Parameter(s)**

`guideFrameVisible`: A boolean value that determines whether to display the guide frame.

### setTemplateFilePath

Set a path for the SDK to load template file.

```java
void setTemplateFilePath(String templateFilePath);
```

**Parameter(s)**

`templateFilePath`: The path of the JSON template file.

### getTemplateFilePath

Returns the local path of the settings template file.

```java
String getTemplateFilePath();
```

**Return Value**

The path of the JSON template file.
