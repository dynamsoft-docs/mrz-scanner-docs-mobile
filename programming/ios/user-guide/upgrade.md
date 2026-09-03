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

## From v3.4.x to v3.6.x

### Update the Libraries

You can include the `DynamsoftMRZScannerBundle` library in your app in two ways:

#### Option 1: Add the xcframeworks via Swift Package Manager

1. In your Xcode project, go to **File > Add Packages**.

2. In the search field at the top right of the window, enter `https://github.com/Dynamsoft/mrz-scanner-spm`.

3. Select **mrz-scanner-spm**, choose **Exact Version**, enter the version number, then click **Add Package**.

4. Check all the **xcframeworks** and add them.

#### Option 2: Add the Frameworks via CocoaPods

1. Add the frameworks to your **Podfile**, replacing `TargetName` with your real target name:

   ```sh
   target 'TargetName' do
      use_frameworks!
      pod 'DynamsoftMRZScannerBundle', '{version-number}'
   end
   ```

   > [!NOTE] See [Add the SDK](index.md#add-the-sdk) in the user guide for the correct version number.

2. Run the pod command to install the frameworks and generate the workspace (**[TargetName].xcworkspace**):

   ```sh
   pod install
   ```

### Handle Behavior Changes

No public API was removed or renamed in 3.6.x, so an existing integration keeps compiling. Three changes to how the scanner *behaves* can still affect it.

#### Results That Fail Check-Digit Validation Are Now Delivered

Through 3.4.x, the scanner discarded any result whose MRZ lines failed check-digit validation and simply carried on scanning. A damaged, misread, or altered document produced **no result at all** — from the app's point of view the scan never finished.

3.6.x delivers the result and reports the failure per field instead:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
DSValidationStatus status = [data getFieldValidationStatus:@"documentNumber"];
if (status == DSValidationStatusFailed) {
        // The value is present but disagrees with its check digit.
}
```
2. 
```swift
let status = data.getFieldValidationStatus("documentNumber")
if status == .failed {
        // The value is present but disagrees with its check digit.
}
```

Any code that assumed every delivered result was check-digit-clean now has to make that check explicitly. See [Reading a field's validation status](index.md#step-6-launch-the-scanner-and-show-the-result) in the user guide, and [`getFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus) for the accepted field names.

#### Camera Access Is Now Gated and Reported

Through 3.4.x the scanner opened the camera unconditionally. With access denied, no frames ever arrived, nothing was reported, and the user was left on a blank preview indefinitely.

3.6.x checks the authorization status first, never opens the camera without access, and reports the outcome as `.exception` carrying [`cameraPermissionDenied`](../api-reference/error-code.md) (1001) or [`cameraPermissionRestricted`](../api-reference/error-code.md) (1002).

Two things to check in existing code:

- **Handle `.exception`.** A `switch` that ignored it, or let it fall through a `default` case, will now silently swallow a permission denial that the SDK is reporting properly.
- **If your app already presents its own permission UI**, suppress the scanner's alert so the user does not get two of them:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
config.isCameraPermissionPromptEnabled = NO;
```
2. 
```swift
config.isCameraPermissionPromptEnabled = false
```

The denial is still reported either way. See [Handling Camera Permission](customize-mrz-scanner.md#handling-camera-permission) for the full flow.

#### The Scan Region Is Now the Guide Frame

Through 3.4.x the whole camera preview was analyzed, so a document held outside the guide frame could still be read. 3.6.x limits capture to the area inside the frame, which is what the frame appeared to promise all along.

If you relied on the wider area — or your users are used to aiming loosely — hiding the guide frame lifts the restriction back to the whole preview. Note that it also hides the prompt text and costs more per frame to process; see [Hiding the guide frame](customize-mrz-scanner.md#hiding-the-guide-frame).

### Adopt the New APIs

These are additive, so adopting them is optional:

- [`getFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus) — per-field check-digit status.
- [`DSMRZErrorCode`](../api-reference/error-code.md) — the bundle's own error codes, currently both about camera access.
- [`isCameraPermissionPromptEnabled`](../api-reference/mrz-scanner-config.md#iscamerapermissionpromptenabled) — suppress the built-in permission alert.

Your users will also notice two additions to the scanner UI that need no code from you: a progress spinner while MRZ-like text is being processed, and a flip prompt for TD1 and TD2 ID cards whose portrait is on the opposite side. Both are described in [The Scanner Screen](index.md#the-scanner-screen).

## From v3.2.x to v3.4.x

### Update the Libraries

You can include the `DynamsoftMRZScannerBundle` library in your app in two ways:

#### Option 1: Add the xcframeworks via Swift Package Manager

1. In your Xcode project, go to **File > Add Packages**.

2. In the search field at the top right of the window, enter `https://github.com/Dynamsoft/mrz-scanner-spm`.

3. Select **mrz-scanner-spm**, choose **Up to Next Major Version**, then click **Add Package**.

4. Check all the **xcframeworks** and add them.

#### Option 2: Add the Frameworks via CocoaPods

1. Add the frameworks to your **Podfile**, replacing `TargetName` with your real target name:

   ```sh
   target 'TargetName' do
      use_frameworks!
      pod 'DynamsoftMRZScannerBundle', '{version-number}'
   end
   ```

   > [!NOTE] See [Add the SDK](index.md#add-the-sdk) in the user guide for the correct version number.

2. Run the pod command to install the frameworks and generate the workspace (**[TargetName].xcworkspace**):

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

#### `templateFilePath` Has Been Removed

The `templateFilePath` property is gone from `MRZScannerConfig`. It was deprecated in 2.0.1 in favor of [`templateFile`](../api-reference/mrz-scanner-config.md#templatefile), which accepts either a file path or a JSON string:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
// Before
config.templateFilePath = @"CustomizedTemplate.json";
// After
config.templateFile = @"CustomizedTemplate.json";
```
2. 
```swift
// Before
config.templateFilePath = "CustomizedTemplate.json"
// After
config.templateFile = "CustomizedTemplate.json"
```

> [!NOTE]
> The Android edition still exposes `setTemplateFilePath` and `getTemplateFilePath` as deprecated methods, so cross-platform code that shares a migration checklist will find them present there and absent here.

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
DSImageData *docImage = [result getDocumentImage:DSDocumentSideMrz];
DSImageData *original = [result getOriginalImage:DSDocumentSideMrz];
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

You can include the `DynamsoftMRZScannerBundle` library in your app in two ways:

#### Option 1: Add the xcframeworks via Swift Package Manager

1. In your Xcode project, go to **File > Add Packages**.

2. In the search field at the top right of the window, enter `https://github.com/Dynamsoft/mrz-scanner-spm`.

3. Select **mrz-scanner-spm**, choose **Up to Next Major Version**, then click **Add Package**.

4. Check all the **xcframeworks** and add them.

#### Option 2: Add the Frameworks via CocoaPods

1. Add the frameworks to your **Podfile**, replacing `TargetName` with your real target name:

   ```sh
   target 'TargetName' do
      use_frameworks!
      pod 'DynamsoftMRZScannerBundle', '{version-number}'
   end
   ```

   > [!NOTE] See [Add the SDK](index.md#add-the-sdk) in the user guide for the correct version number.


2. Run the pod command to install the frameworks and generate the workspace (**[TargetName].xcworkspace**):

   ```sh
   pod install
   ```
