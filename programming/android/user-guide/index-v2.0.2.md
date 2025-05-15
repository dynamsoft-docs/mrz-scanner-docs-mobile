---
layout: default-layout
title: User Guide - Dynamsoft MRZ Scanner for Android (Ready to Use UI edition)
description: This is the user guide of Dynamsoft MRZ Scanner for Android SDK demonstrating the Ready to Use UI.
keywords: user guide, java, kotlin, android, ready to use
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
multiProgrammingLanguage: true
enableLanguageSelection: true
---

# Android User Guide for MRZ Scanning

This user guide will walk through the [ScanMRZ](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ) sample app. When creating your own project, please use this sample as a reference. This guide uses RTU (Ready to Use) APIs which aim to elevate the UI creation process with less code and offer a more pleasant and intuitive UI for your app.

## Supported Machine-Readable Travel Document Types

The Machine Readable Travel Document (MRTD) standard specified by the International Civil Aviation Organization (ICAO) defines how to encode information for optical character recognition on official travel documents.

Currently, the SDK supports three types of MRTD:

> Note: If you need support for other types of MRTDs, our SDK can be easily customized. Please contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/) if you have such a request.

### ID (TD1 Size)

The MRZ (Machine Readable Zone) in TD1 format consists of 3 lines with 30 characters in each line.

<div>
   <img src="../../assets/td1-id.png" alt="Example of MRZ in TD1 format" width="60%" />
</div>

### ID (TD2 Size)

The MRZ (Machine Readable Zone) in TD2 format consists of 2 lines with 36 characters in each line.

<div>
   <img src="../../assets/td2-id.png" alt="Example of MRZ in TD2 format" width="72%" />
</div>

### Passport (TD3 Size)

The MRZ (Machine Readable Zone) in TD3 format consists of 2 lines with 44 characters in each line.

<div>
   <img src="../../assets/td3-passport.png" alt="Example of MRZ in TD2 format" width="88%" />
</div>

## Requirements

- Supported OS:  **Android 5.0** (API Level 21) or higher.
- Supported ABI: **armeabi-v7a**, **arm64-v8a**, **x86** and **x86_64**.
- Development Environment: **Android Studio 2022.2.1** or higher.

## Add the SDK

1. Open the file `[App Project Root Path]\app\build.gradle` and add the Maven repository:

   ```groovy
   allprojects {
      repositories {
         maven {
            url "https://download2.dynamsoft.com/maven/aar"
         }
      }
   }
   ```

2. Add the references in the dependencies:

   ```groovy
   dependencies {
      implementation 'com.dynamsoft:mrzscannerbundle:2.0.2'
   }
   ```

3. Click **Sync Now**. After the synchronization is complete, the SDK is added to the project.

## Step 1: Create a New Project

The first thing that we are going to do is to create a fresh new project. Here are the steps on how to quickly do that

1. Open Android Studio and select **File > New > New Project**.

2. Choose the correct template for your project. In this sample, we use **Empty Views Activity**.

3. When prompted, set your app name to *ScanMRZ* and set the **Save** location, **Language**, and **Minimum SDK** (we use 21 here).

> Note:
>
> - With **minSdkVersion** set to 21, your app is compatible with more than 99.6% of devices on the Google Play Store (last update: October 2023).

## Step 2: Include the Library

Please read [Add the SDK](#add-the-sdk) section for instructions on how to add the SDK to your Android project.

## Step 3: Get Prepare for the Layout File

Open your **activity_main.xml** and replace it with the following code. In the layout file, we prepared 2 UI elements:

- A `TextView` for displaying the MRZ scanning result.
- A "START SCANNING" button for opening the scanner view.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tv_result"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Empty list"
        android:textSize="20sp" />

    <Button
        android:id="@+id/btn_navigate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="30dp"
        android:text="START SCANNING" />
</RelativeLayout>
```

## Step 4: Initialize the License

The first step in code configuration is to include a valid license in the `MRZScannerConfig` object, which is used when launching the scanner.

We first start with the package imports and then proceed to implementing the MainActivity class. That begins with configuring the `TextView` to display the MRZ document info once it is recognized, followed by defining the license via the `setLicense` method of `MRZScannerConfig`.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
package com.dynamsoft.scanmrz;
import android.os.Bundle;
import android.widget.TextView;
import com.dynamsoft.mrzscanner.ui.MRZScanResult;
import com.dynamsoft.mrzscanner.ui.MRZScannerActivity;
import com.dynamsoft.mrzscanner.ui.MRZScannerConfig;
import com.dynamsoft.core.basic_structures.DSRect;
import androidx.activity.result.ActivityResultLauncher;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
public class MainActivity extends AppCompatActivity {
   private ActivityResultLauncher<MRZScannerConfig> launcher;
   @Override
   protected void onCreate(@Nullable Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          TextView textView = findViewById(R.id.tv_result);
          MRZScannerConfig config = new MRZScannerConfig();
          config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
   }
}
```
2. 
```kotlin
package com.dynamsoft.scanmrz
import android.os.Bundle
import android.view.View
import android.widget.TextView
import androidx.activity.result.ActivityResultLauncher
import androidx.appcompat.app.AppCompatActivity
import com.dynamsoft.mrzscanner.ui.MRZScanResult
import com.dynamsoft.mrzscanner.ui.MRZScannerActivity
import com.dynamsoft.mrzscanner.ui.MRZScannerConfig
import com.dynamsoft.core.basic_structures.DSRect
class MainActivity : AppCompatActivity() {
   private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
   override fun onCreate(savedInstanceState: Bundle?) {
          super.onCreate(savedInstanceState)
          setContentView(R.layout.activity_main)
          val textView = findViewById<TextView>(R.id.tv_result)
          val config = MRZScannerConfig().apply {
             license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"
          }
   }
}
```

>Note:
>
>- The license string here grants a time-limited free trial which requires network connection to work.
>- You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=guide&package=ios){:target="_blank"} link.

