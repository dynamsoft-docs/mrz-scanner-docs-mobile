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

The following property is deprecated:

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`templateFilePath`](#templatefilepath) | *NSString* | Sets/Returns the local file path for the JSON parameters template file. |

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
var templateFile: String { get set }
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

### templateFilePath

Sets or returns the local file path for the JSON parameters template file.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property(nonatomic, assign) NSString* templateFilePath;
```
2. 
```swift
var templateFilePath: String { get set }
```
