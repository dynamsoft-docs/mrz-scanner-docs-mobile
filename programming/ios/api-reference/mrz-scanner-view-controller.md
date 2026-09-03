---
layout: default-layout
title: MRZScannerViewController Class - Dynamsoft MRZ Scanner iOS Edition
description: MRZScannerViewController of DynamsoftMRZScanner iOS is a view controller class that implements MRZ scanning features.
keywords: MRZ, scanner, view controller, onScannedResult, license
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScannerViewController
---

# Class MRZScannerViewController

`MRZScannerViewController` is a `UIViewController` subclass that implements MRZ scanning features. It supplies its own full-screen camera UI, so you present it, hand it an [`MRZScannerConfig`](mrz-scanner-config.md), and receive an [`MRZScanResult`](mrz-scan-result.md) through [`onScannedResult`](#onscannedresult).

## Definition

*Assembly:* DynamsoftMRZScannerBundle.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@interface DSMRZScannerViewController: UIViewController
```
2. 
```swift
class MRZScannerViewController: UIViewController
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`config`](#config) | *DSMRZScannerConfig \** | Sets or returns the MRZ scanner configurations. |
| [`onScannedResult`](#onscannedresult) | *void (^)(DSMRZScanResult *)* | A property that holds a Block. The block is a callback that takes a single parameter of type `DSMRZScanResult` and returns no value. |

## config

Sets or returns the MRZ scanner configurations of type [`DSMRZScannerConfig`](mrz-scanner-config.md).

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, strong, readwrite) DSMRZScannerConfig * config
```
2. 
```swift
var config: MRZScannerConfig = .init()
```

## onScannedResult

A property that holds a Block. The block is a callback that takes a single parameter of type [`DSMRZScanResult`](mrz-scan-result.md) and returns no value.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, copy, readwrite) void (^)(DSMRZScanResult *) onScannedResult
```
2. 
```swift
var onScannedResult: ((MRZScanResult) -> Void)?
```

**Remarks**

Two things about this callback are easy to get wrong:

- **It is not called on the main thread.** Dispatch to the main queue before touching any UI or SwiftUI state.
- **The scanner does not dismiss itself.** Whoever presented `MRZScannerViewController` is responsible for dismissing or popping it when the result arrives.

The images on the result are ARC-managed `ImageData` objects, so they stay valid for as long as you hold the result — there is no window in which they expire and nothing to convert early. Call `toUIImage()` when you want a `UIImage` to hand to a view, or to keep independently of the result.

The callback is invoked exactly once per presentation, for all three values of [`resultStatus`](mrz-scan-result.md#resultstatus) — success, cancellation, and failure all arrive here rather than through separate paths.

## How to Use

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
#import "ViewController.h"
#import <DynamsoftMRZScannerBundle/DynamsoftMRZScannerBundle.h>
#import <DynamsoftMRZScannerBundle/DynamsoftMRZScannerBundle-Swift.h>
@implementation ViewController
// Configure a button that presents MRZScannerViewController when tapped.
- (void)buttonTapped {
   DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
   // Required: set a valid license key.
   config.license = @"DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9";
   // Set the document type to scan (default: DSDocumentTypeAll).
   config.documentType = DSDocumentTypeAll;
   // Configure which images to include in the result.
   config.returnDocumentImage = YES;   // Cropped document image (default: YES).
   config.returnPortraitImage = YES;   // Portrait image (default: YES).
   config.returnOriginalImage = NO;    // Original full-frame image (default: NO).
   // Configure UI element visibility.
   config.isCloseButtonVisible = YES;
   config.isTorchButtonVisible = YES;
   config.isCameraToggleButtonVisible = YES;
   config.isBeepButtonVisible = YES;
   config.isVibrateButtonVisible = YES;
   config.isFormatSelectorVisible = YES;
   config.isGuideFrameVisible = YES;
   // Configure feedback on a successful scan.
   config.isBeepEnabled = YES;
   config.isVibrateEnabled = NO;
   DSMRZScannerViewController *vc = [[DSMRZScannerViewController alloc] init];
   vc.config = config;
   __weak typeof(self) weakSelf = self;
   vc.onScannedResult = ^(DSMRZScanResult *result) {
      // Convert to UIImage here, or later from the result — either works.
      NSError *error = nil;
      UIImage *portrait = [[result getPortraitImage] toUIImage:&error];
      UIImage *mrzSide = [[result getDocumentImage:DSDocumentSideMrz] toUIImage:&error];
      UIImage *oppositeSide = [[result getDocumentImage:DSDocumentSideOpposite] toUIImage:&error];
      // The callback runs off the main thread and the scanner does not close
      // itself, so hop to the main queue and dismiss it here.
      dispatch_async(dispatch_get_main_queue(), ^{
         [weakSelf dismissViewControllerAnimated:YES completion:nil];
         switch (result.resultStatus) {
            case DSResultStatusFinished: {
               // Scan completed successfully. data is nil for the other statuses.
               DSMRZData *data = result.data;
               NSString *mrzText = data.mrzText;
               NSString *firstName = data.firstName;
               // Check a single field against its MRZ check digit.
               DSValidationStatus status = [data getFieldValidationStatus:@"documentNumber"];
               break;
            }
            case DSResultStatusCanceled:
               // The user closed the scanner before completing a scan.
               break;
            case DSResultStatusException: {
               // An error occurred during initialization or scanning.
               NSInteger errorCode = result.errorCode;
               NSString *errorMessage = result.errorString;
               break;
            }
         }
      });
   };
   // The scanner draws its own close button, so present it full screen.
   vc.modalPresentationStyle = UIModalPresentationFullScreen;
   [self presentViewController:vc animated:YES completion:nil];
}
@end
```
2. 
```swift
import UIKit
import DynamsoftMRZScannerBundle
import DynamsoftCaptureVisionBundle
class ViewController: UIViewController {
   // Configure a button that presents MRZScannerViewController when tapped.
   @objc func buttonTapped() {
      let config = MRZScannerConfig()
      // Required: set a valid license key.
      config.license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"
      // Set the document type to scan (default: .all).
      config.documentType = .all
      // Configure which images to include in the result.
      config.returnDocumentImage = true   // Cropped document image (default: true).
      config.returnPortraitImage = true   // Portrait image (default: true).
      config.returnOriginalImage = false  // Original full-frame image (default: false).
      // Configure UI element visibility.
      config.isCloseButtonVisible = true
      config.isTorchButtonVisible = true
      config.isCameraToggleButtonVisible = true
      config.isBeepButtonVisible = true
      config.isVibrateButtonVisible = true
      config.isFormatSelectorVisible = true
      config.isGuideFrameVisible = true
      // Configure feedback on a successful scan.
      config.isBeepEnabled = true
      config.isVibrateEnabled = false
      let vc = MRZScannerViewController()
      vc.config = config
      vc.onScannedResult = { [weak self] result in
         // Convert to UIImage here, or later from the result — either works.
         let portrait = try? result.getPortraitImage()?.toUIImage()
         let mrzSide = try? result.getDocumentImage(.mrz)?.toUIImage()
         let oppositeSide = try? result.getDocumentImage(.opposite)?.toUIImage()
         // The callback runs off the main thread and the scanner does not close
         // itself, so hop to the main queue and dismiss it here.
         DispatchQueue.main.async {
            self?.dismiss(animated: true)
            switch result.resultStatus {
            case .finished:
               // Scan completed successfully. data is nil for the other statuses.
               guard let data = result.data else { return }
               let mrzText = data.mrzText
               let firstName = data.firstName
               // Check a single field against its MRZ check digit.
               let status = data.getFieldValidationStatus("documentNumber")
            case .canceled:
               // The user closed the scanner before completing a scan.
               break
            case .exception:
               // An error occurred during initialization or scanning.
               let errorCode = result.errorCode
               let errorMessage = result.errorString
            @unknown default:
               break
            }
         }
      }
      // The scanner draws its own close button, so present it full screen.
      vc.modalPresentationStyle = .fullScreen
      present(vc, animated: true)
   }
}
```

The example presents the scanner modally, which works in any app. Pushing it onto a `UINavigationController` also works — hide the navigation bar while it is on screen, since the scanner draws its own close button, and pop instead of dismissing. The [ScanMRZ sample](../samples/scanmrz-walkthrough.md) takes that route.
