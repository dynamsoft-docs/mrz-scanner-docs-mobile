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
| Extras | — | long-press a document image to save it to Photos |

## Project structure

`ScanMRZ` keeps its storyboard, which is the one structural difference from `ScanMRZBasic`. **Main.storyboard** holds a `UINavigationController` whose root view controller is `ViewController`; the user guide's app deletes the storyboard and installs its root in `SceneDelegate` instead.

That navigation controller is what makes the rest possible. The scanner and the result screen are *pushed* rather than presented, so **Re-scan** can pop one screen and **Return Home** can pop to the root.

`ViewController` hides the navigation bar for the home and scanner screens and `ResultViewController` shows it again, so the bar appears only where there is something to title.

## ViewController

### Pushing the scanner

The configuration is identical to the user guide's. The difference is what happens with the result: instead of rendering it in place, `ViewController` converts the images and hands everything to a result screen.

```swift
@objc func buttonTapped() {
    let vc = MRZScannerViewController()
    let config = MRZScannerConfig()
    config.license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"
    vc.config = config

    vc.onScannedResult = { [weak self] result in
        guard let self = self else { return }
        switch result.resultStatus {
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
        case .canceled:
            DispatchQueue.main.async {
                self.label.isHidden = false
                self.label.text = "Scan canceled"
                self.navigationController?.popViewController(animated: true)
            }
        case .exception:
            DispatchQueue.main.async {
                self.label.isHidden = false
                self.label.text = result.errorString
                self.settingsButton.isHidden =
                    result.errorCode != ErrorCode.cameraPermissionDenied.rawValue
                self.navigationController?.popViewController(animated: true)
            }
        default:
            break
        }
    }
    self.label.isHidden = true
    self.settingsButton.isHidden = true
    DispatchQueue.main.async {
        self.navigationController?.pushViewController(vc, animated: true)
    }
}
```

All six images are converted to `UIImage` here and passed as plain properties, so `ResultViewController` never holds the `MRZScanResult`. Four of the six are normally `nil` — `getOriginalImage(_:)` returns nothing unless `returnOriginalImage` is set, and both `.opposite` getters return nothing for a passport. The result screen collapses whatever is missing rather than reserving space for it.

Because the scanner was pushed, both the cancel and error branches pop it. The success branch does not: it pushes the result screen on top, and the scanner is removed from the stack when **Re-scan** pops back to it.

### Recovering from a permission denial

This is the part with no counterpart in `ScanMRZBasic`, and the reason it exists is specific to iOS. The scanner shows its own alert offering **Open Settings**, but changing a privacy setting there **terminates the app**. By the time the user returns, the scanner and its alert are long gone — so the home screen keeps its own route into Settings:

```swift
/// Shown only when the scanner reported a camera-permission denial, since that is the
/// one failure the user can resolve themselves.
let settingsButton = UIButton()

/// The scanner's own alert already offers this, but the message stays on screen after
/// the scanner closes — so the route into Settings has to remain reachable from here.
@objc func openSettingsTapped() {
    guard let url = URL(string: UIApplication.openSettingsURLString) else { return }
    UIApplication.shared.open(url)
}
```

The button appears only for `cameraPermissionDenied`:

```swift
self.settingsButton.isHidden =
    result.errorCode != ErrorCode.cameraPermissionDenied.rawValue
```

A camera withheld by device policy reports [`cameraPermissionRestricted`](../api-reference/error-code.md) instead, and in that state the per-app camera toggle is absent from Settings — so offering the button would be a dead end. The error string still explains the situation.

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
/// Shows each segment only when its own image set came back, so the label above the
/// images always says what they are, and collapses the whole section when no images
/// arrived at all.
private func updateImageSectionVisibility() {
    let hasProcessed = primaryDocumentImage != nil || secondaryDocumentImage != nil
    let hasOriginal = primaryOriginalImage != nil || secondaryOriginalImage != nil
    let hasAnyImage = hasProcessed || hasOriginal

    // With only one segment there is nothing to switch to, so don't leave the selection
    // pointing at a set that isn't there.
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
/// Amber (#FFC107) used to color values whose MRZ check digit failed.
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
        // The symbol is slightly wider than tall, so derive the width from its own aspect
        // ratio. Sitting it on the text baseline rather than the line box matches Android.
        attachment.bounds = CGRect(x: 0, y: font.descender,
                                   width: iconHeight * icon.size.width / icon.size.height,
                                   height: iconHeight)
    }

    result.append(NSAttributedString(string: "  "))
    result.append(NSAttributedString(attachment: attachment))

    label.attributedText = result
    // The icon carries no accessible text and color alone isn't a cue, so spell the
    // failure out for VoiceOver.
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

The raw MRZ text is tappable too, for a reason that is easy to miss:

```swift
// Raw MRZ Text — tappable too, because a line-composite failure can flag the
// raw MRZ when no individual field is failed (e.g. corruption in a field without
// its own check digit like name/nationality/sex).
applyLabel(mrzValueLabel,
           text: data.mrzText,
           status: data.getFieldValidationStatus("mrzText"),
           defaultColor: .lightGray)
```

`mrzText` aggregates whole MRZ lines, so it can report `.failed` when every individual field passes — see [`getFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus).

### Re-scan and Return Home

Both actions are navigation, which is the payoff for pushing rather than presenting:

```swift
@objc private func rescanTapped() {
    navigationController?.popViewController(animated: true)
}

@objc private func returnHomeTapped() {
    navigationController?.popToRootViewController(animated: true)
}
```

**Re-scan** pops back to the scanner, which is still on the stack and resets its own state in `viewWillAppear`. **Return Home** pops all the way to `ViewController`.

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

    // Support for iPad popovers
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
