---
layout: default-layout
title: User Guide - Dynamsoft MRZ Scanner for Flutter (Ready to Use UI edition)
description: This is the user guide of Dynamsoft MRZ Scanner for Flutter SDK demonstrating the Ready to Use UI.
keywords: user guide, dart, Flutter, ready to use, mrz
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
---

# MRZ Scanner User Guide (Flutter Edition)

The Dynamsoft MRZ Scanner (Flutter Edition) provides a ready-to-use scanning component that lets you add MRZ reading to your app with minimal setup. This guide walks through building a complete MRZ scanning app from scratch using `MRZScanner` — the built-in component that handles the camera UI, scanning logic, and result delivery.

> [!IMPORTANT]
> For the full sample code, visit the [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/capture-vision-flutter-samples/tree/main/ScanMRZ).

## Supported Document Types

The SDK supports three ICAO Machine Readable Travel Document (MRTD) formats: **TD1** (ID cards, 3-line MRZ), **TD2** (ID cards, 2-line MRZ), and **TD3** (passports, 2-line MRZ). For a visual reference of each format, see [Supported Document Types](../../shared/supported-document-types.md).

> [!NOTE]
> For support for other MRTD types, contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).

## System Requirements

- Latest [Flutter SDK](https://flutter.dev/)
- **Android**: Android 5.0 (API Level 21) or higher; armeabi-v7a, arm64-v8a, x86, x86_64; Android Studio Meerkat (2024.3.1); Java 17+; Gradle 8.0+
- **iOS**: iOS 13 or higher; arm64 and x86_64; Xcode 13+ (Xcode 14.1+ recommended)

## Licensing

A valid license key is required to use the SDK. If you are just getting started, request a free 30-day trial license below:

{% include trialLicense.html %}

> [!NOTE]
>
> - The license string above grants a time-limited free trial which requires a network connection.
> - You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=guide&package=flutter){:target="_blank"} link.

## Building the MRZ Scanner Application