## Step 5: Implementing the MRZ Scanner

Now that the MRZ Scanner is configured and the license has been set, it is time to implement the actions (via the `launcher`) to take when a MRZ is scanned. Once the launcher is called, the MRZ Scanner opens the camera and begins the scanning process.

Each result comes with a `resultStatus` that can be one of *RS_FINISHED*, *RS_CANCELED*, or *RS_EXCEPTION*. The first, *RS_FINISHED*, indicates that the result has been decoded and is available - while *RS_CANCELED* indicates that the operation has been halted. If *RS_EXCEPTION* is the result status, then that means that an error has occurred during the MRZ scanning process.

Once a MRZ is found, the content of the extracted information from the MRZ is outputted to the `TextView` object set up earlier. Continuing the code from step 3:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
public class MainActivity extends AppCompatActivity {
   private ActivityResultLauncher<MRZScannerConfig> launcher;
   @Override
   protected void onCreate(@Nullable Bundle savedInstanceState) {
          /* CONTINUATION OF THE CODE FROM STEP 3 */
          launcher = registerForActivityResult(new MRZScannerActivity.ResultContract(), result -> {
             if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_FINISHED && result.getData() != null) {
                String content = "Result: document type: " + result.getData().getDocumentType() + "\n" + "document number: " + result.getData().getDocumentNumber();
                textView.setText(content);
             } else if(result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_CANCELED ){
                textView.setText("Scan canceled.");
             }
             if (result.getErrorString() != null && !result.getErrorString().isEmpty()) {
                textView.setText(result.getErrorString());
             }
          });
         findViewById(R.id.btn_navigate).setOnClickListener(v -> launcher.launch(config));
   }
}
```
2. 
```kotlin
class MainActivity : AppCompatActivity() {
   private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
   override fun onCreate(savedInstanceState: Bundle?) {
       launcher = registerForActivityResult(MRZScannerActivity.ResultContract()) { result ->
          if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_FINISHED && result.data != null) {
             val content = """
             Result: document type: ${result.data.documentType}
             document number: ${result.data.documentNumber}
             """.trimIndent()
             textView.text = content
          } else if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_CANCELED) {
             textView.text = "Scan canceled."
          }
          if (result.errorString != null && result.errorString.isNotEmpty()) {
             textView.text = result.errorString
          }
       }
       findViewById<View>(R.id.btn_navigate).setOnClickListener {
          launcher.launch(config)
       }
   }
}
```

## Step 6: Configure the MRZ Scanner (optional)

This next step, although optional, is highly recommended to help achieve a more smooth-looking and intuitive UI. In this setup we will configure the visibility of the torch button as well as the close button. To do this, we are going back to the `MRZScannerConfig` object we used to define the license, and will make use of some of the other parameters available in the `MRZScannerConfig` class.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
public class MainActivity extends AppCompatActivity {
   private ActivityResultLauncher<MRZScannerConfig> launcher;
   @Override
   protected void onCreate(@Nullable Bundle savedInstanceState) {
          /* CONTINUATION OF THE CODE FROM STEP 3 */
          MRZScannerConfig config = new MRZScannerConfig();
          config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
          // You can use the following code to specify the document type to scan.
          config.setDocumentType(EnumDocumentType.DT_PASSPORT);
          // If you have a customized template file, please put it under "src\main\assets\Templates\" and call the following code. You can also use a JSON string as the `templateFile`.
          config.setTemplateFile("CustomizedTemplate.json");
          // The following code enables the beep sound when an MRZ is scanned.
          config.setBeepEnabled = true
          // The following code controls whether to display a torch button.
          config.setTorchButtonVisible(true);
          // The following code controls whether to display a close button.
          config.setCloseButtonVisible(true);
          /* CONTINUATION OF THE CODE FROM STEP 4 */
   }
}
```
2. 
```kotlin
class MainActivity : AppCompatActivity() {
   private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
   override fun onCreate(savedInstanceState: Bundle?) {
          /* CONTINUATION OF THE CODE FROM STEP 3 */
          val config = MRZScannerConfig().apply {
             license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9";
             // You can use the following code to specify the document type to scan.
             documentType = EnumDocumentType.DT_PASSPORT
             // If you have a customized template file, please put it under "src\main\assets\Templates\" and call the following code. You can also use a JSON string as the `templateFile`.
             templateFile = "CustomizedTemplate.json"
             // Add the following line to disable the beep sound.
             isBeepEnabled = false
             // Add the following line if you don't want to display the torch button.
             isTorchButtonVisible = false
             // Add the following line if you don't want to display the close button.
             isCloseButtonVisible = false
          }
          /* CONTINUATION OF THE CODE FROM STEP 4 */
   }
}
```

## Step 7: Run the Project

Now that the code has been written and the project complete, it's time to run the project. During setup, all of the gradle settings should have already been configured for you, so pretty much all you need to do now is to connect a physical Android device, select the proper configuration, and click Run.

## Conclusion

Now that your RTU project is up and running you should be able to see a clean and simplified UI that contains all the necessary UI elements that are needed to make the MRZ scanning process as easy and intuitive for the user as it can be.

## Next Steps

If you have any questions in regards to the usage of the new specialized SDK, do not hesitate to get in touch with the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).
