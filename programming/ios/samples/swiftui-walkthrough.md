---
layout: default-layout
title: Using the Scanner from SwiftUI - Dynamsoft MRZ Scanner iOS Edition
description: Add the Dynamsoft MRZ Scanner to a SwiftUI app - the UIViewControllerRepresentable bridge, presenting the scanner, and handling the result.
keywords: SwiftUI, UIViewControllerRepresentable, MRZScannerView, iOS, sample
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: SwiftUI
noTitleIndex: true
---

# Using the Scanner from SwiftUI

The SDK ships its scanner as a `UIViewController`, so a SwiftUI app reaches it through a `UIViewControllerRepresentable` bridge. That bridge is the only SDK-facing code a SwiftUI integration needs; everything above it is ordinary SwiftUI.

Two samples take this route. **ScanMRZBasicSwiftUI** shows the whole pattern on one screen and is what this page builds. **ScanMRZSwiftUI** adds a dedicated result screen on top of it, covered under [Navigating to a separate result screen](#navigating-to-a-separate-result-screen).

> [!NOTE]
> Full source on GitHub: [ScanMRZBasicSwiftUI](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZBasicSwiftUI){:target="_blank"} and [ScanMRZSwiftUI](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/ios/samples/ScanMRZSwiftUI){:target="_blank"}.

## Before you start

This page replaces Steps 4 through 6 of the [MRZ Scanner User Guide](../user-guide/index.md). Its first three steps still apply, with one change:

1. **Create the project** with **Interface** set to **SwiftUI** rather than Storyboard. Leave the generated `App` struct alone — a SwiftUI app has no `SceneDelegate`, so there is no equivalent of the guide's Step 4 and nothing to delete.
2. **[Add the SDK](../user-guide/index.md#add-the-sdk)** exactly as the guide describes.
3. **[Declare the camera usage description](../user-guide/index.md#step-3-declare-the-camera-usage-description)** — still mandatory, and still crashes the app if missing.

> [!NOTE]
> SwiftUI requires Swift. The Objective-C tabs in the user guide have no counterpart on this page.

> [!IMPORTANT]
> Both SwiftUI samples require **iOS 16** or later because they use the SwiftUI app lifecycle — newer than the SDK's own **iOS 13** minimum. The bridge shown below works on iOS 13; it is the surrounding app structure that raises the floor.

## The bridge

Add a new Swift file named `MRZScannerView.swift`. This is reusable as written — it is the same file in both SwiftUI samples:

```swift
import SwiftUI
import DynamsoftMRZScannerBundle

struct MRZScannerView: UIViewControllerRepresentable {

    // Called once the scanner finishes, is canceled, or fails.
    let onScannedResult: (MRZScanResult) -> Void

    func makeUIViewController(context: Context) -> MRZScannerViewController {
        let config = MRZScannerConfig()
        // A trial license, so it needs a network connection. Request your own at
        // https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=guide&package=ios
        config.license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"

        let scanner = MRZScannerViewController()
        scanner.config = config
        scanner.onScannedResult = onScannedResult
        return scanner
    }

    func updateUIViewController(_ uiViewController: MRZScannerViewController,
                               context: Context) {
        // The scanner is configured once in makeUIViewController and owns its own
        // state from then on, so there is nothing to push down on state changes.
    }
}
```

`updateUIViewController` is deliberately empty. SwiftUI calls it whenever surrounding state changes, but the scanner reads its config when it starts and manages its own capture session after that, so there is nothing to re-apply. Reassigning `config` here would have no effect on a scan already in progress.

Any [configuration](../user-guide/customize-mrz-scanner.md) belongs in `makeUIViewController` alongside the license — document type, UI element visibility, which images to return.

## Presenting the scanner

Present the bridge from a `.fullScreenCover`. That is the SwiftUI equivalent of the modal full-screen presentation the user guide uses: the scanner draws its own close button and expects the whole screen, so `.ignoresSafeArea()` keeps the camera preview edge to edge.

```swift
import SwiftUI
import DynamsoftMRZScannerBundle
import DynamsoftCaptureVisionBundle

// Everything the result section shows, captured off the scan result.
private struct ScannedDocument {
    let data: MRZData
    let portrait: UIImage?
}

struct ContentView: View {

    @State private var isScanning = false
    // Canceled message or error string, shown in place of the fields.
    @State private var status = ""
    @State private var scanned: ScannedDocument?

    var body: some View {
        VStack(spacing: 0) {
            ScrollView {
                VStack(alignment: .leading, spacing: 12) {
                    if !status.isEmpty {
                        Text(status)
                    }
                    if let scanned = scanned {
                        results(for: scanned)
                    }
                }
                .frame(maxWidth: .infinity, alignment: .leading)
                .padding()
            }

            Button {
                isScanning = true
            } label: {
                Text("Scan an MRZ")
                    .foregroundColor(.white)
                    .frame(maxWidth: .infinity, minHeight: 50)
                    .background(Color.accentColor)
                    .cornerRadius(8)
            }
            .padding()
        }
        .fullScreenCover(isPresented: $isScanning) {
            MRZScannerView(onScannedResult: handle(result:))
                .ignoresSafeArea()
        }
    }
}
```

## Handling the result

`onScannedResult` reports all three outcomes through the same closure. The SwiftUI version reads a little differently from the UIKit one:

```swift
// Renders one of the three result statuses the scanner can come back with.
// onScannedResult arrives off the main thread, so every state change hops first.
private func handle(result: MRZScanResult) {
    switch result.resultStatus {
    case .finished:
        guard let data = result.data else { return }
        let document = ScannedDocument(
            data: data,
            portrait: try? result.getPortraitImage()?.toUIImage()
        )
        DispatchQueue.main.async {
            status = ""
            scanned = document
            isScanning = false
        }
    case .canceled:
        // The user closed the scanner. There is no data and nothing went wrong.
        DispatchQueue.main.async {
            status = "Scan canceled"
            scanned = nil
            isScanning = false
        }
    case .exception:
        // The scanner asks for camera access itself, so a denial lands here as a
        // readable error string. This app needs no permission code of its own.
        let errorString = result.errorString ?? ""
        DispatchQueue.main.async {
            status = errorString
            scanned = nil
            isScanning = false
        }
    @unknown default:
        break
    }
}
```

Three things differ from the UIKit flow:

- **Dismissal is a state change.** Setting `isScanning = false` closes the cover, in place of a `dismiss(animated:)` call. The scanner still does not close itself — something has to do it.
- **Every branch hops to the main queue.** `onScannedResult` arrives off the main thread, and mutating `@State` from a background thread is a bug even when it appears to work.
- **Values are read before the hop.** The portrait conversion and `result.errorString` happen outside `DispatchQueue.main.async`, so each closure captures a plain value instead of reaching back into the result.

The `results(for:)` and field-rendering helpers hold the same per-field validation logic the user guide covers in [Reading a field's validation status](../user-guide/index.md#step-6-launch-the-scanner-and-show-the-result), expressed as SwiftUI views: a caption, the value, and an amber tint when `getFieldValidationStatus` returns `.failed`.

## Navigating to a separate result screen

If the result goes to its own screen rather than the same one, carry the scanned data **inside the navigation route** rather than in a companion `@State` property:

```swift
struct ScanPayload: Hashable, Identifiable {
    let id = UUID()
    let data: MRZData
    let portraitImage: UIImage?
    let primaryDocumentImage: UIImage?
    let primaryOriginalImage: UIImage?
    let secondaryDocumentImage: UIImage?
    let secondaryOriginalImage: UIImage?

    // Identity is the payload's own id — hashing the images themselves would be
    // both expensive and meaningless for navigation.
    static func == (lhs: ScanPayload, rhs: ScanPayload) -> Bool { lhs.id == rhs.id }
    func hash(into hasher: inout Hasher) { hasher.combine(id) }
}

enum Route: Hashable {
    case scanner
    case result(ScanPayload)
}
```

Keeping the payload alongside the path avoids a race: `NavigationStack` can evaluate the destination before a separately-stored `@State` value lands, which renders an empty result screen on the first push. Appending `.result(payload)` to the path makes the data arrive with the navigation rather than after it.

The custom `Hashable` conformance matters for a second reason. `Route` has to be hashable for `NavigationStack`, and the payload carries up to five `UIImage` values — hashing those would be expensive and tells you nothing useful, so identity is derived from the `UUID` instead.

`ScanMRZSwiftUI` pushes the scanner as a route too, hiding the navigation bar while it is on screen:

```swift
.navigationDestination(for: Route.self) { route in
    switch route {
    case .scanner:
        MRZScannerView(onScannedResult: handle(result:))
            .ignoresSafeArea()
            .toolbar(.hidden, for: .navigationBar)
            .navigationBarBackButtonHidden(true)
    case .result(let payload):
        ResultView(payload: payload,
                   onRescan: { if !path.isEmpty { path.removeLast() } },
                   onReturnHome: { path.removeAll() })
    }
}
```

Hiding the bar and the back button leaves the scanner's own close button as the single way out, which keeps cancellation flowing through `onScannedResult` rather than through a navigation gesture the scanner never learns about.

## Next steps

- [MRZ Scanner User Guide](../user-guide/index.md) — the UIKit walkthrough, and the shared project setup steps.
- [ScanMRZ Demo App](scanmrz-walkthrough.md) — the complete UIKit sample and its result screen.
- [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) — document type, UI elements, feedback, and camera permission.
- [iOS API Reference](../api-reference/index.md) — all classes and methods.
