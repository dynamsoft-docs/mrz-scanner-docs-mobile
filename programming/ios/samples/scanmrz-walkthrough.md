---
layout: default-layout
title: Building the ScanMRZ Demo App - Dynamsoft MRZ Scanner iOS Edition
description: Build the complete ScanMRZ demo app for iOS - its result screen, document image switcher, per-field validation display, and camera-permission recovery.
keywords: demo app, sample, ScanMRZ, result screen, iOS, UIKit
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: ScanMRZ Demo App
noTitleIndex: true
---

# Building the ScanMRZ Demo App

The [MRZ Scanner User Guide](../user-guide/index.md) builds **ScanMRZBasic**, the smallest app that scans an MRZ and shows the parsed data on a single screen. This page builds **ScanMRZ**, the complete demo app on top of it.

> [!NOTE]
> The **MRZ Scanner Demo** published on the App Store is this same implementation behind a more polished landing screen. Building `ScanMRZ` gives you the full scanning and result experience; the store app adds branding around it.

Everything on this page is presentation. It uses the same SDK calls the user guide covers — `MRZScannerConfig`, `MRZScannerViewController`, `onScannedResult`, and the getters on `MRZScanResult` — and adds a result screen around them. None of it is required in order to use the MRZ Scanner.

> [!NOTE]
> Full source on GitHub: [ScanMRZ](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZ){:target="_blank"}. This page covers the UIKit sample; its SwiftUI twin is covered in [Using the Scanner from SwiftUI](swiftui-walkthrough.md). The code below is Swift, as the sample is — there is no Objective-C version of this app.

## What ScanMRZ adds

| | ScanMRZBasic | ScanMRZ |
| --- | --- | --- |
| Screens | one view controller | `ViewController` plus a dedicated `ResultViewController` |
| Navigation | scanner presented modally | `UINavigationController`, scanner and result pushed |
| Result data | seven fields, plain text | full field set, grouped into personal and document sections |
| Images | portrait only | portrait, plus a Processed/Original switcher for both document sides |
| Failed validation | value colored amber | amber, underlined, with an inline icon and an explanatory dialog |
| Permission denial | error string on screen | error string plus an **Open Settings** button |
| Extras | — | long-press the portrait or a document image to save it to Photos |

## Project structure

Neither app has a storyboard — both build their root view controller in `SceneDelegate`. The one structural difference is that `ScanMRZ` wraps its root in a `UINavigationController`:

```swift
guard let windowScene = (scene as? UIWindowScene) else { return }
let window = UIWindow(windowScene: windowScene)
let rootViewController = ViewController()
let navigationController = UINavigationController(rootViewController: rootViewController)
window.rootViewController = navigationController
self.window = window
window.makeKeyAndVisible()
```

That navigation controller is what makes the rest possible. The scanner and the result screen are *pushed* rather than presented, so **Re-scan** can pop one screen and **Return home** can pop to the root.

The navigation bar is toggled per screen: `ViewController` hides it in `viewWillAppear` and `ResultViewController` shows it there, so it appears only where there is something to title. `viewWillAppear` rather than `viewDidLoad`, because the home screen has to hide the bar again every time the result screen is popped off it.

## ViewController

### Pushing the scanner

Only the license is required, exactly as in the user guide. `ScanMRZ` doubles as a catalog of the rest: every remaining `MRZScannerConfig` setting appears commented out and assigned the opposite of its default, so uncommenting a single line is enough to see what that setting changes.

```swift
private typealias ScannedImages = (portrait: UIImage?, primaryDoc: UIImage?,
                                   primaryOrig: UIImage?, secondaryDoc: UIImage?,
                                   secondaryOrig: UIImage?)

@objc func buttonTapped() {
    let config = MRZScannerConfig()
    config.license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"

    //config.documentType = .passport         // Default is .all, which reads both.
    //config.isTorchButtonVisible = false     // Every scanner control is visible by default.
    //config.isBeepEnabled = true             // Scan feedback is off by default.
    //config.returnOriginalImage = true       // The full camera frame is not returned by default.

    let vc = MRZScannerViewController()
    vc.config = config
    vc.onScannedResult = { [weak self] result in
        let images: ScannedImages = (
            portrait:      try? result.getPortraitImage()?.toUIImage(),
            primaryDoc:    try? result.getDocumentImage(.mrz)?.toUIImage(),
            primaryOrig:   try? result.getOriginalImage(.mrz)?.toUIImage(),
            secondaryDoc:  try? result.getDocumentImage(.opposite)?.toUIImage(),
            secondaryOrig: try? result.getOriginalImage(.opposite)?.toUIImage()
        )
        DispatchQueue.main.async { self?.handle(result, images) }
    }

    label.isHidden = true
    settingsButton.isHidden = true
    navigationController?.pushViewController(vc, animated: true)
}
```

[Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) covers what each setting does.

`onScannedResult` arrives off the main thread, so the callback decodes the images there — with `returnOriginalImage` on they are full camera frames, and converting those on the main thread stalls the UI — then hops once, which leaves one place where all three statuses are handled:

```swift
private func handle(_ result: MRZScanResult, _ images: ScannedImages) {
    switch result.resultStatus {
    case .finished:
        guard let data = result.data else {
            report("Scan returned no data")
            return
        }
        let resultVC = ResultViewController()
        resultVC.mrzData = data
        resultVC.portraitImage = images.portrait
        resultVC.primaryDocumentImage = images.primaryDoc
        resultVC.primaryOriginalImage = images.primaryOrig
        resultVC.secondaryDocumentImage = images.secondaryDoc
        resultVC.secondaryOriginalImage = images.secondaryOrig
        navigationController?.pushViewController(resultVC, animated: true)
    case .canceled:
        report("Scan canceled")
    case .exception:
        report(result.errorString ?? "")
        settingsButton.isHidden =
            result.errorCode != ErrorCode.cameraPermissionDenied.rawValue
    @unknown default:
        break
    }
}

private func report(_ message: String) {
    label.text = message
    label.isHidden = false
    navigationController?.popViewController(animated: true)
}
```

All five images reach the result screen as plain `UIImage` properties, so `ResultViewController` never holds the `MRZScanResult`. Three of the five are normally `nil` — `getOriginalImage(_:)` returns nothing unless `returnOriginalImage` is set, and both `.opposite` getters return nothing for a passport. The result screen collapses whatever is missing rather than reserving space for it.

A `.finished` result with no data reports rather than returning silently: the scanner was pushed, so an early `return` would leave the user staring at it with no feedback.

Because the scanner was pushed, the cancel and error branches both pop it, which is what `report` does after leaving its message behind. The success branch does not: it pushes the result screen on top, and the scanner is removed from the stack when **Re-scan** pops back to it.

### Recovering from a permission denial

This is the part with no counterpart in `ScanMRZBasic`, and the reason it exists is specific to iOS. The scanner shows its own alert offering **Open Settings**, but changing a privacy setting there **terminates the app**. By the time the user returns, the scanner and its alert are long gone — so the home screen keeps its own route into Settings:

```swift
private let settingsButton = ViewController.makeStyledButton(title: "Open Settings", fontSize: 16)

@objc func openSettingsTapped() {
    guard let url = URL(string: UIApplication.openSettingsURLString) else { return }
    UIApplication.shared.open(url)
}
```

The `.exception` branch above shows the button only for `cameraPermissionDenied`. A camera withheld by device policy reports [`cameraPermissionRestricted`](../api-reference/error-code.md) instead, and in that state the per-app camera toggle is absent from Settings — so offering the button would be a dead end. The error string still explains the situation.

Suppressing the scanner's own alert entirely is one of the settings in the config catalog: `isCameraPermissionPromptEnabled = false` leaves the denial to `onScannedResult` alone, which is where the button above already comes from.

## ResultViewController

### What it receives

Six plain properties, set before the push:

```swift
var mrzData: MRZData?
var portraitImage: UIImage?
var primaryDocumentImage: UIImage?
var primaryOriginalImage: UIImage?
var secondaryDocumentImage: UIImage?
var secondaryOriginalImage: UIImage?
```

Because these are `UIImage` values rather than `ImageData`, the result screen has no dependency on the SDK for its images. It imports `DynamsoftMRZScannerBundle` only for `MRZData`, and `DynamsoftCaptureVisionBundle` only for `ValidationStatus`.

