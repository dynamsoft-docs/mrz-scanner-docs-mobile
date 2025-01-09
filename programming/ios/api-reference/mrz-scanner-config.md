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

`MRZScannerConfig` is the class that defines the configurations for MRZ scanning. It is set via the input value of `MRZScannerActivity.ResultContract`

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
| [`license`](#license) | *NSString* | Sets/Gets the license string. |
| [`templateFilePath`](#templatefilepath) | *NSString* | Sets/Gets a path for the SDK to load template file. |
| [`documentType`](#documenttype) | *NSString* | Sets/Gets the document type to scan. |
| [`isTorchButtonVisible`](#istorchbuttonvisible) | *BOOL* | Sets/Gets the visibility of the torch button. |
| [`isBeepEnabled`](#isbeepenabled) | *BOOL* | Sets/Gets whether to trigger a beep sound when a MRZ is scanned. |
| [`isCloseButtonVisible`](#isclosebuttonvisible) | *BOOL* | Sets/Gets the visibility of the close button. |
| [`isGuideFrameVisible`](#isguideframevisible) | *BOOL* | Sets/Gets the visibility of the guide frame. |

### license

Sets or gets the license string.

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

### templateFilePath

Sets or gets a path for the SDK to load template file.

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

### documentType

Sets or gets the document type to scan.

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

Sets or gets the visibility of the torch button.

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

Sets or gets the status of the beep sound setting.

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

### isCloseButtonVisible

Sets or gets the visibility of the close button.

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

Sets or gets the visibility of the guide frame.

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
