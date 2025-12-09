---
layout: default-layout
title: User Guide - Dynamsoft MRZ Scanner for Flutter (Ready to Use UI edition)
description: This is the user guide of Dynamsoft MRZ Scanner for Flutter SDK demonstrating the Ready to Use UI.
keywords: user guide, dart, Flutter, ready to use, mrz
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
multiProgrammingLanguage: true
enableLanguageSelection: true
---

# MRZ Scanner User Guide

This user guide will explore using the Dynamsoft MRZ Scanner (Flutter Edition) to easily integrate the ability to read MRZ data from identity documents such as passports and ID cards. The Dynamsoft MRZ Scanner comes with a ready-to-use setup that simplifies the development process, allowing you to focus on other aspects of the application.

`MRZScanner` is the ready-to-use component that allows developers to quickly set up an MRZ scanning app. With the built-in component, it streamlines the integration of MRZ scanning functionality into any application.

## Supported Machine-Readable Travel Document Types

The Machine Readable Travel Documents (MRTD) standard specified by the International Civil Aviation Organization (ICAO) defines how to encode information for optical character recognition on official travel documents.

Currently, the SDK supports three types of MRTD:

> [!NOTE]
> If you need support for other types of MRTDs, our SDK can be easily customized. Please contact support@dynamsoft.com.

### ID (TD1 Size)

The MRZ (Machine Readable Zone) in TD1 format consists of 3 lines, each containing 30 characters.

<div>
   <img src="../../assets/td1-id.png" alt="Example of MRZ in TD1 format" width="60%" />
</div>

### ID (TD2 Size)

The MRZ (Machine Readable Zone) in TD2 format consists of 2 lines, with each line containing 36 characters.

<div>
   <img src="../../assets/td2-id.png" alt="Example of MRZ in TD2 format" width="72%" />
</div>

### Passport (TD3 Size)

The MRZ (Machine Readable Zone) in TD3 format consists of 2 lines, with each line containing 44 characters.

<div>
   <img src="../../assets/td3-passport.png" alt="Example of MRZ in TD2 format" width="88%" />
</div>

## System Requirements

