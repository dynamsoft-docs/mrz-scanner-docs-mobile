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

# MRZ Scanner User Guide (Android Edition)

The Dynamsoft MRZ Scanner (Android Edition) provides a ready-to-use scanning component that lets you add MRZ reading to your app with minimal setup. This guide walks through building a complete MRZ scanning app from scratch using `MRZScannerActivity` — the built-in activity that handles the camera UI, scanning logic, and result delivery.

> [!IMPORTANT]
> For the full sample code, visit the [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ).

## Supported Document Types

The SDK supports three ICAO Machine Readable Travel Document (MRTD) formats: **TD1** (ID cards, 3-line MRZ), **TD2** (ID cards, 2-line MRZ), and **TD3** (passports, 2-line MRZ). For a visual reference of each format, see [Supported Document Types](../../shared/supported-document-types.md).

> [!NOTE]
> For support for other MRTD types, contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).

## System Requirements

- Supported OS: **Android 5.0** (API Level 21) or higher.
- Supported ABI: **armeabi-v7a**, **arm64-v8a**, **x86** and **x86_64**.
- Development Environment:
   - IDE: **Android Studio 2024.3.2** suggested.
   - JDK: **Java 17** or higher.
   - Gradle: **8.0** or higher.

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

2. Add the dependency:

   ```groovy
   dependencies {
      implementation 'com.dynamsoft:mrzscannerbundle:3.4.1000'
   }
   ```

3. Click **Sync Now**. After the synchronization completes, the SDK is added to the project.

## Building the MRZ Scanner Application

The following steps build the **ScanMRZ** sample app. You can also download the complete project from the [GitHub repo](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ).

### Step 1: Create a New Project

1. Open Android Studio and select **File > New > New Project**.
2. Choose **Empty Views Activity** as the project template.
3. Set the app name to *ScanMRZ*, choose a save location and language, and set the **Minimum SDK** to 21.

### Step 2: Add the SDK

