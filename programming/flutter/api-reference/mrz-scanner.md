---
layout: default-layout
title: MRZScanner Class - Dynamsoft MRZ Scanner Flutter Edition
description: MRZScanner of DynamsoftMRZScanner Flutter is an activity class that implements MRZ scanning features.
keywords: MRZ, scanner, activity
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanner
---

# Class MRZScanner

`MRZScanner` is the main class for setting up and running the MRZ Scanner.

## Definition

*Assembly:* dynamsoft_mrz_scanner_bundle_flutter

```dart
class MRZScanner
```

## Methods

## launch

Starts the MRZ (Machine Readable Zone) scanner by launching a new page (Activity on Android or ViewController on iOS).

This function initiates the MRZ scanning process by launching a new page dedicated to scanning. It accepts an optional configuration object to customize the scanning behavior. If no configuration is provided, it uses a default configuration. The function returns a Promise that resolves with the scan result or rejects with an error if the scanning process fails.

For Android, this function starts an Activity that handles the MRZ scanning. For iOS, it presents a ViewController for the same purpose. After scanning, the function handles the result and closes the scanning page if necessary.

```dart
Future<MRZScanResult> launch(MRZScannerConfig config)
```

## Code Snippet

```dart
void _launchMrzScanner() async {
    var config = MRZScannerConfig(
        license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
    );
    MRZScanResult mrzScanResult = await MRZScanner.launch(config);

    setState(() {
        if(mrzScanResult.status == EnumResultStatus.canceled) {
        _displayString = "Scan canceled";
        } else if(mrzScanResult.status == EnumResultStatus.exception) {
        _displayString = "ErrorCode: ${mrzScanResult.errorCode}\n\nErrorString: ${mrzScanResult.errorMessage}";
        } else { //EnumResultStatus.finished
        MRZData data = mrzScanResult.mrzData!;
        _displayString = "Name:\t${data.firstName} ${data.lastName}\n\n"
            "Sex: ${data.sex.substring(0,1).toUpperCase() + data.sex.substring(1)}\n\n"
            "Age: ${data.age}\n\n"
            "Document Type: ${data.documentType}\n\n"
            "Document Number: ${data.documentNumber}\n\n"
            "Issuing State: ${data.issuingState}\n\n"
            "Nationality: ${data.nationality}\n\n"
            "Date of Birth(YYYY-MM-DD): ${data.dateOfBirth}\n\n"
            "Date of Expiry(YYYY-MM-DD): ${data.dateOfExpire}";
        }
    });
}
```

> [!NOTE]
> If you would like to see the full code to implement the full Hello World sample, please refer to the [User Guide](../user-guide/index.md).
