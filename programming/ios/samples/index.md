---
layout: default-layout
title: Demo & Samples - Dynamsoft MRZ Scanner iOS Edition
description: The index of Dynamsoft MRZ Scanner iOS demo & samples.
keywords: demo, sample, index, iOS, UIKit, SwiftUI
needAutoGenerateSidebar: true
noTitleIndex: false
---

# Demo and Samples

## MRZ Scanner Demo

- [View in the App Store](https://apps.apple.com/us/app/dynamsoft-mrz-scanner/id6736854735){:target="_blank"}

## Samples

Four samples scan the MRZ on a passport or ID card using the ready-to-use `MRZScannerViewController`. They come in two pairs — a minimal one and a complete one — each available in UIKit and SwiftUI.

Unlike the Android samples, each iOS sample is a self-contained Xcode project: open the one you want directly rather than a shared workspace. Each declares its own bundle identifier, so all four can be installed on one device and compared side by side.

### ScanMRZBasic

The smallest thing that scans an MRZ and shows the result: one screen that presents the scanner, handles all three result statuses, and renders the parsed fields and the portrait below the button. This is the app the [MRZ Scanner User Guide](../user-guide/index.md) builds step by step.

- [Swift (UIKit)](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZBasic){:target="_blank"}
- [Swift (SwiftUI)](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZBasicSwiftUI){:target="_blank"}

`ScanMRZBasic` has no storyboard and no navigation controller — it presents the scanner modally and dismisses it when the result arrives. `ScanMRZBasicSwiftUI` does the same through a `.fullScreenCover`.

### ScanMRZ

A complete app built on the same SDK calls, adding a dedicated result screen, a processed/original switcher for the document images from both sides, per-field validation explanations, and camera-permission recovery. The [ScanMRZ Demo App](scanmrz-walkthrough.md) goes through it section by section.

- [Swift (UIKit)](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZ){:target="_blank"}
- [Swift (SwiftUI)](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZSwiftUI){:target="_blank"}

Within each pair the SwiftUI version is a port of the UIKit one with the same screens, result layout, and scanner flow, so pick whichever UI framework matches your project.

> [!NOTE]
> Both SwiftUI samples require **iOS 16** or later — newer than the SDK's own iOS 13 minimum — because they use the SwiftUI app lifecycle. The UIKit samples target iOS 13.

Both SwiftUI samples wrap `MRZScannerViewController` in a `UIViewControllerRepresentable` called `MRZScannerView`. That bridge is the pattern to copy when adding the scanner to a SwiftUI app of your own; the user guide walks through it in [Using the Scanner from SwiftUI](../user-guide/index.md#using-the-scanner-from-swiftui).

> [!NOTE]
> A physical device is required to run any of the samples. The iOS Simulator does not expose a camera.

## Next Steps

- [MRZ Scanner User Guide](../user-guide/index.md) — build `ScanMRZBasic` from an empty project.
- [ScanMRZ Demo App](scanmrz-walkthrough.md) — how the complete sample builds its result screen.
- [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) — document type, UI elements, feedback, and camera permission.
- [iOS API Reference](../api-reference/index.md) — all classes and methods.
