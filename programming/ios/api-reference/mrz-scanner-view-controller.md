---
layout: default-layout
title: MRZScannerViewController Class - Dynamsoft MRZ Scanner iOS Edition
description: MRZScannerViewController of DynamsoftMRZScanner iOS is an activity class that implements MRZ scanning features.
keywords: scanner, activity, startCapturing, license 
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScannerViewController
---

# Class MRZScannerViewController

`MRZScannerViewController` is an extension of the `ViewController` class that implements MRZ scanning features.

## Definition

*Assembly:* DynamsoftMRZScanner.xcframework

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

## How to Use

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
#import "ViewController.h"
#import <DynamsoftLicense/DynamsoftLicense.h>
#import <DynamsoftMRZScanner/DynamsoftMRZScanner.h>
#import <DynamsoftMRZScanner/DynamsoftMRZScanner-Swift.h>
@interface ViewController ()
@property (nonatomic, strong) UIButton *button;
@property (nonatomic, strong) UILabel *label;
@end
@implementation ViewController
- (void)viewDidLoad {
   [super viewDidLoad];
   [self setup];
}
// Config a button on the UI. Pop the MRZScannerViewController when the button is clicked.
- (void)buttonTapped {
   DSMRZScannerViewController *vc = [[DSMRZScannerViewController alloc] init];
   DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
   config.license = @"DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9";
   vc.config = config;
   __weak typeof(self) weakSelf = self;
   vc.onScannedResult = ^(DSMRZScanResult *result) {
          // The result have 3 status: finished (Successfully decoded), canceled (Quit by clicking close button), exception (error occurs when processing).
          switch (result.resultStatus) {
             case DSResultStatusFinished: {
                    // Add your code to do when the result status is finished.
             }
             case DSResultStatusCanceled: {
                    // Add your code to do when the result status is canceled.
             }
             case DSResultStatusException: {
                    // Add your code to do when the result status is exception.
             }
             default:
                    break;
          }
          dispatch_async(dispatch_get_main_queue(), ^{
             [weakSelf.navigationController popViewControllerAnimated:YES];
          });
   };
   dispatch_async(dispatch_get_main_queue(), ^{
          weakSelf.navigationController.navigationBar.hidden = YES;
          [weakSelf.navigationController pushViewController:vc animated:YES];
   });
}
@end
```
2. 
```swift
import UIKit
import DynamsoftLicense
import DynamsoftMRZScanner
class ViewController: UIViewController {
   let button = UIButton()
   let label = UILabel()
   override func viewDidLoad() {
          super.viewDidLoad()
          setup()
   }
   // Config a button on the UI. Pop the MRZScannerViewController when the button is clicked.
   @objc func buttonTapped() {
          let vc = MRZScannerViewController()
          // Set up the license via the MRZScannerConfig
          let config = MRZScannerConfig()
          config.license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"
          vc.config = config
          // Set up the result callback of MRZScannerViewController.
          vc.onScannedResult = { [weak self] result in
             guard let self = self else { return }
             // The result have 3 status: finished (Successfully decoded), canceled (Quit by clicking close button), exception (error occurs when processing).
             switch result.resultStatus {
             case .finished:
                    // Add your code to do when the result status is finished.
             case .canceled:
                    // Add your code to do when the result status is canceled.
             case .exception:
                    // Add your code to do when the result status is exception.
             @unknown default:
                    break
             }
             DispatchQueue.main.async {
                    self.navigationController?.popViewController(animated: true)
             }
          }
          DispatchQueue.main.async {
             self.navigationController?.navigationBar.isHidden = true
             self.navigationController?.pushViewController(vc, animated: true)
          }
   }
}
```