* Latest [Flutter SDK](https://flutter.dev/)
* Android
  * Supported OS: Android 5.0 (API Level 21) and higher
  * Supported ABI: armeabi-v7a, arm64-v8a, x86 and x86_64
  * Development Environment: Android Studio Meerkat (2024.3.1); Java 17+; Gradle 8.0+
* iOS
  * Supported OS: iOS 13+
  * Supported ABI: arm64 and x86_64
  * Development Environment: Xcode 13+ (Xcode 14.1+ recommended)

## Including the Library

Run the following command in the root directory of your Flutter project to add `dynamsoft_mrz_scanner_bundle_flutter` to the dependencies:

```bash
flutter pub add dynamsoft_mrz_scanner_bundle_flutter
```

Then run the command to install all dependencies:

```bash
flutter pub get
```

## Building the MRZ Scanner Widget

Now that the package is added, it's time to start building the `MRZScanner` Widget using the SDK.

### Import

To use the MRZScanner API, please import `dynamsoft_mrz_scanner_bundle_flutter` in your dart file:

```dart
import 'package:dynamsoft_mrz_scanner_bundle_flutter/dynamsoft_mrz_scanner_bundle_flutter.dart';
```

### Quick Start

The code below shows the simplest function implementation to initialize and start the MRZ Scanner

```dart
import 'package:dynamsoft_mrz_scanner_bundle_flutter/dynamsoft_mrz_scanner_bundle_flutter.dart';

void _launchMrzScanner() async {
  var config = MRZScannerConfig(
    license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
  );
  MRZScanResult mrzScanResult = await MRZScanner.launch(config);
  if(mrzScanResult.status == EnumResultStatus.finished) {
    MRZData data = mrzScanResult.mrzData!;
    // do something with the data
  }
}
```

You can call the above function anywhere (e.g., when the app starts, on a button click, etc.) to achieve the effect:
open an MRZ scanning interface, and after scanning is complete, close the interface and return the result.

This next code snippet is the **full Hello World implementation** that uses the above `_launchMrzScanner` function. This is done in *main.dart* but can be used as a reference for your own implementation, whether it is in *main.dart* or any other page.

```dart
import 'package:dynamsoft_mrz_scanner_bundle_flutter/dynamsoft_mrz_scanner_bundle_flutter.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Scan MRZ',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.orange),
      ),
      home: const MyHomePage(title: 'Scan MRZ'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  final String title;
  const MyHomePage({super.key, required this.title});
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  String _displayString = "";

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

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(widget.title),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text(
                _displayString,
                style: Theme.of(context).textTheme.bodyLarge,
              ),
              SizedBox(height: 20), // Add a spacing of 20
              TextButton(
                onPressed: _launchMrzScanner,
                style: TextButton.styleFrom(
                  backgroundColor: Colors.orange,
                  foregroundColor: Colors.white,
                ),
                child: const Text("Scan an MRZ"),
              ),
            ],
          ),
        )
    );
  }
}
```

> [!NOTE]
>
>- The license string here grants a time-limited free trial which requires network connection to work.
>- You can request a 30-day trial license via the [Trial License portal](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=github&package=mobile).

### MRZ Result and Data

Once the scan process completes and the MRZ Scanner successfully recognizes a MRZ, a `MRZScanResult` is produced, representing all of the decrypted data contained within the MRZ.

`MRZScanResult` has the following properties:

- **resultStatus**: The status of the MRZ scan result, of type `EnumResultStatus`.
    - finished: The MRZ scan was successful.
    - canceled: The MRZ scanning activity is closed before the process is finished.
    - exception: Failed to start MRZ scanning or an error occurs when scanning the MRZ.
- **errorCode**: The error code indicates if something went wrong during the MRZ scanning process (0 means no error). Only defined when `resultStatus` is RS_EXCEPTION.
- **errorString**: The error message associated with the error code if an error occurs during MRZ scanning process. Only defined when `resultStatus` is RS_EXCEPTION.
- **data**: The parsed MRZ data as a `MRZData` object.
  
`MRZData` holds the actual decrypted data of the MRZ result, and it comes with the following fields:

- **documentType**: The type of document, such as `'ID cards'` or `'passports'`.
- **firstName**: The first name of the user of the MRZ document.
- **lastName**: The last name of the user of the MRZ document.
- **sex**: The sex of the user of the MRZ document.
- **issuingState**: The issuing state of the MRZ document.
- **nationality**: The nationality of the user of the MRZ document.
- **dateOfBirth**: The date of birth of the user of the MRZ document.
- **dateOfExpiry**: The expiry date of the MRZ document.
- **documentNumber**: The MRZ document number.
- **age**: The age of the user of the MRZ document.
- **mrzText**: The raw text of the MRZ.


### Customizing the MRZ Scanner (Optional)

Even though the default settings of the ready-to-use MRZ Scanner is sufficient to cover the majority of MRZ scanning scenarios, the `MRZScannerConfig` class allows you to change the behaviour of the MRZ Scanner to fit your specific scenario. Using this class can help you customize different UI elements and determine the settings of the scanner engine itself.

```dart
var config = MRZScannerConfig(
  ///The license key required to initialize the MRZ Scanner.
  license: "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",
  
  ///Determines whether the torch (flashlight) button is visible in the scanning UI.
  ///Set to true to display the torch button, enabling users to turn the flashlight on/off. Default is true.
  isTorchButtonVisible: true,
  
  ///Specifies the type of document to be scanned for MRZ.
  ///This property accepts values defined in the EnumDocumentType, such as `EnumDocumentType.all`, `EnumDocumentType.id`, or `EnumDocumentType.passport`.
  ///It helps the scanner to optimize its processing based on the expected document type.
  ///Default is EnumDocumentType.all.
  documentType: EnumDocumentType.all,

  ///Specifies if a beep sound should be played when an MRZ is successfully detected.
  ///Set to true to enable the beep sound, or false to disable it. Default is false.
  isBeepEnabled: false,

  ///Determines whether the close button is visible on the scanner UI.
  ///This button allows users to exit the scanning interface. Default is true.
  isCloseButtonVisible: true,

  ///Specifies whether the camera toggle button is displayed.
  ///This button lets users switch between available cameras (e.g., front and rear). Default is false.
  isCameraToggleButtonVisible: false,

  ///Determines whether a guide frame is visible during scanning.
  ///The guide frame assists users in properly aligning the document for optimal MRZ detection.
  ///When set to true, a visual overlay is displayed on the scanning interface. Default is true.
  isGuideFrameVisible: true,


  ///Specifies the template configuration for the MRZ scanner.
  ///This can be either a file path or a JSON string that defines various scanning parameters.
  ///Default is undefined, which means the default template will be used.
  templateFile: "JSON template string",
);
```

## Run the Project

### iOS

Before the project can be deployed to a *iOS* device, the camera permissions and the developer signature must first be set. To add the camera permissions to the iOS portion of the app, we recommend first installing the **pods** dependencies to generate the **.xcworkspace** project under the ios folder (`ios/Runner.xcworkspace`). Please run the following commands from the root directory:

```bash
cd ios/
pod install --repo-update
```

Once the pods are installed, *Runner.xcworkspace* should now be generated in the *ios* folder. 

#### Camera Permissions

To add the **camera permissions**, open the generated *Runner.xcworkspace* and navigate to the *Info* section of the project settings. Then you must add the "Privacy - Camera Usage Description" key to the list (where you can also assign a string message to show in the alert box).

#### Deploying to Device

In order to deploy the app to a iOS device, we recommend doing it via Xcode by using the `Runner.xcworkspace` project that was generated when the pods were installed. Since the camera permissions are taken care of, all you need to do is properly configure the *Signing & Capabilities* section of the project settings. Should the iOS device be connected to the computer, you can now run and deploy the app to the device. 

If everything is set up correctly, you should see the app running on your device.

### Android

#### Deploying to Device

Go to the project root folder, open a new terminal and run the following command:

```bash
flutter run
# or
flutter run -d <your_device_id>
```

You can get the IDs of all connected (physical) devices by running the command `flutter devices` in the terminal. 

## Full Sample Code

The full sample code is available [here](./ScanMRZ).

## License

- You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=github&package=mobile) link.

## Contact

https://www.dynamsoft.com/company/contact/