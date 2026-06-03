---
layout: default-layout
title: How to update - Dynamsoft MRZ Scanner for iOS
description: Follow the upgrade instructions to learn to upgrade MRZ Scanner SDK iOS edition.
keywords: updates guide, ios
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
multiProgrammingLanguage: true
enableLanguageSelection: true
---

# How to Upgrade

## From v3.2.x to v3.4.x

### Update the Libraries

There are 2 ways in which you can include the `DynamsoftMRZScannerBundle` library in your app:

#### Option 1: Add the xcframeworks via Swift Package Manager

1. In your Xcode project, go to **File --> AddPackages**.

2. In the top-right section of the window, search "https://github.com/Dynamsoft/mrz-scanner-spm"

3. Select `mrz-scanner-spm`, choose `Up to Next Major Version`, then click **Add Package**.

4. Check all the **xcframeworks** and add.

#### Option 2: Add the Frameworks via CocoaPods

1. Add the frameworks in your **Podfile**, replace `TargetName` with your real target name.

   ```sh
   target 'ScanMRZ' do
      use_frameworks!

   pod 'DynamsoftMRZScannerBundle','{version-number}'

   end
   ```

   > [!NOTE] Please view [user guide](index.md#option-2-add-the-frameworks-via-cocoapods) for the correct version number.

2. Execute the pod command to install the frameworks and generate workspace(**[TargetName].xcworkspace**):

   ```sh
   pod install
   ```

### Handle Breaking Changes

#### `MRZScanResult.data` Is Now Nullable

The `data` property on [`MRZScanResult`](../api-reference/mrz-scan-result.md) is now declared `nullable`. In Swift this becomes a compile error if you access it directly without unwrapping; in Objective-C it generates a nullability warning.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
// Before
NSString *firstName = result.data.firstName;

// After
if (result.data != nil) {
        NSString *firstName = result.data.firstName;
}
```
2. 
```swift
// Before
let firstName = result.data.firstName

// After
guard let data = result.data else { return }
let firstName = data.firstName
```

#### `errorMessage` Renamed to `errorString`

The `errorMessage` property on [`MRZScanResult`](../api-reference/mrz-scan-result.md) has been renamed to `errorString`. Update any references in your code:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
// Before
NSLog(@"%@", result.errorMessage);

// After
NSLog(@"%@", result.errorString);
```
2. 
```swift
// Before
print(result.errorMessage)

// After
print(result.errorString)
```

### Adopt the New Image Capture APIs

v3.4.x adds the ability to retrieve captured images alongside the parsed MRZ data. Three types of images are available via [`MRZScanResult`](../api-reference/mrz-scan-result.md):

- **Document image** — a cropped, perspective-corrected image of the document. Enabled by default.
- **Portrait image** — the portrait extracted from the document. Enabled by default.
- **Original image** — the raw full-frame camera capture. Disabled by default.

Control which images are returned using the new [`MRZScannerConfig`](../api-reference/mrz-scanner-config.md) properties:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
config.returnDocumentImage = YES; // default: YES
config.returnPortraitImage = YES; // default: YES
config.returnOriginalImage = NO;  // default: NO — opt in to enable
```
2. 
```swift
let config = MRZScannerConfig()
config.returnDocumentImage = true  // default: true
config.returnPortraitImage = true  // default: true
config.returnOriginalImage = false // default: false — opt in to enable
```

Retrieve the images from the scan result:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSImageData *portrait = [result getPortraitImage];
DSImageData *docImage = [result getDocumentImage:DSDocumentSideMRZ];
DSImageData *original = [result getOriginalImage:DSDocumentSideMRZ];

// For two-sided ID cards, also retrieve the opposite side:
DSImageData *opposite = [result getDocumentImage:DSDocumentSideOpposite];
```
2. 
```swift
let portrait = result.getPortraitImage()
let docImage = result.getDocumentImage(.mrz)
let original = result.getOriginalImage(.mrz)

// For two-sided ID cards, also retrieve the opposite side:
let opposite = result.getDocumentImage(.opposite)
```

> [!NOTE] All three methods return `nil` if the corresponding return flag is disabled or the image was not captured. `getDocumentImage(.opposite)` and `getOriginalImage(.opposite)` also return `nil` for single-sided documents such as passports.

## From v2 to v3

### Update the Libraries

There are 2 ways in which you can include the `DynamsoftMRZScannerBundle` library in your app:

#### Option 1: Add the xcframeworks via Swift Package Manager

1. In your Xcode project, go to **File --> AddPackages**.

2. In the top-right section of the window, search "https://github.com/Dynamsoft/mrz-scanner-spm"

3. Select `mrz-scanner-spm`, choose `Up to Next Major Version`, then click **Add Package**.

4. Check all the **xcframeworks** and add.

#### Option 2: Add the Frameworks via CocoaPods

1. Add the frameworks in your **Podfile**, replace `TargetName` with your real target name.

   ```sh
   target 'ScanMRZ' do
      use_frameworks!

   pod 'DynamsoftMRZScannerBundle','{version-number}'

   end
   ```

   > [!NOTE] Please view [user guide](index.md#option-2-add-the-frameworks-via-cocoapods) for the correct version number.


2. Execute the pod command to install the frameworks and generate workspace(**[TargetName].xcworkspace**):

   ```sh
   pod install
   ```