### The Processed / Original switcher

The two document sides are shown side by side, with a custom segmented control choosing between the cropped ("Processed") and full-frame ("Original") versions. It is built from two buttons with underline views rather than a `UISegmentedControl`, so the selected tab can be underlined:

```swift
@objc private func processedTapped() {
    isProcessedSelected = true
    updateSegmentAppearance()
    updateDocumentImages()
}
```

The interesting part is what happens when a set is missing. With default settings `returnOriginalImage` is `false`, so there is no Original set at all — and a tab leading to an empty view is worse than no tab:

```swift
private func updateImageSectionVisibility() {
    let hasProcessed = primaryDocumentImage != nil || secondaryDocumentImage != nil
    let hasOriginal = primaryOriginalImage != nil || secondaryOriginalImage != nil
    let hasAnyImage = hasProcessed || hasOriginal

    if !(hasProcessed && hasOriginal) {
        isProcessedSelected = hasProcessed
    }
    updateSegmentAppearance()

    processedSegmentStackView.isHidden = !hasProcessed
    originalSegmentStackView.isHidden = !hasOriginal

    segmentContainerView.isHidden = !hasAnyImage
    segmentTopConstraint.constant = hasAnyImage ? 24 : 0
    segmentHeightConstraint.constant = hasAnyImage ? 30 : 0

    imageStackView.isHidden = !hasAnyImage
    imageStackHeightConstraint.constant = hasAnyImage ? 160 : 0
    imageStackTopConstraint.constant = hasAnyImage ? 16 : 0
}
```

Note that hiding is not enough. A hidden view still occupies whatever space its constraints reserve, so the heights and the gaps above them are zeroed as well — which is why those four constraints are held as properties. The two segments themselves are arranged subviews of a stack view, so they collapse on their own.

### Showing a failed field

The user guide colors a failed value amber. `ScanMRZ` goes further: it underlines the value so it reads as tappable, appends an inline amber icon, and opens a dialog explaining what a failed check digit means.

```swift
private static let warningAmber = UIColor(red: 1.0, green: 193.0/255.0, blue: 7.0/255.0, alpha: 1.0)

private static func applyFailedValue(_ text: String, to label: UILabel) {
    let font: UIFont = label.font
    let result = NSMutableAttributedString(string: text, attributes: [
        .underlineStyle: NSUnderlineStyle.single.rawValue,
        .foregroundColor: warningAmber,
        .font: font
    ])

    let iconHeight = font.pointSize * 1.2
    let attachment = NSTextAttachment()
    attachment.image = UIImage(
        systemName: "exclamationmark.circle.fill",
        withConfiguration: UIImage.SymbolConfiguration(pointSize: iconHeight)
    )?.withTintColor(warningAmber, renderingMode: .alwaysOriginal)
    if let icon = attachment.image {
        attachment.bounds = CGRect(x: 0, y: font.descender,
                                   width: iconHeight * icon.size.width / icon.size.height,
                                   height: iconHeight)
    }

    result.append(NSAttributedString(string: "  "))
    result.append(NSAttributedString(attachment: attachment))

    label.attributedText = result
    label.accessibilityLabel = "\(text), validation failed"
}
```

Two details worth copying. The icon is a `NSTextAttachment` sized from the label's own font, so it scales with the text instead of being a fixed-size image. And the `accessibilityLabel` states the failure in words — an amber tint and an icon are both invisible to VoiceOver, so without it the failure simply would not be announced.

Tapping a failed row opens the explanation:

```swift
@objc private func showValidationInfoDialog() {
    let alert = UIAlertController(
        title: "Field validation warning",
        message: "This value doesn't match its check digit. The document may be invalid or altered.",
        preferredStyle: .alert
    )
    alert.addAction(UIAlertAction(title: "OK", style: .default))
    present(alert, animated: true)
}
```

The gesture recognizer is attached only when the status is `.failed`, so rows that passed are not interactive:

```swift
if failed {
    let tap = UITapGestureRecognizer(target: self, action: #selector(showValidationInfoDialog))
    containerView.addGestureRecognizer(tap)
    containerView.isUserInteractionEnabled = true
}
```

### Grouping the fields

