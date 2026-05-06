---
layout: default-layout
title: User Guide - Dynamsoft MRZ Scanner for React Native (Ready to Use UI edition)
description: This is the user guide of Dynamsoft MRZ Scanner for React Native SDK demonstrating the Ready to Use UI.
keywords: user guide, React Native, typescript, ready to use, mrz
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
---

# MRZ Scanner User Guide (React Native Edition)

The Dynamsoft MRZ Scanner (React Native Edition) provides a ready-to-use scanning component that lets you add MRZ reading to your app with minimal setup. This guide walks through building a complete MRZ scanning app from scratch using `MRZScanner` — the built-in component that handles the camera UI, scanning logic, and result delivery.

> [!IMPORTANT]
> For the full sample code, visit the [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile-react-native).

## Supported Document Types

The SDK supports three ICAO Machine Readable Travel Document (MRTD) formats: **TD1** (ID cards, 3-line MRZ), **TD2** (ID cards, 2-line MRZ), and **TD3** (passports, 2-line MRZ). For a visual reference of each format, see [Supported Document Types](../../shared/supported-document-types.md).

> [!NOTE]
> For support for other MRTD types, contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).

## System Requirements

- React Native **0.71.0** or higher (**0.79.0+** recommended).
- Node **18** or higher.
- Android
  - Supported OS: **Android 5.0** (API Level 21) or higher.
  - Supported ABI: **armeabi-v7a**, **arm64-v8a**, **x86** and **x86_64**.
  - Development Environment: **Android Studio 2022.2.1+** (2025.2.1 recommended), **Java 17+**, **Gradle 8.0+**.
- iOS
  - Supported OS: **iOS 13** or higher.
  - Supported ABI: **arm64** and **x86_64**.
  - Development Environment: **Xcode 13** and above (**Xcode 14.1+** recommended).

## Licensing

A valid license key is required to use the SDK. If you are just getting started, request a free 30-day trial license below:

{% include trialLicense.html %}

> [!NOTE]
>
> - The license string above grants a time-limited free trial which requires a network connection.
> - You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=guide&package=react-native){:target="_blank"} link.

## Add the SDK

Run the following command from your React Native project root to add `dynamsoft-mrz-scanner-bundle-react-native` to the dependencies:

```bash
npm install dynamsoft-mrz-scanner-bundle-react-native@3.4.1300
```

For iOS, install the CocoaPods dependencies after the npm install completes. Run the following from the project root:

```bash
cd ios && bundle exec pod install --repo-update
```

## Building the MRZ Scanner Application

