---
layout: default-layout
title: User Guide - Dynamsoft MRZ Scanner for iOS (Ready to Use UI edition)
description: This is the user guide of Dynamsoft MRZ Scanner for iOS SDK demonstrating the Ready to Use UI.
keywords: user guide, objective-c, oc, swift
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
multiProgrammingLanguage: true
enableLanguageSelection: true
---

# MRZ Scanner User Guide (iOS Edition)

The Dynamsoft MRZ Scanner (iOS Edition) provides a ready-to-use scanning component that lets you add MRZ reading to your app with minimal setup. This guide walks through building a complete MRZ scanning app from scratch using `MRZScannerViewController` — the built-in view controller that handles the camera UI, scanning logic, and result delivery.

> [!IMPORTANT]
> For the full sample code, visit the [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZ).

## Supported Document Types

The SDK supports three ICAO Machine Readable Travel Document (MRTD) formats: **TD1** (ID cards, 3-line MRZ), **TD2** (ID cards, 2-line MRZ), and **TD3** (passports, 2-line MRZ). For a visual reference of each format, see [Supported Document Types](../../shared/supported-document-types.md).

> [!NOTE]
> For support for other MRTD types, contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).

## System Requirements

- Supported OS: **iOS 13** or higher.
- Supported ABI: **arm64** and **x86_64**.
- Development Environment: **Xcode 13** and above (**Xcode 14.1+** recommended).

## Licensing

A valid license key is required to use the SDK. If you are just getting started, request a free 30-day trial license below:

{% include trialLicense.html %}

> [!NOTE]
>
> - The license string above grants a time-limited free trial which requires a network connection.
> - You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=guide&package=ios){:target="_blank"} link.
> - For production license setup, see the [License Activation](license-activation.md) guide.

## Add the SDK

There are two ways in which you can include the `DynamsoftMRZScannerBundle` library in your app:

### Option 1: Add the xcframeworks via Swift Package Manager

1. In your Xcode project, go to **File --> AddPackages**.

2. In the top-right section of the window, search "https://github.com/Dynamsoft/mrz-scanner-spm"

3. Select `mrz-scanner-spm`, choose `Exact version`, enter **3.4.1300**, then click **Add Package**.

4. Check all the **xcframeworks** and add them.

### Option 2: Add the Frameworks via CocoaPods

1. Add the frameworks in your **Podfile**, replace `TargetName` with your real target name.

   ```sh
   target 'TargetName' do
      use_frameworks!

   pod 'DynamsoftMRZScannerBundle','3.4.1300'

   end
   ```

2. Execute the pod command to install the frameworks and generate workspace(**[TargetName].xcworkspace**):

   ```sh
   pod install
   ```

## Building the MRZ Scanner Application

The following steps build the **ScanMRZ** sample app. You can also download the complete project from the [GitHub repo](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZ).

### Step 1: Create a New Project

1. Open Xcode and select **File > New > New Project**.
2. Choose **iOS > App** as the project template.
3. Set the product name to *ScanMRZ*, choose **StoryBoard** as the interface, and select your language (Objective-C or Swift).

### Step 2: Add the SDK