Follow the instructions in the [Add the SDK](#add-the-sdk) section above to add `mrzscannerbundle` to your project.

### Step 3: Set Up the Layout

Open **activity_main.xml** and replace its contents with the following. The layout contains a single "Scan an MRZ" button centered on the screen. Scan results will be shown in a separate `ResultActivity`, created in [Step 6](#step-6-display-the-results).

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.appcompat.widget.AppCompatButton
        android:id="@+id/btn_start"
        android:text="Scan an MRZ"
        android:textSize="18sp"
        android:padding="16dp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### Step 4: Configure the Scanner

All scanner settings are controlled through a single `MRZScannerConfig` object. Declare it as a class field so it can be shared between the launcher and any re-scan calls.

The only required setting is the license key. If you are just getting started, request a free 30-day trial license below:

{% include trialLicense.html %}

All other settings are optional and can be omitted to use their defaults. The code below shows the full set of available options with their default values noted in comments:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1.
```java
package com.dynamsoft.scanmrz;

import android.annotation.SuppressLint;
import android.os.Bundle;
import androidx.activity.EdgeToEdge;
import androidx.activity.result.ActivityResultLauncher;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import com.dynamsoft.mrzscannerbundle.ui.EnumDocumentType;
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerActivity;
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerConfig;

public class MainActivity extends AppCompatActivity {
   private ActivityResultLauncher<MRZScannerConfig> launcher;
   private final MRZScannerConfig config = new MRZScannerConfig();

   @SuppressLint("RestrictedApi")
   @Override
   protected void onCreate(@Nullable Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      EdgeToEdge.enable(this);
      setContentView(R.layout.activity_main);
      ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
         Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
         v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
         return insets;
      });

      // Required: set a valid license key.
      config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");

      // Optional: restrict scanning to a specific document type (default: DT_ALL).
      config.setDocumentType(EnumDocumentType.DT_PASSPORT);
      // Optional: load a custom template from src/main/assets/Templates/ or pass a JSON string.
      config.setTemplateFile("CustomizedTemplate.json");

      // Optional: feedback on successful scan (both default to false).
      config.setBeepEnabled(true);
      config.setVibrateEnabled(true);

      // Optional: control which buttons appear on the scanner UI.
      config.setTorchButtonVisible(true);           // Torch toggle (default: true).
      config.setCloseButtonVisible(true);           // Close/back button (default: true).
      config.setCameraToggleButtonVisible(true);    // Front/back camera toggle (default: true).
      config.setBeepButtonVisible(true);            // Beep on/off toggle (default: true).
      config.setVibrateButtonVisible(true);         // Vibrate on/off toggle (default: true).
      config.setFormatSelectorVisible(true);        // Document format selector (default: true).

      // Optional: choose which images to return with the result.
      config.setReturnDocumentImage(true);    // Cropped document image (default: true).
      config.setReturnPortraitImage(true);    // Portrait extracted from document (default: true).
      config.setReturnOriginalImage(false);   // Full camera frame (default: false).
   }
}
```
2.
```kotlin
package com.dynamsoft.scanmrz

import android.annotation.SuppressLint
import android.os.Bundle
import androidx.activity.EdgeToEdge
import androidx.activity.result.ActivityResultLauncher
import androidx.appcompat.app.AppCompatActivity
import androidx.core.graphics.Insets
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.dynamsoft.mrzscannerbundle.ui.EnumDocumentType
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerActivity
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerConfig

class MainActivity : AppCompatActivity() {
   private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
   private val config = MRZScannerConfig()

   @SuppressLint("RestrictedApi")
   override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      EdgeToEdge.enable(this)
      setContentView(R.layout.activity_main)
      ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
         val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
         v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
         insets
      }

      // Required: set a valid license key.
      config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9")

      config.apply {
         // Optional: restrict scanning to a specific document type (default: DT_ALL).
         setDocumentType(EnumDocumentType.DT_PASSPORT)
         // Optional: load a custom template from src/main/assets/Templates/ or pass a JSON string.
         setTemplateFile("CustomizedTemplate.json")

         // Optional: feedback on successful scan (both default to false).
         setBeepEnabled(true)
         setVibrateEnabled(true)

         // Optional: control which buttons appear on the scanner UI.
         setTorchButtonVisible(true)          // Torch toggle (default: true).
         setCloseButtonVisible(true)          // Close/back button (default: true).
         setCameraToggleButtonVisible(true)   // Front/back camera toggle (default: true).
         setBeepButtonVisible(true)           // Beep on/off toggle (default: true).
         setVibrateButtonVisible(true)        // Vibrate on/off toggle (default: true).
         setFormatSelectorVisible(true)       // Document format selector (default: true).

         // Optional: choose which images to return with the result.
         setReturnDocumentImage(true)    // Cropped document image (default: true).
         setReturnPortraitImage(true)    // Portrait extracted from document (default: true).
         setReturnOriginalImage(false)   // Full camera frame (default: false).
      }
   }
}
```

> [!NOTE]
>
> - The license string above grants a time-limited free trial which requires a network connection.
> - You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=guide&package=android){:target="_blank"} link.
> - The `@SuppressLint("RestrictedApi")` annotation is required because `retainAllImageInstances()` is annotated as a library-internal API. Adding it to the activity suppresses the lint warning.

### Step 5: Launch the Scanner

Register the `ActivityResultLauncher` and wire it to the scan button. Each result carries a `resultStatus` of *RS_FINISHED* (MRZ decoded), *RS_CANCELED* (user closed the scanner), or *RS_EXCEPTION* (an error occurred) — all three are handled inside `ResultActivity` in the next step.

Because `MRZScanResult` holds references to native image memory, you **must** call `result.retainAllImageInstances()` before passing it to another activity, otherwise the images may be garbage-collected before the next activity can use them.

Continuing from Step 4:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1.
```java
public class MainActivity extends AppCompatActivity {
   private ActivityResultLauncher<MRZScannerConfig> launcher;
   private final MRZScannerConfig config = new MRZScannerConfig();

   @SuppressLint("RestrictedApi")
   @Override
   protected void onCreate(@Nullable Bundle savedInstanceState) {
      /* CONTINUATION OF THE CODE FROM STEP 4 */
      launcher = registerForActivityResult(new MRZScannerActivity.ResultContract(), result -> {
         // MRZScanResult is Serializable and can be put directly into an Intent.
         // If the result contains images, call retainAllImageInstances() BEFORE
         // startActivity() to prevent native image memory from being recycled.
         Intent intent = new Intent(this, ResultActivity.class);
         result.retainAllImageInstances();
         intent.putExtra(ResultActivity.EXTRA_RESULT, result);
         startActivityForResult(intent, ResultActivity.REQUEST_CODE);
      });
      findViewById(R.id.btn_start).setOnClickListener(v -> launcher.launch(config));
   }

   @Override
   protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
      super.onActivityResult(requestCode, resultCode, data);
      if (requestCode == ResultActivity.REQUEST_CODE && resultCode == RESULT_OK && data != null) {
         int action = data.getIntExtra(ResultActivity.EXTRA_ACTION, ResultActivity.ACTION_RETURN_HOME);
         if (action == ResultActivity.ACTION_RESCAN) {
            launcher.launch(config);
         }
      }
   }
}
```
2.
```kotlin
class MainActivity : AppCompatActivity() {
   private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
   private val config = MRZScannerConfig()

   @SuppressLint("RestrictedApi")
   override fun onCreate(savedInstanceState: Bundle?) {
      /* CONTINUATION OF THE CODE FROM STEP 4 */
      launcher = registerForActivityResult(MRZScannerActivity.ResultContract()) { result ->
         // MRZScanResult is Serializable and can be put directly into an Intent.
         // If the result contains images, call retainAllImageInstances() BEFORE
         // startActivity() to prevent native image memory from being recycled.
         val intent = Intent(this, ResultActivity::class.java)
         result.retainAllImageInstances()
         intent.putExtra(ResultActivity.EXTRA_RESULT, result)
         startActivityForResult(intent, ResultActivity.REQUEST_CODE)
      }
      findViewById<View>(R.id.btn_start).setOnClickListener {
         launcher.launch(config)
      }
   }

   @Deprecated("Deprecated in Java")
   override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
      super.onActivityResult(requestCode, resultCode, data)
      if (requestCode == ResultActivity.REQUEST_CODE && resultCode == RESULT_OK && data != null) {
         val action = data.getIntExtra(ResultActivity.EXTRA_ACTION, ResultActivity.ACTION_RETURN_HOME)
         if (action == ResultActivity.ACTION_RESCAN) {
            launcher.launch(config)
         }
      }
   }
}
```

### Step 6: Display the Results

Create `ResultActivity` to receive and display the `MRZScanResult`. It handles all three result statuses and provides **Rescan** and **Return Home** actions.

First, create **activity_results.xml** in **src/main/res/layout/**. The layout has a scrollable result area (`result_view`), an error text view (`no_result_view`), and the action buttons — both result views start hidden and are toggled by the activity:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ResultActivity">

    <!-- Shown for RS_EXCEPTION results; hidden otherwise -->
    <TextView
        android:id="@+id/no_result_view"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:visibility="gone"
        android:gravity="center"
        android:textSize="16sp"
        app:layout_constraintBottom_toTopOf="@+id/ll_buttons"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <!-- Shown for RS_FINISHED results; hidden otherwise -->
    <ScrollView
        android:id="@+id/result_view"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:visibility="gone"
        app:layout_constraintBottom_toTopOf="@+id/ll_buttons"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="16dp">

            <!-- Header row: name/info on the left, portrait on the right -->
            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:minHeight="100dp"
                android:orientation="horizontal">

                <LinearLayout
                    android:layout_width="0dp"
                    android:layout_height="match_parent"
                    android:layout_weight="4"
                    android:gravity="center_vertical"
                    android:orientation="vertical">

                    <TextView
                        android:id="@+id/tv_full_name"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:textSize="24sp"
                        android:textStyle="bold" />

                    <TextView
                        android:id="@+id/tv_gender_and_age"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:textSize="14sp" />

                    <TextView
                        android:id="@+id/tv_nationality"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:textSize="14sp" />

                </LinearLayout>

                <ImageView
                    android:id="@+id/iv_portrait"
                    android:layout_width="0dp"
                    android:layout_height="match_parent"
                    android:layout_weight="1.2"
                    android:scaleType="centerCrop" />

            </LinearLayout>

            <TextView
                android:id="@+id/tv_doc_number"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:paddingTop="16dp" />

            <TextView
                android:id="@+id/tv_expiry_date"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:paddingTop="4dp" />

            <TextView
                android:id="@+id/tv_raw_mrz"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:fontFamily="monospace"
                android:paddingTop="16dp"
                android:paddingBottom="16dp" />

        </LinearLayout>
    </ScrollView>

    <LinearLayout
        android:id="@+id/ll_buttons"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="16dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent">

        <androidx.appcompat.widget.AppCompatButton
            android:id="@+id/btn_rescan"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:text="Re-Scan" />

        <androidx.appcompat.widget.AppCompatButton
            android:id="@+id/btn_return_home"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:text="Return Home" />

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

Next, declare `ResultActivity` in your **AndroidManifest.xml**:

```xml
<activity
    android:name=".ResultActivity"
    android:exported="true"
    android:screenOrientation="portrait" />
```

> [!NOTE]
> `MRZScannerActivity` is already declared in the library manifest with a default `screenOrientation` of `portrait`. If you need to override its orientation, redeclare it in your app manifest with `tools:replace="android:screenOrientation"`.

Now implement `ResultActivity`:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1.
```java
package com.dynamsoft.scanmrz;

import android.os.Bundle;
import android.view.View;
import android.widget.ImageView;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import com.dynamsoft.core.basic_structures.CoreException;
import com.dynamsoft.core.basic_structures.ImageData;
import com.dynamsoft.mrzscannerbundle.ui.EnumDocumentSide;
import com.dynamsoft.mrzscannerbundle.ui.MRZData;
import com.dynamsoft.mrzscannerbundle.ui.MRZScanResult;

public class ResultActivity extends AppCompatActivity {
   public static final int REQUEST_CODE = 1024;
   public static final String EXTRA_RESULT = "RESULT";
   public static final String EXTRA_ACTION = "ACTION";
   public static final int ACTION_RESCAN = 0;
   public static final int ACTION_RETURN_HOME = 1;

   @Override
   protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_results);
      ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
         Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
         v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
         return insets;
      });

      MRZScanResult scanResult = (MRZScanResult) getIntent().getSerializableExtra(EXTRA_RESULT);
      if (scanResult != null) {
         showMRZScanResult(scanResult);
      }

      findViewById(R.id.btn_rescan).setOnClickListener(v -> {
         setResult(RESULT_OK, getIntent().putExtra(EXTRA_ACTION, ACTION_RESCAN));
         finish();
      });
      findViewById(R.id.btn_return_home).setOnClickListener(v -> {
         setResult(RESULT_OK, getIntent().putExtra(EXTRA_ACTION, ACTION_RETURN_HOME));
         finish();
      });
   }

   private void showMRZScanResult(MRZScanResult result) {
      if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_CANCELED) {
         // User closed the scanner — return home without showing a result.
         setResult(RESULT_OK, getIntent().putExtra(EXTRA_ACTION, ACTION_RETURN_HOME));
         finish();
         return;
      }
      if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_EXCEPTION) {
         // Display the error message; hide the result view.
         findViewById(R.id.result_view).setVisibility(View.GONE);
         TextView tvNoResult = findViewById(R.id.no_result_view);
         tvNoResult.setVisibility(View.VISIBLE);
         tvNoResult.setText(result.getErrorString());
         return;
      }

      // RS_FINISHED — show the result view and display extracted MRZ data.
      findViewById(R.id.result_view).setVisibility(View.VISIBLE);
      findViewById(R.id.no_result_view).setVisibility(View.GONE);
      MRZData data = result.getData();

      // Personal information
      TextView tvFullName = findViewById(R.id.tv_full_name);
      tvFullName.setText(data.getFirstName() + " " + data.getLastName());
      TextView tvGenderAndAge = findViewById(R.id.tv_gender_and_age);
      tvGenderAndAge.setText(data.getSex() + ", " + data.getAge() + " years old");
      TextView tvNationality = findViewById(R.id.tv_nationality);
      tvNationality.setText(data.getNationality());

      // Document information
      TextView tvDocNumber = findViewById(R.id.tv_doc_number);
      tvDocNumber.setText(data.getDocumentNumber());
      TextView tvExpiry = findViewById(R.id.tv_expiry_date);
      tvExpiry.setText(data.getDateOfExpire());

      // Raw MRZ text
      TextView tvRawMRZ = findViewById(R.id.tv_raw_mrz);
      tvRawMRZ.setText(data.getMrzText());

      // Portrait image — show a placeholder if not available.
      ImageView ivPortrait = findViewById(R.id.iv_portrait);
      ImageData portraitImage = result.getPortraitImage();
      if (portraitImage != null) {
         try {
            ivPortrait.setImageBitmap(portraitImage.toBitmap());
         } catch (CoreException e) {
            e.printStackTrace();
         }
      } else {
         // Add a placeholder drawable to res/drawable/ and reference it here.
         ivPortrait.setImageResource(R.drawable.ic_portrait_placeholder);
      }

      // DS_MRZ = the side containing the MRZ; DS_OPPOSITE = the reverse side.
      // Cropped document images (nullable — only present if returnDocumentImage = true in config).
      ImageData mrzSideDocImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ);
      ImageData oppositeSideDocImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE);

      // Full original frame images (nullable — only present if returnOriginalImage = true in config).
      ImageData mrzSideOriginal = result.getOriginalImage(EnumDocumentSide.DS_MRZ);
      ImageData oppositeSideOriginal = result.getOriginalImage(EnumDocumentSide.DS_OPPOSITE);
   }
}
```
2.
```kotlin
package com.dynamsoft.scanmrz

import android.os.Bundle
import android.view.View
import android.widget.ImageView
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.core.graphics.Insets
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.dynamsoft.core.basic_structures.CoreException
import com.dynamsoft.core.basic_structures.ImageData
import com.dynamsoft.mrzscannerbundle.ui.EnumDocumentSide
import com.dynamsoft.mrzscannerbundle.ui.MRZData
import com.dynamsoft.mrzscannerbundle.ui.MRZScanResult

class ResultActivity : AppCompatActivity() {
   companion object {
      const val REQUEST_CODE = 1024
      const val EXTRA_RESULT = "RESULT"
      const val EXTRA_ACTION = "ACTION"
      const val ACTION_RESCAN = 0
      const val ACTION_RETURN_HOME = 1
   }

   override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_results)
      ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
         val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
         v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
         insets
      }

      val scanResult = intent.getSerializableExtra(EXTRA_RESULT) as? MRZScanResult
      scanResult?.let { showMRZScanResult(it) }

      findViewById<View>(R.id.btn_rescan).setOnClickListener {
         setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RESCAN))
         finish()
      }
      findViewById<View>(R.id.btn_return_home).setOnClickListener {
         setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RETURN_HOME))
         finish()
      }
   }

   private fun showMRZScanResult(result: MRZScanResult) {
      if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_CANCELED) {
         // User closed the scanner — return home without showing a result.
         setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RETURN_HOME))
         finish()
         return
      }
      if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_EXCEPTION) {
         // Display the error message; hide the result view.
         findViewById<View>(R.id.result_view).visibility = View.GONE
         val tvNoResult = findViewById<TextView>(R.id.no_result_view)
         tvNoResult.visibility = View.VISIBLE
         tvNoResult.text = result.errorString
         return
      }

      // RS_FINISHED — show the result view and display extracted MRZ data.
      findViewById<View>(R.id.result_view).visibility = View.VISIBLE
      findViewById<View>(R.id.no_result_view).visibility = View.GONE
      val data = result.data

      // Personal information
      val tvFullName = findViewById<TextView>(R.id.tv_full_name)
      tvFullName.text = "${data.firstName} ${data.lastName}"
      val tvGenderAndAge = findViewById<TextView>(R.id.tv_gender_and_age)
      tvGenderAndAge.text = "${data.sex}, ${data.age} years old"
      val tvNationality = findViewById<TextView>(R.id.tv_nationality)
      tvNationality.text = data.nationality

      // Document information
      val tvDocNumber = findViewById<TextView>(R.id.tv_doc_number)
      tvDocNumber.text = data.documentNumber
      val tvExpiry = findViewById<TextView>(R.id.tv_expiry_date)
      tvExpiry.text = data.dateOfExpire

      // Raw MRZ text
      val tvRawMRZ = findViewById<TextView>(R.id.tv_raw_mrz)
      tvRawMRZ.text = data.mrzText

      // Portrait image — show a placeholder if not available.
      val ivPortrait = findViewById<ImageView>(R.id.iv_portrait)
      val portraitImage = result.getPortraitImage()
      if (portraitImage != null) {
         try {
            ivPortrait.setImageBitmap(portraitImage.toBitmap())
         } catch (e: CoreException) {
            e.printStackTrace()
         }
      } else {
         // Add a placeholder drawable to res/drawable/ and reference it here.
         ivPortrait.setImageResource(R.drawable.ic_portrait_placeholder)
      }

      // DS_MRZ = the side containing the MRZ; DS_OPPOSITE = the reverse side.
      // Cropped document images (nullable — only present if returnDocumentImage = true in config).
      val mrzSideDocImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ)
      val oppositeSideDocImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE)

      // Full original frame images (nullable — only present if returnOriginalImage = true in config).
      val mrzSideOriginal = result.getOriginalImage(EnumDocumentSide.DS_MRZ)
      val oppositeSideOriginal = result.getOriginalImage(EnumDocumentSide.DS_OPPOSITE)
   }
}
```

> [!NOTE]
> - `EnumDocumentSide.DS_MRZ` refers to the side of the document containing the machine-readable zone; `DS_OPPOSITE` is the reverse side (relevant for two-sided documents like TD1 ID cards).
> - Image retrieval methods on `MRZScanResult` (`getDocumentImage()`, `getOriginalImage()`, `getPortraitImage()`) return `null` if the corresponding option was disabled in the config or if no image was captured for that side.
> - `ic_portrait_placeholder` referenced in the portrait fallback is a custom drawable. Add your own placeholder image to **res/drawable/** and reference it there.

For the full list of fields available on `MRZData`, see the [MRZData API reference](../api-reference/mrz-data.md).

### Step 7: Run the Project

Connect a physical Android device, select the run configuration, and click **Run**. When the scanner finishes, the result is passed to `ResultActivity`, where the extracted MRZ data and any captured images are displayed.

## Next Steps

- **Samples** — Explore the complete [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ).
- **Customize** — Learn how to configure document type, UI elements, and feedback in the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.
- **API Reference** — Browse the full [Android API Reference](../api-reference/index.md) for all classes and methods.
- **License** — See the [License Activation](license-activation.md) guide for production license setup.
- **Support** — Contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/) for help or custom requirements.