The following steps build the **ScanMRZ** sample app. You can also download the complete project from the [GitHub repo](https://github.com/Dynamsoft/mrz-scanner-mobile-react-native).

### Step 1: Create a New Project

Initialize a new React Native project:

```bash
npx @react-native-community/cli init ScanMRZ
```

Change into the new project directory — all remaining steps run from the project root:

```bash
cd ScanMRZ
```

The implementation in this guide lives in `src/App.tsx`. Create the `src` folder, move the generated `App.tsx` into it, and update `index.js` so the registered component resolves to `./src/App`:

```js
import App from './src/App';
```

### Step 2: Add the SDK

Follow the instructions in the [Add the SDK](#add-the-sdk) section above to add `dynamsoft-mrz-scanner-bundle-react-native` to your project.

### Step 3: Configure Platform Settings

Before the scanner can open the camera, both platforms need a small amount of configuration.

**iOS** — open `ios/ScanMRZ/Info.plist` and add a camera usage description. Without this the app will crash immediately when the scanner tries to open the camera:

```xml
<key>NSCameraUsageDescription</key>
<string>This app uses the camera to scan MRZ documents.</string>
```

**Android** — no manual permission changes are required. The SDK's manifest merges the `CAMERA` permission automatically. The trial license requires a network connection, so keep the `INTERNET` permission declared in `android/app/src/main/AndroidManifest.xml` (it is present by default in React Native templates):

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### Step 4: Set Up the UI

Create the initial Home screen with a single **Scan an MRZ** button anchored toward the bottom of the screen. Scan results will be rendered in the same component via conditional rendering, added in [Step 7](#step-7-display-the-results).

Replace the contents of `src/App.tsx` with the following:

```tsx
import React from 'react';
import {StyleSheet, Text, TouchableOpacity, View} from 'react-native';

function App(): React.JSX.Element {
  const ScanMRZ = async () => {
    // The scanner launch logic is added in Steps 5 and 6.
  };

  return (
    <View style={styles.idleContainer}>
      <TouchableOpacity style={styles.scanButton} onPress={ScanMRZ}>
        <Text style={styles.scanButtonText}>Scan an MRZ</Text>
      </TouchableOpacity>
    </View>
  );
}

const BG = '#000000';
const WHITE = '#ffffff';
const BTN_GREY = '#555555';

const styles = StyleSheet.create({
  idleContainer: {
    flex: 1,
    backgroundColor: BG,
    alignItems: 'center',
    justifyContent: 'flex-end',
    padding: 24,
    paddingBottom: 32,
  },
  scanButton: {
    backgroundColor: BTN_GREY,
    borderRadius: 8,
    paddingVertical: 14,
    paddingHorizontal: 48,
  },
  scanButtonText: {
    color: WHITE,
    fontSize: 16,
    fontWeight: '600',
  },
});

export default App;
```

### Step 5: Configure the Scanner

Import the SDK and create an `MRZScanConfig` inside the `ScanMRZ` function. The only required setting is the license key — see the [Licensing](#licensing) section above for how to obtain one. For the full list of optional settings such as document-type filtering, UI button visibility, and image-capture options, see the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.

```tsx
/* Add to the import block at the top of App.tsx */
import {
  EnumResultStatus,
  MRZScanConfig,
  MRZScanner,
  MRZScanResult,
} from 'dynamsoft-mrz-scanner-bundle-react-native';

/* App.tsx — update the ScanMRZ function from Step 4 */
const ScanMRZ = async () => {
  const mrzScanConfig = {
    // Required: set a valid license key.
    license: 'DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9',
  } as MRZScanConfig;
};
```

### Step 6: Launch the Scanner

Add a `mrzScanResult` state variable, then call `MRZScanner.launch(config)` to open the scanner and await the result. Each result carries a `resultStatus` of `RS_FINISHED` (MRZ decoded), `RS_CANCELED` (user closed the scanner), or `RS_EXCEPTION` (an error occurred).

Continuing from Step 5:

```tsx
/* Update the React import to include useState */
import React, {useState} from 'react';

/* App.tsx — inside the App component, above the ScanMRZ declaration */
const [mrzScanResult, setMrzScanResult] = useState<MRZScanResult | null>(null);

/* App.tsx — update the ScanMRZ function from Step 5 */
const ScanMRZ = async () => {
  const mrzScanConfig = {
    license: 'DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9',
  } as MRZScanConfig;
  const mrzResult = await MRZScanner.launch(mrzScanConfig);
  setMrzScanResult(mrzResult);
};
```

> [!NOTE]
>
> - `mrzScanResult.data` holds the parsed MRZ fields (`firstName`, `lastName`, `dateOfBirth`, etc.). See the [MRZData API reference](../api-reference/mrz-data.md) for the full field list.
> - `mrzScanResult.portraitImage`, `mrzSideDocumentImage`, `oppositeSideDocumentImage`, `mrzSideOriginalImage`, and `oppositeSideOriginalImage` are `undefined` when the corresponding image option is disabled in `MRZScanConfig` or when no image was captured for that side.
> - `mrzSideDocumentImage` corresponds to the side of the document containing the machine-readable zone. `oppositeSideDocumentImage` refers to the reverse side, which is relevant for two-sided documents such as TD1 ID cards.

### Step 7: Display the Results

The `return` statement written in Step 4 only renders the idle Home screen. Now that `mrzScanResult` is populated after a scan, branch on `resultStatus` and render the appropriate view. `RS_CANCELED` shows a short hint, `RS_EXCEPTION` shows the error string, and `RS_FINISHED` shows the parsed fields and captured images.

Replace the single `return` block from Step 4 with the following conditional renders:

```tsx
/* Add Image and ScrollView to the react-native import */
import {Image, ScrollView, StyleSheet, Text, TouchableOpacity, View} from 'react-native';

/* App.tsx — replace the return block from Step 4 */
if (mrzScanResult == null) {
  return (
    <View style={styles.idleContainer}>
      <TouchableOpacity style={styles.scanButton} onPress={ScanMRZ}>
        <Text style={styles.scanButtonText}>Scan an MRZ</Text>
      </TouchableOpacity>
    </View>
  );
}

if (mrzScanResult.resultStatus === EnumResultStatus.RS_CANCELED) {
  return (
    <View style={styles.idleContainer}>
      <Text style={styles.idleHint}>Scan cancelled.</Text>
      <TouchableOpacity style={styles.scanButton} onPress={ScanMRZ}>
        <Text style={styles.scanButtonText}>Scan an MRZ</Text>
      </TouchableOpacity>
    </View>
  );
}

if (mrzScanResult.resultStatus === EnumResultStatus.RS_EXCEPTION) {
  return (
    <View style={styles.idleContainer}>
      <Text style={styles.errorText}>{mrzScanResult.errorString}</Text>
      <TouchableOpacity style={styles.scanButton} onPress={ScanMRZ}>
        <Text style={styles.scanButtonText}>Scan an MRZ</Text>
      </TouchableOpacity>
    </View>
  );
}

// resultStatus === EnumResultStatus.RS_FINISHED
const mrzData = mrzScanResult.data!;
const fullName = `${mrzData.firstName} ${mrzData.lastName}`;

return (
  <View style={styles.container}>
    <ScrollView contentContainerStyle={styles.scrollContent}>
      {/* Header: name, gender/age, expiry, portrait image */}
      <View style={styles.headerSection}>
        <View style={styles.headerTextBlock}>
          <Text style={styles.fullName}>{fullName}</Text>
          <Text style={styles.genderAge}>
            {mrzData.sex}, {mrzData.age} years old
          </Text>
          <Text style={styles.expiryShort}>Expiry: {mrzData.dateOfExpire}</Text>
        </View>
        <Image
          style={styles.portraitBox}
          source={mrzScanResult.portraitImage ?? require('./assets/ic_portrait_placeholder.jpg')}
          resizeMode="contain"
        />
      </View>

      {/* Personal Info */}
      <Text style={styles.sectionTitle}>Personal Info</Text>
      <Text style={styles.infoRow}>Given Name: {mrzData.firstName}</Text>
      <Text style={styles.infoRow}>Surname: {mrzData.lastName}</Text>
      <Text style={styles.infoRow}>Date of Birth: {mrzData.dateOfBirth}</Text>
      <Text style={styles.infoRow}>Nationality: {mrzData.nationalityRaw}</Text>

      {/* Document Info */}
      <Text style={styles.sectionTitle}>Document Info</Text>
      <Text style={styles.infoRow}>Doc. Number: {mrzData.documentNumber}</Text>
      <Text style={styles.infoRow}>Expiry Date: {mrzData.dateOfExpire}</Text>

      {/* Raw MRZ Text */}
      <Text style={styles.sectionTitle}>Raw MRZ Text</Text>
      <Text style={styles.rawMrz}>{mrzData.mrzText}</Text>
    </ScrollView>

    {/* Bottom re-scan button */}
    <View style={styles.bottomButtons}>
      <TouchableOpacity style={styles.bottomBtn} onPress={ScanMRZ}>
        <Text style={styles.bottomBtnText}>Scan an MRZ</Text>
      </TouchableOpacity>
    </View>
  </View>
);
```

Extend the `StyleSheet.create` block with styles for the new views (`container`, `scrollContent`, `headerSection`, `headerTextBlock`, `fullName`, `genderAge`, `expiryShort`, `portraitBox`, `sectionTitle`, `infoRow`, `rawMrz`, `idleHint`, `errorText`, `bottomButtons`, `bottomBtn`, `bottomBtnText`). For brevity, the full style definitions are omitted here — refer to [App.tsx in the ScanMRZ sample](https://github.com/Dynamsoft/mrz-scanner-mobile-react-native/src/App.tsx) for the complete implementation, which also includes a Processed/Original image switcher for the document images and a long-press-to-save-image action.

> [!NOTE]
>
> - When no portrait image is available, the sample falls back to a bundled placeholder at `./assets/ic_portrait_placeholder.jpg`. Add your own placeholder image and reference it with `require('./assets/your-placeholder.jpg')`.
> - For the complete set of fields on `MRZData`, see the [MRZData API reference](../api-reference/mrz-data.md).

### Step 8: Run the Project

Both platforms must be run on a physical device — the iOS simulator and most Android emulators do not expose a working camera to the SDK.

#### iOS

Before running, the developer signing must be configured:

1. Open `ios/ScanMRZ.xcworkspace` in Xcode.
2. Select the project, open the **Signing & Capabilities** tab, and set a valid **Team**. Without this the build will fail.

Connect a physical iOS device, then launch the app using either option below.

**Option A — Run from Xcode.** Select your connected device from the top bar and click **Run**.

**Option B — Run from the command line.** From the project root, target the device with the `--device` flag. Running `npm run ios` or `npx react-native run-ios` without this flag defaults to the simulator, which will not work for this sample:

```bash
npx react-native run-ios --device
```

If multiple devices are connected, pass the device name explicitly:

```bash
npx react-native run-ios --device "DEVICE-NAME"
```

> [!NOTE]
> If you try running the project on the iOS simulator, you will encounter errors as the scanner uses the device camera, which is unavailable on the simulator.

#### Android

Connect a physical Android device with USB debugging enabled, then run from the project root:

```bash
npm run android
```

You can list connected devices with `adb devices`.

## Next Steps

- **Samples** — Explore the complete [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile-react-native).
- **Customize** — Learn how to configure document type, UI elements, and feedback in the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.
- **API Reference** — Browse the full [React Native API Reference](../api-reference/index.md) for all classes and methods.
- **Support** — Contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/) for help or custom requirements.