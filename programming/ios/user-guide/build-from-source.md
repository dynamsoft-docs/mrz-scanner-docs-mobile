---
layout: default-layout
title: Building from Source - Dynamsoft MRZ Scanner iOS Edition
description: Build the Dynamsoft MRZ Scanner component from its published source code and use your own build in an iOS app.
keywords: build from source, open source, customize, iOS, xcframework, Xcode
breadcrumbText: Build from Source
noTitleIndex: true
needGenerateH3Content: true
needAutoGenerateSidebar: true
---

# Building the MRZ Scanner from Source

[`MRZScannerConfig`](customize-mrz-scanner.md) covers most of what an integration needs to change. When it does not go far enough, the source of the `DynamsoftMRZScannerBundle` component is published on GitHub, and you can build your own copy of the framework.

## When to Build from Source

Build from source when you need behavior the configuration API cannot express — restructuring the scanner screen, replacing its controls, or changing how results are assembled before they reach your app.

Stay on the released framework when `MRZScannerConfig` can do the job. Building from source means you own the merge at every SDK release: your changes have to be re-applied to each new version, and you no longer pick up fixes by changing a version number.

> [!NOTE]
> If you are unsure whether you need a source build, or get stuck along the way, contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).

## What the Source Includes

The published source is the `DynamsoftMRZScannerBundle` component — `MRZScannerViewController` and its extensions, the configuration and result types, the camera permission handling, and the scanner UI with its image assets. It also carries the Core ML models the scanner runs on and the `mrz-mobile.json` template that drives detection.

The MRZ detection engine itself is not part of it. The project depends on **Dynamsoft Capture Vision**, resolved through Swift Package Manager from [`capture-vision-spm`](https://github.com/Dynamsoft/capture-vision-spm), and that stays a binary dependency.

## Prerequisites

- **Xcode 14.1 or higher**, matching the SDK's own requirement.
- **A physical iOS device** for testing. The Simulator has no camera, so the scanner cannot run there — though the build does produce a Simulator slice.
- **Network access to GitHub**, so Swift Package Manager can resolve Capture Vision on the first build.

## Get the Source

Clone the repository:

```bash
git clone https://github.com/Dynamsoft/mrz-scanner-mobile.git
```

The framework project is at `ios/src/DynamsoftMRZScannerBundle`. Open `DynamsoftMRZScannerBundle.xcodeproj` and you will find two schemes:

| Scheme | Purpose |
| ------ | ------- |
| `DynamsoftMRZScannerBundle` | Builds the framework itself for a single destination. Use this while editing the source. |
| `XCFramework` | Archives the framework for device and Simulator, then combines both into a distributable `.xcframework`. |

The first build resolves the Capture Vision package, which takes a moment and needs network access.

## Build the XCFramework

Select the **XCFramework** scheme in Xcode and build it, or run:

```bash
xcodebuild -project DynamsoftMRZScannerBundle.xcodeproj -scheme XCFramework build
```

The scheme runs a script that archives the framework twice — once for `generic/platform=iOS` and once for `generic/platform=iOS Simulator`, both with `BUILD_LIBRARY_FOR_DISTRIBUTION` enabled — and then merges the two archives. The result is:

```
build/DynamsoftMRZScannerBundle.xcframework
```

It carries two slices, `ios-arm64` and `ios-arm64_x86_64-simulator`, and is self-contained: the Core ML models, the image assets, `mrz-mobile.json`, and the privacy manifest are all bundled inside it. The two intermediate `.xcarchive` bundles are left in `build/` as well and can be discarded.

> [!NOTE]
> The build script checks that the `Models` directory contains model files and stops with an error if it is empty. If you see that error, your clone is incomplete rather than misconfigured — re-clone the repository.

## Use Your Build in an App

Capture Vision is **not** bundled into the framework you just built. The released `mrz-scanner-spm` package pulls it in for you; your own build does not, so you have to add it yourself either way.

### Add Capture Vision

In your app project, remove the `mrz-scanner-spm` package dependency if it is present, then add Capture Vision on its own:

1. In Xcode, go to **File** > **Add Package Dependencies...**.
2. Enter `https://github.com/Dynamsoft/capture-vision-spm` in the search field.
3. Choose **Exact Version**, enter **3.6.2000**, then click **Add Package**.

Match the version to the one in the source you built. It is declared in the framework project's package dependencies, and it moves with each SDK release.

### Add Your Framework

Two ways to bring in your own build, depending on what you are doing:

**While you are still changing the source**, add the project itself. Drag `DynamsoftMRZScannerBundle.xcodeproj` into your app project in Xcode, then add `DynamsoftMRZScannerBundle.framework` to your target's **Frameworks, Libraries, and Embedded Content**, set to **Embed & Sign**. Your edits then rebuild along with the app, with no separate publish step.

**Once your changes have settled**, use the built artifact instead. Drag `build/DynamsoftMRZScannerBundle.xcframework` into your app project, and set it to **Embed & Sign** in the same section. This is also the form to hand to other people or check into a dependency repository.

## Confirm You Are Running Your Build

An unmodified build of the published source behaves identically to the released framework, so a successful build is not by itself evidence that your copy is the one being used — particularly easy to get wrong here, because the released package and your build share a name.

To confirm the wiring before you rely on it, make a change you can see — a string in the scanner UI is enough — then rebuild and run the app on a device.

## Keeping Up with SDK Releases

Your changes live in your own copy of the source, so each new SDK release is a merge:

1. Pull the new version of the repository.
2. Re-apply your changes to the updated source.
3. Rebuild the XCFramework and replace the copy in your app.

Check the Capture Vision version in the framework project's package dependencies after each pull. It moves with the release, and the version your app resolves has to match the one your build was compiled against.