`populateData` splits the fields into a summary header, a **Personal Info** section, and a **Document Info** section:

```swift
personalInfoStackView.addArrangedSubview(createInfoRow(label: "Given Name",    value: data.firstName,      status: data.getFieldValidationStatus("firstName")))
personalInfoStackView.addArrangedSubview(createInfoRow(label: "Surname",       value: data.lastName,       status: data.getFieldValidationStatus("lastName")))
personalInfoStackView.addArrangedSubview(createInfoRow(label: "Date of Birth", value: data.dateOfBirth,    status: data.getFieldValidationStatus("dateOfBirth")))
personalInfoStackView.addArrangedSubview(createInfoRow(label: "Gender",        value: genderText,          status: data.getFieldValidationStatus("sex")))
personalInfoStackView.addArrangedSubview(createInfoRow(label: "Nationality",   value: data.nationalityRaw, status: data.getFieldValidationStatus("nationality")))
```

Three decisions in that block are worth noting:

- **The summary header carries no validation highlighting.** `nameLabel` and `subInfoLabel` join several fields into one line each — "gender, age" — and tinting a compound line on one field's status would imply both are invalid. Field-level validation is surfaced in the sections below instead.
- **`Doc. Type` is passed no status.** It is derived from the MRZ layout rather than read from a field with a check digit, so it has nothing to validate against. The sample maps it to friendly text: `data.documentType == "MRTD_TD3_PASSPORT" ? "Passport" : "ID"`.
- **Nationality uses `nationalityRaw`.** That is the three-letter ICAO code as it appears in the MRZ, rather than the expanded country name.

The raw MRZ text is tappable too, for a reason that is easy to miss: a line-composite failure can flag the raw MRZ when no individual field failed — corruption in a field that has no check digit of its own, such as name, nationality or sex.

```swift
applyLabel(mrzValueLabel,
           text: data.mrzText,
           status: data.getFieldValidationStatus("mrzText"),
           defaultColor: .lightGray)
```

`mrzText` aggregates whole MRZ lines, so it can report `.failed` when every individual field passes — see [`getFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus).

### Re-scan and Return home

Both actions are navigation, which is the payoff for pushing rather than presenting:

```swift
@objc private func rescanTapped() {
    navigationController?.popViewController(animated: true)
}

@objc private func returnHomeTapped() {
    navigationController?.popToRootViewController(animated: true)
}
```

**Re-scan** pops back to the scanner, which is still on the stack and resets its own state in `viewWillAppear`. **Return home** pops all the way to `ViewController`.

### Saving an image to Photos

A long press on the portrait or either document image offers to save it:

```swift
@objc private func handleLongPress(_ gesture: UILongPressGestureRecognizer) {
    guard gesture.state == .began,
          let imageView = gesture.view as? UIImageView,
          let image = imageView.image else { return }

    let alert = UIAlertController(title: "Save Image",
                                  message: "Would you like to save this image to your photos?",
                                  preferredStyle: .actionSheet)
    alert.addAction(UIAlertAction(title: "Save", style: .default) { _ in
        UIImageWriteToSavedPhotosAlbum(image, self,
            #selector(self.image(_:didFinishSavingWithError:contextInfo:)), nil)
    })
    alert.addAction(UIAlertAction(title: "Cancel", style: .cancel))

    if let popoverController = alert.popoverPresentationController {
        popoverController.sourceView = imageView
        popoverController.sourceRect = imageView.bounds
    }

    present(alert, animated: true)
}
```

Writing to the photo library needs its own usage description. `ScanMRZ` declares **Privacy - Photo Library Additions Usage Description** (`NSPhotoLibraryAddUsageDescription`) alongside the camera one; without it this action crashes the app the same way a missing camera description does.

Setting `popoverPresentationController.sourceView` is not optional either — an action sheet with no source view raises an exception on iPad.

## Next steps

- [MRZ Scanner User Guide](../user-guide/index.md) — build `ScanMRZBasic` from an empty project.
- [Using the Scanner from SwiftUI](swiftui-walkthrough.md) — the same app in SwiftUI, and the representable bridge.
- [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) — document type, UI elements, feedback, and camera permission.
- [iOS API Reference](../api-reference/index.md) — all classes and methods.