Follow the instructions in the [Add the SDK](#add-the-sdk) section above to add `DynamsoftMRZScannerBundle` to your project.

### Step 3: Set Up the UI

Create the main `ViewController` with a single "Scan an MRZ" button and a label to display status messages. The button is anchored to the bottom of the screen; the label is centered. Scan results will be shown in a separate `ResultViewController`, created in [Step 7](#step-7-display-the-results).

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
#import "ViewController.h"
#import <DynamsoftMRZScannerBundle/DynamsoftMRZScannerBundle.h>
#import <DynamsoftMRZScannerBundle/DynamsoftMRZScannerBundle-Swift.h>
@interface ViewController ()
@property (nonatomic, strong) UIButton *button;
@property (nonatomic, strong) UILabel *label;
@end
@implementation ViewController
- (void)viewDidLoad {
   [super viewDidLoad];
   self.navigationController.navigationBar.hidden = YES;
   self.view.backgroundColor = [UIColor whiteColor];
   [self setup];
   [self setupAppearance];
}
- (void)setup {
   self.button = [UIButton buttonWithType:UIButtonTypeSystem];
   self.button.backgroundColor = [UIColor blackColor];
   [self.button setTitle:@"Scan an MRZ" forState:UIControlStateNormal];
   [self.button setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
   self.button.layer.cornerRadius = 8;
   self.button.clipsToBounds = YES;
   [self.button addTarget:self action:@selector(buttonTapped) forControlEvents:UIControlEventTouchUpInside];
   self.button.translatesAutoresizingMaskIntoConstraints = NO;
   [self.view addSubview:self.button];
   self.label = [[UILabel alloc] init];
   self.label.numberOfLines = 0;
   self.label.textColor = [UIColor blackColor];
   self.label.textAlignment = NSTextAlignmentCenter;
   self.label.font = [UIFont systemFontOfSize:20];
   self.label.translatesAutoresizingMaskIntoConstraints = NO;
   [self.view addSubview:self.label];
   UILayoutGuide *safeArea = self.view.safeAreaLayoutGuide;
   [NSLayoutConstraint activateConstraints:@[
              [self.button.centerXAnchor constraintEqualToAnchor:self.view.centerXAnchor],
              [self.button.bottomAnchor constraintEqualToAnchor:safeArea.bottomAnchor constant:-10],
              [self.button.heightAnchor constraintEqualToConstant:50],
              [self.button.widthAnchor constraintEqualToConstant:150],
              [self.label.centerXAnchor constraintEqualToAnchor:safeArea.centerXAnchor],
              [self.label.centerYAnchor constraintEqualToAnchor:safeArea.centerYAnchor],
              [self.label.leadingAnchor constraintEqualToAnchor:safeArea.leadingAnchor constant:30],
              [self.label.trailingAnchor constraintEqualToAnchor:safeArea.trailingAnchor constant:-30]
   ]];
}
- (void)setupAppearance {
   UINavigationBarAppearance *appearance = [[UINavigationBarAppearance alloc] init];
   [appearance configureWithOpaqueBackground];
   appearance.backgroundColor = [UIColor blackColor];
   appearance.titleTextAttributes = @{NSForegroundColorAttributeName: [UIColor whiteColor]};
   self.navigationController.navigationBar.standardAppearance = appearance;
   self.navigationController.navigationBar.scrollEdgeAppearance = appearance;
   self.navigationController.navigationBar.compactAppearance = appearance;
   self.navigationController.navigationBar.tintColor = [UIColor whiteColor];
}
@end
```
2. 
```swift
import UIKit
import DynamsoftMRZScannerBundle
class ViewController: UIViewController {
   let button = UIButton()
   let label = UILabel()
   override func viewDidLoad() {
          super.viewDidLoad()
          navigationController?.navigationBar.isHidden = true
          view.backgroundColor = .white
          setup()
          setupAppearance()
   }
   func setup() {
          button.backgroundColor = .black
          button.setTitle("Scan an MRZ", for: .normal)
          button.setTitleColor(.white, for: .normal)
          button.layer.cornerRadius = 8
          button.clipsToBounds = true
          button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)
          button.translatesAutoresizingMaskIntoConstraints = false
          view.addSubview(button)
          label.numberOfLines = 0
          label.textColor = .black
          label.textAlignment = .center
          label.font = UIFont.systemFont(ofSize: 20)
          label.translatesAutoresizingMaskIntoConstraints = false
          view.addSubview(label)
          let safeArea = view.safeAreaLayoutGuide
          NSLayoutConstraint.activate([
             button.centerXAnchor.constraint(equalTo: view.centerXAnchor),
             button.bottomAnchor.constraint(equalTo: safeArea.bottomAnchor, constant: -10),
             button.heightAnchor.constraint(equalToConstant: 50),
             button.widthAnchor.constraint(equalToConstant: 150),
             label.centerXAnchor.constraint(equalTo: safeArea.centerXAnchor),
             label.centerYAnchor.constraint(equalTo: safeArea.centerYAnchor),
             label.leadingAnchor.constraint(equalTo: safeArea.leadingAnchor, constant: 30),
             label.trailingAnchor.constraint(equalTo: safeArea.trailingAnchor, constant: -30)
          ])
   }
   func setupAppearance() {
          let appearance = UINavigationBarAppearance()
          appearance.configureWithOpaqueBackground()
          appearance.backgroundColor = .black
          appearance.titleTextAttributes = [.foregroundColor: UIColor.white]
          navigationController?.navigationBar.standardAppearance = appearance
          navigationController?.navigationBar.scrollEdgeAppearance = appearance
          navigationController?.navigationBar.compactAppearance = appearance
          navigationController?.navigationBar.tintColor = UIColor.white
   }
}
```

> [!NOTE]
> We will only have one *ViewController*, where most of the code will be written, along with an associated *NavigationController* to allow the user to navigate back and forth between the home page and the `ResultViewController` where the MRZ data is displayed.

### Step 4: Set Up the Scene Delegate

Since this project uses programmatic UI (no storyboard), you need to configure `SceneDelegate.swift` to set up the `UINavigationController` as the root. Replace the default `func scene` body with the following:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
- (void)scene:(UIScene *)scene willConnectToSession:(UISceneSession *)session options:(UISceneConnectionOptions *)connectionOptions {
   UIWindowScene *windowScene = (UIWindowScene *)scene;
   if (!windowScene) return;
   UIWindow *window = [[UIWindow alloc] initWithWindowScene:windowScene];
   ViewController *rootViewController = [[ViewController alloc] init];
   UINavigationController *navigationController = [[UINavigationController alloc] initWithRootViewController:rootViewController];
   window.rootViewController = navigationController;
   self.window = window;
   [window makeKeyAndVisible];
}
```
2. 
```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
   guard let windowScene = (scene as? UIWindowScene) else { return }
   let window = UIWindow(windowScene: windowScene)
   let rootViewController = ViewController()
   let navigationController = UINavigationController(rootViewController: rootViewController)
   window.rootViewController = navigationController
   self.window = window
   window.makeKeyAndVisible()
}
```

### Step 5: Configure the Scanner

This step implements the `buttonTapped` action wired to the button in Step 3 — without it, the project will not compile. Create an `MRZScannerViewController` and configure it with an `MRZScannerConfig`. 

The only required setting is the license key — see the [Licensing](#licensing) section above for how to obtain one. For the full list of optional settings such as document type filtering, UI button visibility, and image capture options, see the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
/* Add to ViewController.swift */
- (void)buttonTapped {
   DSMRZScannerViewController *vc = [[DSMRZScannerViewController alloc] init];
   DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
   // Required: set a valid license key.
   config.license = @"DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9";
   vc.config = config;
}
```
2. 
```swift
/* Add to ViewController.swift */
@objc func buttonTapped() {
   let vc = MRZScannerViewController()
   let config = MRZScannerConfig()
   // Required: set a valid license key.
   config.license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"
   vc.config = config
}
```

### Step 6: Launch the Scanner

Wire the scan button to `MRZScannerViewController` and handle results via the `onScannedResult` callback. Each result carries a `resultStatus` of *finished* (MRZ decoded), *canceled* (user closed the scanner), or *exception* (an error occurred).

Continuing from Step 5:

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
/* ViewController.swift — continue the buttonTapped method from Step 5 */
- (void)buttonTapped {
   DSMRZScannerViewController *vc = [[DSMRZScannerViewController alloc] init];
   DSMRZScannerConfig *config = [[DSMRZScannerConfig alloc] init];
   config.license = @"DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9";
   vc.config = config;
   __weak typeof(self) weakSelf = self;
   vc.onScannedResult = ^(DSMRZScanResult *result) {
              switch (result.resultStatus) {
                 /* if the result is valid, navigate to ResultViewController */
                 case DSResultStatusFinished: {
                    if (result.data) {
                       dispatch_async(dispatch_get_main_queue(), ^{
                          ResultViewController *resultVC = [[ResultViewController alloc] init];
                          resultVC.mrzData = result.data;
                          NSError *error = nil;
                          resultVC.portraitImage = [[result getPortraitImage] toUIImageAndReturnError:&error];
                          resultVC.primaryDocumentImage = [[result getDocumentImage:DSDocumentSideMrz] toUIImageAndReturnError:&error];
                          resultVC.primaryOriginalImage = [[result getOriginalImage:DSDocumentSideMrz] toUIImageAndReturnError:&error];
                          resultVC.secondaryDocumentImage = [[result getDocumentImage:DSDocumentSideOpposite] toUIImageAndReturnError:&error];
                          resultVC.secondaryOriginalImage = [[result getOriginalImage:DSDocumentSideOpposite] toUIImageAndReturnError:&error];
                          [weakSelf.navigationController pushViewController:resultVC animated:YES];
                       });
                    }
                    break;
                 }
                 /* if the scan operation is canceled by the user */
                 case DSResultStatusCanceled: {
                    dispatch_async(dispatch_get_main_queue(), ^{
                       weakSelf.label.isHidden = NO;
                       weakSelf.label.text = @"Scan canceled";
                       [weakSelf.navigationController popViewControllerAnimated:YES];
                    });
                    break;
                 }
                 /* if an error occurs during capture, display the error string in the label */
                 case DSResultStatusException: {
                    dispatch_async(dispatch_get_main_queue(), ^{
                       weakSelf.label.isHidden = NO;
                       weakSelf.label.text = result.errorString;
                       [weakSelf.navigationController popViewControllerAnimated:YES];
                    });
                    break;
                 }
                 default:
                    break;
              }
   };
   self.label.isHidden = YES;
   dispatch_async(dispatch_get_main_queue(), ^{
              [weakSelf.navigationController pushViewController:vc animated:YES];
   });
}
```
2. 
```swift
/* ViewController.swift — continue the buttonTapped method from Step 5 */
@objc func buttonTapped() {
   let vc = MRZScannerViewController()
   let config = MRZScannerConfig()
   config.license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"
   vc.config = config
   vc.onScannedResult = { [weak self] result in
          guard let self = self else { return }
          switch result.resultStatus {
             /* if the result is valid, navigate to ResultViewController */
             case .finished:
                if let data = result.data {
                   DispatchQueue.main.async {
                      let resultVC = ResultViewController()
                      resultVC.mrzData = data
                      resultVC.portraitImage = try? result.getPortraitImage()?.toUIImage()
                      resultVC.primaryDocumentImage = try? result.getDocumentImage(.mrz)?.toUIImage()
                      resultVC.primaryOriginalImage = try? result.getOriginalImage(.mrz)?.toUIImage()
                      resultVC.secondaryDocumentImage = try? result.getDocumentImage(.opposite)?.toUIImage()
                      resultVC.secondaryOriginalImage = try? result.getOriginalImage(.opposite)?.toUIImage()
                      self.navigationController?.pushViewController(resultVC, animated: true)
                   }
                }
             /* if the scan operation is canceled by the user */
             case .canceled:
                DispatchQueue.main.async {
                   self.label.isHidden = false
                   self.label.text = "Scan canceled"
                   self.navigationController?.popViewController(animated: true)
                }
             /* if an error occurs during capture, display the error string in the label */
             case .exception:
                DispatchQueue.main.async {
                   self.label.isHidden = false
                   self.label.text = result.errorString
                   self.navigationController?.popViewController(animated: true)
                }
             default:
                break
          }
   }
   self.label.isHidden = true
   DispatchQueue.main.async {
              self.navigationController?.pushViewController(vc, animated: true)
   }
}
```

> [!NOTE]
>
> - `DocumentSide.mrz` refers to the side of the document containing the machine-readable zone. `DocumentSide.opposite` refers to the reverse side, which is relevant for two-sided documents such as TD1 ID cards.
> - Image retrieval methods on `MRZScanResult` (`getDocumentImage()`, `getOriginalImage()`, `getPortraitImage()`) return `nil` if the corresponding option was disabled in the config or if no image was captured for that side.

### Step 7: Display the Results

In Xcode, add a new file to your project (**File > New > File**), choose **Swift File** (or **Objective-C File** for an Objective-C project), name it `ResultViewController`, and save it in the same group as `ViewController.swift`.

`ResultViewController` receives and displays the scan result. The *canceled* and *exception* statuses are already handled inline in Step 6's callback, so `ResultViewController` only needs to handle the *finished* status. It provides **Re-scan** and **Return Home** actions.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
/* ResultViewController.h */
#import <UIKit/UIKit.h>
#import <DynamsoftMRZScannerBundle/DynamsoftMRZScannerBundle-Swift.h>
@interface ResultViewController : UIViewController
// Data properties — set by ViewController before pushing
@property (nonatomic, strong) DSMRZData *mrzData;
@property (nonatomic, strong) UIImage *portraitImage;
@property (nonatomic, strong) UIImage *primaryDocumentImage;
@property (nonatomic, strong) UIImage *primaryOriginalImage;
@property (nonatomic, strong) UIImage *secondaryDocumentImage;
@property (nonatomic, strong) UIImage *secondaryOriginalImage;
@end
/* ResultViewController.m */
#import "ResultViewController.h"
@interface ResultViewController ()
// UI components
@property (nonatomic, strong) UILabel *nameLabel;
@property (nonatomic, strong) UILabel *subInfoLabel;
@property (nonatomic, strong) UIImageView *portraitImageView;
@property (nonatomic, strong) UIImageView *primaryImageView;
@property (nonatomic, strong) UIImageView *secondaryImageView;
@property (nonatomic, strong) UILabel *mrzTextLabel;
@property (nonatomic, strong) UIButton *rescanButton;
@property (nonatomic, strong) UIButton *returnHomeButton;
@end
@implementation ResultViewController
- (void)viewDidLoad {
   [super viewDidLoad];
   self.view.backgroundColor = [UIColor blackColor];
   self.title = @"Result";
   [self setupUI];
   [self populateData];
}
- (void)populateData {
   if (!self.mrzData) return;
   // Personal info header
   self.nameLabel.text = [NSString stringWithFormat:@"%@ %@", self.mrzData.firstName, self.mrzData.lastName];
   self.subInfoLabel.text = [NSString stringWithFormat:@"%@, %ld years old\nExpiry: %@",
          [self.mrzData.sex capitalizedString], (long)self.mrzData.age, self.mrzData.dateOfExpire];
   // Portrait image
   if (self.portraitImage) {
          self.portraitImageView.image = self.portraitImage;
   }
   // Document images
   self.primaryImageView.image = self.primaryDocumentImage;
   self.secondaryImageView.image = self.secondaryDocumentImage;
   // Raw MRZ text
   self.mrzTextLabel.text = self.mrzData.mrzText;
}
- (void)rescanTapped {
   [self.navigationController popViewControllerAnimated:YES];
}
- (void)returnHomeTapped {
   [self.navigationController popToRootViewControllerAnimated:YES];
}
@end
```
2. 
```swift
import UIKit
import DynamsoftMRZScannerBundle
class ResultViewController: UIViewController {
   // Data properties — set by ViewController before pushing
   var mrzData: MRZData?
   var portraitImage: UIImage?
   var primaryDocumentImage: UIImage?
   var primaryOriginalImage: UIImage?
   var secondaryDocumentImage: UIImage?
   var secondaryOriginalImage: UIImage?
   // UI components
   let nameLabel = UILabel()
   let subInfoLabel = UILabel()
   let portraitImageView = UIImageView()
   let primaryImageView = UIImageView()
   let secondaryImageView = UIImageView()
   let mrzTextLabel = UILabel()
   let rescanButton = UIButton(type: .system)
   let returnHomeButton = UIButton(type: .system)
   override func viewDidLoad() {
              super.viewDidLoad()
              view.backgroundColor = .black
              title = "Result"
              setupUI()
              populateData()
   }
   private func populateData() {
              guard let data = mrzData else { return }
              // Personal info header
              nameLabel.text = "\(data.firstName) \(data.lastName)"
              subInfoLabel.text = "\(data.sex.capitalized), \(data.age) years old\nExpiry: \(data.dateOfExpire)"
              // Portrait image
              if let portrait = portraitImage {
                 portraitImageView.image = portrait
              }
              // Document images
              primaryImageView.image = primaryDocumentImage
              secondaryImageView.image = secondaryDocumentImage
              // Raw MRZ text
              mrzTextLabel.text = data.mrzText
   }
   @objc private func rescanTapped() {
              navigationController?.popViewController(animated: true)
   }
   @objc private func returnHomeTapped() {
              navigationController?.popToRootViewController(animated: true)
   }
}
```

> [!NOTE]
>
> - `primaryDocumentImage` and `secondaryDocumentImage` correspond to the MRZ side and opposite side of the document respectively, as retrieved via `getDocumentImage(.mrz)` and `getDocumentImage(.opposite)` in Step 6.
> - When no portrait image is available, use a placeholder — the sample uses a bundled asset named `"user"`. Add your own placeholder image to your asset catalog and reference it as `UIImage(named: "yourPlaceholder")` / `[UIImage imageNamed:@"yourPlaceholder"]`.
> - For the complete `ResultViewController` implementation including the full UI layout, segmented image switcher, and info sections, refer to the [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZ).

For the full list of fields available on `MRZData`, see the [MRZData API reference](../api-reference/mrz-data.md).

### Step 8: Run the Project

Before running, complete these two required configuration steps in Xcode:

1. **Configure Signing** — In the project navigator, select your project, go to the **Signing & Capabilities** tab, and set a valid **Team**. Without this the project will fail to build.

2. **Add Camera Permission** — Select your target, go to the **Info** tab, and add the **Privacy - Camera Usage Description** key with a description string (e.g. `"This app uses the camera to scan MRZ documents."`). Without this the app will crash immediately when the camera is opened, even on a successful build.

Once both are configured, connect a physical iOS device, select it from the top bar, and click **Run**. When the scanner finishes, the result is passed to `ResultViewController`, where the extracted MRZ data and any captured images are displayed.

> [!NOTE]
> If you try running the project on a simulator, you will encounter errors as this sample uses the device camera which is unavailable when using the simulator.

## Next Steps

- **Samples** — Explore the complete [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZ).
- **Customize** — Learn how to configure document type, UI elements, and feedback in the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.
- **API Reference** — Browse the full [iOS API Reference](../api-reference/index.md) for all classes and methods.
- **License** — See the [License Activation](license-activation.md) guide for production license setup.
- **Support** — Contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/) for help or custom requirements.
