---
layout: default-layout
title: MRZScannerConfig Class - Dynamsoft MRZ Scanner iOS Edition
description: MRZScannerConfig of DynamsoftMRZScanner iOS is the class that defines the configurations for MRZ scanning.
keywords: MRZ, scanner, config 
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScannerConfig
---

# MRZScannerConfig

`MRZScannerConfig` is the class that defines the configurations for MRZ scanning. It is set via the `MRZScannerViewController`.

## Definition

*Assembly:* DynamsoftMRZScanner.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@interface DSMRZScannerConfig : NSObject
```
2. 
```swift
class MRZScannerConfig : NSObject
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`license`](#license) | *NSString* | Sets/Returns the license string. |
| [`templateFile`](#templatefile) | *NSString \** | Sets or returns the template with a file path or a JSON string. |
| [`documentType`](#documenttype) | *NSString* | Sets/Returns the document type to scan, such as ID cards or passports. |
| [`isTorchButtonVisible`](#istorchbuttonvisible) | *BOOL* | Sets/Returns the visibility of the torch button. |
| [`isBeepEnabled`](#isbeepenabled) | *BOOL* | Sets/Returns whether the beep sound is enabled when a MRZ is scanned. |
| [`isVibrateEnabled`](#isvibrateenabled) | *BOOL* | Sets/Returns whether the vibration is enabled when a MRZ is scanned. |
| [`isCloseButtonVisible`](#isclosebuttonvisible) | *BOOL* | Sets/Returns the visibility of the close button. |
| [`isGuideFrameVisible`](#isguideframevisible) | *BOOL* | Sets/Returns the visibility of the guide frame on the display. |
| [`isCameraToggleButtonVisible`](#iscameratogglebuttonvisible) | *BOOL* | Sets/Returns the visibility of the camera switch button. |
| [`isBeepButtonVisible`](#isbeepbuttonvisible) | *BOOL* | Sets/Returns the visibility of the beep toggle button. |
| [`isVibrateButtonVisible`](#isvibratebuttonvisible) | *BOOL* | Sets/Returns the visibility of the vibrate toggle button. |
| [`isFormatSelectorVisible`](#isformatselectorvisible) | *BOOL* | Sets/Returns the visibility of the document format selector. |
| [`returnDocumentImage`](#returndocumentimage) | *BOOL* | Sets/Returns whether to return a cropped document image in the scan result. |
| [`returnPortraitImage`](#returnportraitimage) | *BOOL* | Sets/Returns whether to return a cropped portrait image in the scan result. |
| [`returnOriginalImage`](#returnoriginalimage) | *BOOL* | Sets/Returns whether to return the original frame image in the scan result. |

### license

Sets or returns the license string.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) NSString* license;
```
2. 
```swift
var license: String { get set }
```

### templateFile

Sets or returns the template with a file path or a JSON string.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) NSString* templateFile;
```
2. 
```swift
var templateFile: String? { get set }
```

### documentType

Sets or returns the document type to scan, such as ID cards or passports.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) DSDocumentType documentType;
```
2. 
```swift
var documentType: DocumentType { get set }
```

### isTorchButtonVisible

Sets or returns the visibility of the torch button. Users can click the torch button to turn on/off the torch.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isTorchButtonVisible;
```
2. 
```swift
var isTorchButtonVisible: Bool { get set }
```

### isBeepEnabled

Sets or returns whether the beep sound is enabled when a MRZ is scanned.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isBeepEnabled;
```
2. 
```swift
var isBeepEnabled: Bool { get set }
```

### isVibrateEnabled

Sets or returns whether the vibration is enabled when a MRZ is scanned.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isVibrateEnabled;
```
2. 
```swift
var isVibrateEnabled: Bool { get set }
```

### isCloseButtonVisible

Sets or returns the visibility of the close button.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isCloseButtonVisible;
```
2. 
```swift
var isCloseButtonVisible: Bool { get set }
```

### isGuideFrameVisible

Sets or returns the visibility of the guide frame on the display.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isGuideFrameVisible;
```
2. 
```swift
var isGuideFrameVisible: Bool { get set }
```

### isCameraToggleButtonVisible

Sets or returns the visibility of the camera switch button that allows users to switch between the rear and front cameras.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isCameraToggleButtonVisible;
```
2. 
```swift
var isCameraToggleButtonVisible: Bool { get set }
```

### isBeepButtonVisible

Sets or returns the visibility of the beep toggle button. Users can click the beep button to turn on/off the beep sound.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isBeepButtonVisible;
```
2. 
```swift
var isBeepButtonVisible: Bool { get set }
```

### isVibrateButtonVisible

Sets or returns the visibility of the vibrate toggle button. Users can click the vibrate button to turn on/off the vibration.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isVibrateButtonVisible;
```
2. 
```swift
var isVibrateButtonVisible: Bool { get set }
```

### isFormatSelectorVisible

Sets or returns the visibility of the document format selector that allows users to switch between scanning modes (ID cards, passports, or both).

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL isFormatSelectorVisible;
```
2. 
```swift
var isFormatSelectorVisible: Bool { get set }
```

### returnDocumentImage

Sets or returns whether to return a cropped document image in the scan result. Enabled by default.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL returnDocumentImage;
```
2. 
```swift
var returnDocumentImage: Bool { get set }
```

### returnPortraitImage

Sets or returns whether to return a cropped portrait image in the scan result. Enabled by default.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL returnPortraitImage;
```
2. 
```swift
var returnPortraitImage: Bool { get set }
```

### returnOriginalImage

Sets or returns whether to return the original full-frame image in the scan result. Disabled by default.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) BOOL returnOriginalImage;
```
2. 
```swift
var returnOriginalImage: Bool { get set }
```
