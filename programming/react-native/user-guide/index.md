---
layout: default-layout
title: User Guide - Dynamsoft MRZ Scanner for React Native (Ready to Use UI edition)
description: This is the user guide of Dynamsoft MRZ Scanner for React Native SDK demonstrating the Ready to Use UI.
keywords: user guide, dart, React Native, ready to use, mrz
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
multiProgrammingLanguage: true
enableLanguageSelection: true
---

# MRZ Scanner User Guide

This user guide will explore using the Dynamsoft MRZ Scanner (React Native Edition) to easily integrate the ability to read MRZ data from identity documents such as passports and ID cards. The Dynamsoft MRZ Scanner comes with a ready-to-use setup that simplifies the development process, allowing you to focus on other aspects of the application.

`MRZScanner` is the ready-to-use component that allows developers to quickly set up an MRZ scanning app. With the built-in component, it streamlines the integration of MRZ scanning functionality into any application.

## Supported Machine-Readable Travel Document Types

The Machine Readable Travel Documents (MRTD) standard specified by the International Civil Aviation Organization (ICAO) defines how to encode information for optical character recognition on official travel documents.

Currently, the SDK supports three types of MRTD:

> [!NOTE]
> If you need support for other types of MRTDs, our SDK can be easily customized. Please contact our [support team](https://www.dynamsoft.com/contact/).

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

* React Native 0.71.0+ (0.79.0+ recommended)
* Node 18+
* Android
  * Supported OS: Android 5.0 (API Level 21) and higher
  * Supported ABI: armeabi-v7a, arm64-v8a, x86 and x86_64
  * Development Environment: Android Studio 2022.2.1+ (2025.2.1 recommended); Java 17+; Gradle 8.0+
* iOS
  * Supported OS: iOS 13+
  * Supported ABI: arm64 and x86_64
  * Development Environment: Xcode 13+ (Xcode 14.1+ recommended)

## Including the Library

Run the following command in the root directory of your React Native project to add `dynamsoft-capture-vision-react-native` to the dependencies:

```bash
npm install dynamsoft-capture-vision-react-native
```

Then run this command to install all dependencies:

```bash
npm install 
```

## Building the MRZ Scanner Widget

Now that the package is added, it's time to start building the `MRZScanner` Widget using the SDK.

### Import

To use the MRZScanner API, we have to import the relevant classes from the `dynamsoft-capture-vision-react-native` package. Please import `dynamsoft-capture-vision-react-native` in your dart file:

```ts
import { MRZScanner, MRZScanResult, MRZScanConfig } from 'dynamsoft-mrz-scanner-bundle-react-native';
```

### Quick Start

The code below shows the simplest function implementation to initialize and start the MRZ Scanner

```tsx
import { MRZScanner, MRZScanResult, MRZScanConfig } from 'dynamsoft-mrz-scanner-bundle-react-native';

async function ScanMRZ() {
  const config = {
    license: 'DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9', // 24-hour license key
  } as MRZScanConfig;
  const mrzResult = await MRZScanner.launch(config);
  if (mrzResult.resultStatus === EnumResultStatus.RS_FINISHED) {
    let mrzData = mrzResult?.data!;
    // do something with the MRZData object
  }
}
```

You can call the above function anywhere (e.g., when the app starts, on a button click, etc.) to achieve the effect:
open an MRZ scanning interface, and after scanning is complete, close the interface and return the result.

This next code snippet is the **full Hello World implementation** that uses the above `ScanMRZ` function. This is done in *App.tsx* but can be used as a reference for your own implementation, whether it is in *App.tsx* or any other page.

```tsx
import React, {useState} from 'react';
import {Button, ScrollView, StyleSheet, Text, View} from 'react-native';
import {MRZScanner, EnumResultStatus, MRZScanConfig} from 'dynamsoft-mrz-scanner-bundle-react-native';

function App(): React.JSX.Element {
  const [displayText, setDisplayText] = useState<string>('');
  const [isError, setIsError] = useState<boolean>(false);
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

  return (
    <View style={styles.container}>
      <ScrollView>
        <Text style={[styles.text, isError ? styles.errorText : null]}>
          {displayText}
        </Text>
      </ScrollView>
      <Button title={'Scan MRZ'} onPress={ScanMRZ}/>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {flex: 1, alignItems: 'center', justifyContent: 'center', padding: 20, marginBottom: 50},
  text: {marginTop: 20, fontSize: 16, color: 'black'},
  errorText: {color: 'red'},
});

export default App;
```

> [!NOTE]
>
>- The license string here grants a time-limited free trial which requires network connection to work.
>- You can request a 30-day trial license via the [Trial License portal](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=github&package=mobile).

### MRZ Result and Data

Once the scan process completes and the MRZ Scanner successfully recognizes a MRZ, a `MRZScanResult` is produced, representing all of the decrypted data contained within the MRZ.

[`MRZScanResult`](../api-reference/mrz-scan-result.md) has the following properties:

- **resultStatus**: The status of the MRZ scan result, of type `EnumResultStatus`.
    - *finished*: The MRZ scan was successful.
    - *canceled*: The MRZ scanning activity is closed before the process is finished.
    - *exception*: Failed to start MRZ scanning or an error occurs when scanning the MRZ.
- **errorCode**: The error code indicates if something went wrong during the MRZ scanning process (0 means no error). Only defined when `resultStatus` is RS_EXCEPTION.
- **errorString**: The error message associated with the error code if an error occurs during MRZ scanning process. Only defined when `resultStatus` is RS_EXCEPTION.
- **data**: The parsed MRZ data as a `MRZData` object.
  
[`MRZData`](../api-reference/mrz-data.md) holds the actual decrypted data of the MRZ result, and it comes with the following fields:

- **documentType**: The type of document, which would either be `EnumDocumentType.DT_PASSPORT`, `EnumDocumentType.DT_ID`, or `EnumDocumentType.DT_ALL`.
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

Even though the default settings of the ready-to-use MRZ Scanner is sufficient to cover the majority of MRZ scanning scenarios, the `MRZScanConfig` class allows you to change the behaviour of the MRZ Scanner to fit your specific scenario. Using this class can help you customize different UI elements and determine the settings of the scanner engine itself.

To learn of the different ways in which the MRZ scanner can be customized, please refer to the [MRZ Scanner Customization Guide](customize-mrz-scanner.md).

## Run the Project

### iOS

Before the project can be deployed to a *iOS* device, the camera permissions and the developer signature must first be set. To add the camera permissions to the iOS portion of the app, we recommend first installing the **pods** dependencies to generate the **.xcworkspace** project under the ios folder (`ios/<Project Name>.xcworkspace`). Please run the following commands from the root directory:

```bash
cd ios/
pod install --repo-update
```

Once the pods are installed, *<Project Name>.xcworkspace* should now be generated in the *ios* folder. 

#### Camera Permissions

To add the **camera permissions**, open the generated *<Project Name>.xcworkspace* and navigate to the *Info* section of the project settings. Then you must add the "Privacy - Camera Usage Description" key to the list (where you can also assign a string message to show in the alert box).

#### Deploying to Device

In order to deploy the app to a iOS device, we recommend doing it via Xcode by using the *<Project Name>.xcworkspace* project that was generated when the pods were installed. Since the camera permissions are taken care of, all you need to do is properly configure the *Signing & Capabilities* section of the project settings. Should the iOS device be connected to the computer, you can now run and deploy the app to the device. 

If everything is set up correctly, you should see the app running on your device.

### Android

#### Deploying to Device

Go to the project root folder, open a new terminal and run the following command:

```bash
npm run android
# or
yarn android
```

You can get the IDs of all connected (physical) devices by running the command `adb devices` in the terminal. 

## Full Sample Code

The full sample code is available [here](https://github.com/Dynamsoft/capture-vision-react-native-samples/tree/main/ScanMRZ).

## License

- You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=github&package=mobile) link.

## Contact

If you have any questions or issues, please contact the [Dynamsoft support team](https://www.dynamsoft.com/company/contact/).