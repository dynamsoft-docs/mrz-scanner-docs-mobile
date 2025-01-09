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
| [`setLicense`](#setlicense) | Set the license. |
| [`setTemplateFilePath`](#settemplatefilepath) | Set a path for the SDK to load template file. |
| [`setTorchButtonVisible`](#settorchbuttonvisible) | Set whether to display the torch button when scanning. Uses can click the torch button to turn on/off the torch. |
| [`setBeepEnabled`](#setbeepenabled) | Set whether to trigger a beep sound when MRZ is scanned. |
| [`setCloseButtonVisible`](#setclosebuttonvisible) | Set whether to display a button for uses to close the scanner page. |
| [`setDocumentType`](#setdocumenttype) | Specifies the type of document to scan, such as ID cards or passports. |
| [`getLicense`](#getlicense) | Returns the license string. |
| [`getTemplateFilePath`](#gettemplatefilepath) | Get the file path of the template file. |
| [`isTorchButtonVisible`](#istorchbuttonvisible) | Returns whether the torch button is visible. |
| [`isBeepEnabled`](#isbeepenabled) | Returns whether the beep sound is enabled. |
| [`isCloseButtonVisible`](#isclosebuttonvisible) | Returns whether the close button is visible. |
| [`getDocumentType`](#getdocumenttype) | Returns the type of document to scan, such as ID cards or passports. |
| [`isGuideFrameVisible`](#isguideframevisible) | Returns whether the guide frame is visible. |
| [`setGuideFrameVisible`](#setguideframevisible) | Set whether to display the guide frame. |

### setLicense

Set the license.

```java
void setLicense(String license);
```

**Parameter(s)**

`license`: The license key to be used for initialization.

### setTemplateFilePath

Set a path for the SDK to load template file.

```java
void setTemplateFilePath(String templateFilePath);
```

**Parameter(s)**

`templateFilePath`: The path of the JSON template file.

### setTorchButtonVisible

Set whether to display the torch button when scanning. Uses can click the torch button to turn on/off the torch.

```java
void setTorchButtonVisible(boolean torchButtonVisible);
```

**Parameter(s)**

`torchButtonVisible`: A boolean value that determines whether to display the torch button.

### setBeepEnabled

Set whether to trigger a beep sound when a MRZ is scanned.

```java
void setBeepEnabled(boolean beepEnabled);
```

**Parameter(s)**

`beepEnabled`: A boolean value that determines whether to enable the beep sound.

### setCloseButtonVisible

Set whether to display a button for uses to close the scanner page.

```java
void setCloseButtonVisible(boolean closeButtonVisible);
```

**Parameter(s)**

`closeButtonVisible`: A boolean value that determines whether to display the close button.

### setDocumentType

Specifies the type of document to scan, such as ID cards or passports.

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

### getTemplateFilePath

Get the file path of the template file.

```java
String getTemplateFilePath();
```

**Return Value**

The path of the JSON template file.

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

Returns whether the guide frame is visible.

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
