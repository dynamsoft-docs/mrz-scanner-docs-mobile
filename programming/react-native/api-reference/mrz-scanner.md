---
layout: default-layout
title: MRZScanner Class - Dynamsoft MRZ Scanner React Native Edition
description: MRZScanner of DynamsoftMRZScanner React Native is an activity class that implements MRZ scanning features.
keywords: MRZ, scanner, activity
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanner
---

# Class MRZScanner

`MRZScanner` is the main class for setting up and running the MRZ Scanner.

## Definition

*Assembly:* dynamsoft-mrz-scanner-bundle-react-native

```ts
class MRZScanner
```

`MRZScanner` has a private constructor and exposes a single static entry point, [`launch`](#launch). Invoke it directly via `MRZScanner.launch(config)`.

## Methods

## launch

Starts the MRZ (Machine Readable Zone) scanner by launching a new page (Activity on Android or ViewController on iOS).

This function initiates the MRZ scanning process by launching a new page dedicated to scanning. It accepts an optional configuration object to customize the scanning behavior. If no configuration is provided, it uses a default configuration. The function returns a Promise that resolves with the scan result or rejects with an error if the scanning process fails.

For Android, this function starts an Activity that handles the MRZ scanning. For iOS, it presents a ViewController for the same purpose. After scanning, the function handles the result and closes the scanning page if necessary.

```ts
launch(config?: MRZScanConfig): Promise<MRZScanResult>
```

## Code Snippet

```ts
const ScanMRZ = async () => {
    const mrzScanConfig = {
        license: 'DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9',
    } as MRZScanConfig;
    const mrzResult = await MRZScanner.launch(mrzScanConfig);
    if (mrzResult.resultStatus === EnumResultStatus.RS_FINISHED) {
        const mrzData = mrzResult.data!;
        // Use the parsed fields (mrzData.firstName, mrzData.dateOfExpire, etc.)
        // and the captured images (mrzResult.portraitImage, mrzResult.mrzSideDocumentImage, ...).
    } else if (mrzResult.resultStatus === EnumResultStatus.RS_CANCELED) {
        // User closed the scanner before it finished.
    } else {
        // RS_EXCEPTION — inspect mrzResult.errorCode / mrzResult.errorString.
    }
};
```

> [!NOTE]
> If you would like to see the full code to implement the full Hello World sample, please refer to the [User Guide](../user-guide/index.md).
