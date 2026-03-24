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
| [`isCameraToggleButtonVisible`](#iscameratogglebuttonvisible) | Returns the visibility status of the camera toggle button. |
| [`setCameraToggleButtonVisible`](#setcameratogglebuttonvisible) | Sets the visibility of the camera toggle button. |
| [`setBeepButtonVisible`](#setbeepbuttonvisible) | Sets the visibility of the beep toggle button in the UI. |
| [`isBeepButtonVisible`](#isbeepbuttonvisible) | Returns the visibility status of the beep toggle button. |
| [`setVibrateButtonVisible`](#setvibrateButtonvisible) | Sets the visibility of the vibrate toggle button in the UI. |
| [`isVibrateButtonVisible`](#isvibrateButtonvisible) | Returns the visibility status of the vibrate toggle button. |
| [`setFormatSelectorVisible`](#setformatselectorvisible) | Sets the visibility of the document format selector. |
| [`isFormatSelectorVisible`](#isformatselectorvisible) | Returns the visibility status of the document format selector. |
| [`setReturnDocumentImage`](#setreturndocumentimage) | Sets whether to return a cropped document image after scanning. |
| [`isReturnDocumentImage`](#isreturndocumentimage) | Returns whether a cropped document image is returned after scanning. |
| [`setReturnOriginalImage`](#setreturnoriginalimage) | Sets whether to return the original full-frame image after scanning. |
| [`isReturnOriginalImage`](#isreturnoriginalimage) | Returns whether the original full-frame image is returned after scanning. |
| [`setReturnPortraitImage`](#setreturnportraitimage) | Sets whether to return the detected portrait image after scanning. |
| [`isReturnPortraitImage`](#isreturnportraitimage) | Returns whether the detected portrait image is returned after scanning. |

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

### isCameraToggleButtonVisible

Returns whether the camera toggle button is visible. The camera toggle button allows users to switch between the front and rear cameras.

```java
boolean isCameraToggleButtonVisible();
```

**Return Value**

A boolean value that determines whether the camera switch button is displayed.

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

### setCameraToggleButtonVisible

Sets whether to display the camera switch button or not. The camera toggle button allows users to switch between the front and rear cameras.

```java
void setCameraToggleButtonVisible(boolean cameraToggleButtonVisible);
```

**Parameter(s)**

`cameraToggleButtonVisible`: A boolean value that determines whether to display the camera switch button.

### setBeepButtonVisible

Sets the visibility of the beep toggle button in the scanning UI. When visible, users can tap the button to enable or disable the beep sound.

```java
void setBeepButtonVisible(boolean isVisible);
```

**Parameter(s)**

`isVisible`: A boolean value that determines whether to display the beep toggle button.

### isBeepButtonVisible

Returns the visibility status of the beep toggle button.

```java
boolean isBeepButtonVisible();
```

**Return Value**

A boolean value that determines whether the beep toggle button is displayed.

### setVibrateButtonVisible

Sets the visibility of the vibrate toggle button in the scanning UI. When visible, users can tap the button to enable or disable vibration feedback.

```java
void setVibrateButtonVisible(boolean isVisible);
```

**Parameter(s)**

`isVisible`: A boolean value that determines whether to display the vibrate toggle button.

### isVibrateButtonVisible

Returns the visibility status of the vibrate toggle button.

```java
boolean isVibrateButtonVisible();
```

**Return Value**

A boolean value that determines whether the vibrate toggle button is displayed.

### setFormatSelectorVisible

Sets the visibility of the document format selector, which allows users to switch between scanning ID cards, passports, or both.

```java
void setFormatSelectorVisible(boolean isVisible);
```

**Parameter(s)**

`isVisible`: A boolean value that determines whether to display the document format selector.

### isFormatSelectorVisible

Returns the visibility status of the document format selector.

```java
boolean isFormatSelectorVisible();
```

**Return Value**

A boolean value that determines whether the document format selector is displayed.

### setReturnDocumentImage

Sets whether to return a cropped document image as part of the scan result. Enabled by default.

```java
void setReturnDocumentImage(boolean returnDocumentImage);
```

**Parameter(s)**

`returnDocumentImage`: A boolean value that determines whether to include the document image in the result.

### isReturnDocumentImage

Returns whether a cropped document image is included in the scan result.

```java
boolean isReturnDocumentImage();
```

**Return Value**

A boolean value that determines whether the document image is returned.

### setReturnOriginalImage

Sets whether to return the original full-frame camera image as part of the scan result. Disabled by default.

```java
void setReturnOriginalImage(boolean returnOriginalImage);
```

**Parameter(s)**

`returnOriginalImage`: A boolean value that determines whether to include the original full-frame image in the result.

### isReturnOriginalImage

Returns whether the original full-frame image is included in the scan result.

```java
boolean isReturnOriginalImage();
```

**Return Value**

A boolean value that determines whether the original image is returned.

### setReturnPortraitImage

Sets whether to return the detected portrait image as part of the scan result. Enabled by default.

```java
void setReturnPortraitImage(boolean returnPortraitImage);
```

**Parameter(s)**

`returnPortraitImage`: A boolean value that determines whether to include the portrait image in the result.

### isReturnPortraitImage

Returns whether the detected portrait image is included in the scan result.

```java
boolean isReturnPortraitImage();
```

**Return Value**

A boolean value that determines whether the portrait image is returned.

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