The following steps build the **ScanMRZ** sample app. You can also download the complete project from the [GitHub repo](https://github.com/Dynamsoft/capture-vision-flutter-samples/tree/main/ScanMRZ).

### Step 1: Create a New Project

Create a new Flutter project and open the project in your IDE:

```bash
flutter create scan_mrz
```

Navigate to `lib/main.dart` — this is where the implementation will go.

### Step 2: Add the SDK

Run the following command from the project root to add `dynamsoft_mrz_scanner_bundle_flutter`:

```bash
flutter pub add dynamsoft_mrz_scanner_bundle_flutter
```

Then install all dependencies:

```bash
flutter pub get
```

### Step 3: Set Up the UI

Replace the contents of `lib/main.dart` with a `StatefulWidget` that contains a **Scan MRZ** button at the bottom and a display area for the result. The `_launchMrzScanner` method will be added in the next step.

```dart
import 'package:dynamsoft_mrz_scanner_bundle_flutter/dynamsoft_mrz_scanner_bundle_flutter.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Scan MRZ',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.orange),
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  String _displayString = "";

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Column(
          children: [
            Expanded(
              child: Center(
                child: Text(
                  _displayString,
                  style: Theme.of(context).textTheme.bodyLarge,
                  textAlign: TextAlign.center,
                ),
              ),
            ),
            Padding(
              padding: const EdgeInsets.all(16),
              child: SizedBox(
                width: double.infinity,
                height: 48,
                child: ElevatedButton(
                  onPressed: _launchMrzScanner,
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.orange,
                    foregroundColor: Colors.white,
                  ),
                  child: const Text("Scan MRZ"),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Step 4: Configure the Scanner

Add the `_launchMrzScanner` method to `_MyHomePageState`. Create an `MRZScannerConfig` with your license key and pass it to `MRZScanner.launch()`.

The only required setting is the license key — see the [Licensing](#licensing) section above for how to obtain one. For the full list of optional settings such as document type filtering, UI button visibility, and image capture options, see the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.

```dart
/* Add to _MyHomePageState */
void _launchMrzScanner() async {
  var config = MRZScannerConfig(
    license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
  );
  MRZScanResult mrzScanResult = await MRZScanner.launch(config);
}
```

### Step 5: Launch the Scanner and Handle Results

`MRZScanner.launch()` returns an `MRZScanResult` when the scanner closes. Each result carries a `status` of *finished* (MRZ decoded), *canceled* (user closed the scanner), or *exception* (an error occurred).

Extend `_launchMrzScanner` to handle all three cases. When the scan succeeds, access the parsed data and any captured images from the result:

```dart
/* _MyHomePageState — complete _launchMrzScanner */
void _launchMrzScanner() async {
  var config = MRZScannerConfig(
    license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
  );
  MRZScanResult mrzScanResult = await MRZScanner.launch(config);

  if (mrzScanResult.status == EnumResultStatus.finished &&
      mrzScanResult.mrzData != null) {
    MRZData data = mrzScanResult.mrzData!;

    // Captured images (Uint8List? — null if not captured or disabled in config)
    final portrait    = mrzScanResult.portraitImage;
    final mrzDoc      = mrzScanResult.mrzSideDocumentImage;
    final oppDoc      = mrzScanResult.oppositeSideDocumentImage;
    final mrzOriginal = mrzScanResult.mrzSideOriginalImage;
    final oppOriginal = mrzScanResult.oppositeSideOriginalImage;

    setState(() {
      _displayString = "Name: ${data.firstName} ${data.lastName}\n"
          "DOB: ${data.dateOfBirth}\nExpiry: ${data.dateOfExpire}";
    });
  } else if (mrzScanResult.status == EnumResultStatus.canceled) {
    /* The user closed the scanner before a result was produced */
    setState(() => _displayString = "Scan canceled");
  } else {
    /* An error occurred during scanning */
    setState(() => _displayString =
        "ErrorCode: ${mrzScanResult.errorCode}\n\nErrorString: ${mrzScanResult.errorMessage}");
  }
}
```

> [!NOTE]
>
> - `mrzSideDocumentImage` is the cropped, perspective-corrected image of the side containing the MRZ. `oppositeSideDocumentImage` is the reverse side, relevant for two-sided documents such as TD1 ID cards.
> - Image properties return `null` if the corresponding capture option was disabled in `MRZScannerConfig`, or if no image was captured for that side.
> - `mrzScanResult.mrzData` is nullable. Always check for `null` before accessing its fields.

#### Image Properties on `MRZScanResult`

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`portraitImage`](../api-reference/mrz-scan-result.md) | `Uint8List?` | Extracted portrait photo. Enabled by default; control via `returnPortraitImage`. |
| [`mrzSideDocumentImage`](../api-reference/mrz-scan-result.md) | `Uint8List?` | Cropped document image of the MRZ side. Enabled by default; control via `returnDocumentImage`. |
| [`oppositeSideDocumentImage`](../api-reference/mrz-scan-result.md) | `Uint8List?` | Cropped document image of the opposite side. Enabled by default; control via `returnDocumentImage`. |
| [`mrzSideOriginalImage`](../api-reference/mrz-scan-result.md) | `Uint8List?` | Full camera frame of the MRZ side. Disabled by default; enable via `returnOriginalImage`. |
| [`oppositeSideOriginalImage`](../api-reference/mrz-scan-result.md) | `Uint8List?` | Full camera frame of the opposite side. Disabled by default; enable via `returnOriginalImage`. |

### Step 6: Display the Results

`MRZScanResult.mrzData` is a [`MRZData`](../api-reference/mrz-data.md) object containing the parsed MRZ fields:

| Property | Type | Description |
| -------- | ---- | ----------- |
| `firstName` | `String` | First name of the document holder. |
| `lastName` | `String` | Last name of the document holder. |
| `sex` | `String` | Sex of the document holder. |
| `age` | `int` | Calculated age of the document holder. |
| `documentType` | `String` | MRTD format (e.g. `MRTD_TD3_PASSPORT`). |
| `documentNumber` | `String` | Document number. |
| `issuingState` | `String` | Full name of the issuing country/region. |
| `issuingStateRaw` | `String` | Raw ICAO issuing state code (e.g. `CAN`). |
| `nationality` | `String` | Full name of the nationality. |
| `nationalityRaw` | `String` | Raw ICAO nationality code (e.g. `CAN`). |
| `dateOfBirth` | `String` | Date of birth (YYYY-MM-DD). |
| `dateOfExpire` | `String` | Expiry date (YYYY-MM-DD). |
| `mrzText` | `String` | Raw unparsed MRZ text. |

For the full field list, see the [MRZData API reference](../api-reference/mrz-data.md). For the complete implementation including image display, portrait fallback, tab switcher between processed and original images, and save-to-gallery support, refer to the [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/capture-vision-flutter-samples/tree/main/ScanMRZ).

### Step 7: Run the Project

#### iOS

Before deploying to an iOS device, install the pod dependencies from the project root:

```bash
cd ios/
pod install --repo-update
```

Once complete, open the generated `Runner.xcworkspace` in Xcode and complete two required configuration steps:

1. **Camera Permission** — In the **Info** tab of the project settings, add the **Privacy - Camera Usage Description** key with a description string (e.g. `"This app uses the camera to scan MRZ documents."`). Without this the app will crash when the camera is opened.

2. **Signing** — In the **Signing & Capabilities** tab, set a valid **Team**. Without this the project will fail to build.

Connect a physical iOS device, select it from the top bar, and click **Run**.

#### Android

From the project root, run:

```bash
flutter run
# or
flutter run -d <your_device_id>
```

You can list connected device IDs with `flutter devices`.

> [!NOTE]
> Running on a simulator or emulator is not supported as the scanner requires a physical device camera.

## Next Steps

- **Samples** — Explore the complete [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/capture-vision-flutter-samples/tree/main/ScanMRZ).
- **Customize** — Learn how to configure document type, UI elements, and image capture in the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.
- **API Reference** — Browse the full [Flutter API Reference](../api-reference/index.md) for all classes and methods.
- **Support** — Contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/) for help or custom requirements.
