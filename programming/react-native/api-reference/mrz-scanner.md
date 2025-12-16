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

*Assembly:* dynamsoft-capture-vision-react-native

```ts
class MRZScanner
```

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
    let mrzScanConfig = {
        license: 'DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9',
    } as MRZScanConfig;
    let mrzResult = await MRZScanner.launch(mrzScanConfig);
    let displayString: string;
    if (mrzResult.resultStatus === EnumResultStatus.RS_FINISHED) {
        let mrzData = mrzResult?.data!;
        displayString = Object.entries(mrzData)
        .map(([fieldName, fieldValue]) => `${fieldName} : \t${fieldValue}`)
        .join('\n\n');
    } else if (mrzResult.resultStatus === EnumResultStatus.RS_CANCELED) {
        displayString = 'Scan cancelled.';
    } else {
        displayString = `ErrorCode: ${mrzResult.errorCode}\n\nErrorMessage: ${mrzResult.errorString}`;
    }
    setDisplayText(displayString);
    setIsError(mrzResult.resultStatus === EnumResultStatus.RS_EXCEPTION);
};
```

> [!NOTE]
> If you would like to see the full code to implement the full Hello World sample, please refer to the [User Guide](../user-guide/index.md).
