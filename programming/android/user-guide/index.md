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
> For the full sample code, visit the [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ) (Java) or its Kotlin twin, [ScanMRZKt](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZKt).

## Supported Document Types

The SDK supports three ICAO Machine Readable Travel Document (MRTD) formats: **TD1** (ID cards, 3-line MRZ), **TD2** (ID cards, 2-line MRZ), and **TD3** (passports, 2-line MRZ). For a visual reference of each format, see [Supported Document Types](supported-document-types.md).

> [!NOTE]
> For support for other MRTD types, contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).

## System Requirements

- Supported OS: **Android 5.0** (API Level 21) or higher.
- Supported ABI: **armeabi-v7a**, **arm64-v8a**, **x86** and **x86_64**.
- Development Environment:
   - IDE: **Android Studio 2024.3.2** suggested.
   - JDK: **Java 17** or higher.
   - Gradle: **8.0** or higher.

## Licensing

A valid license key is required to use the SDK. If you are just getting started, request a free 30-day trial license below:

{% include trialLicense.html %}

> [!NOTE]
>
> - The license string above grants a time-limited free trial which requires a network connection.
> - You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&utm_source=guide&package=android){:target="_blank"} link.
> - For production license setup, see the [License Activation](license-activation.md) guide.

## Add the SDK

1. Open **settings.gradle** (Groovy DSL) or **settings.gradle.kts** (Kotlin DSL) at the project root and add the Dynamsoft Maven repository to the `dependencyResolutionManagement` block:

   <div class="sample-code-prefix"></div>
   >- Groovy
   >- Kotlin
   >
   >1. 
   ```groovy
   dependencyResolutionManagement {
           repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
           repositories {
               google()
               mavenCentral()
               maven {
                   url "https://download2.dynamsoft.com/maven/aar"
               }
           }
   }
   ```
   2. 
   ```kotlin
   dependencyResolutionManagement {
           repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
           repositories {
               google()
               mavenCentral()
               maven {
                   url = uri("https://download2.dynamsoft.com/maven/aar")
               }
           }
   }
   ```

2. Open **build.gradle** (Module: app) for Groovy DSL — or **build.gradle.kts** (Module: app) for Kotlin DSL — and add the dependency:

   <div class="sample-code-prefix"></div>
   >- Groovy
   >- Kotlin
   >
   >1. 
   ```groovy
   dependencies {
           implementation 'com.dynamsoft:mrzscannerbundle:3.6.2000'
   }
   ```
   2. 
   ```kotlin
   dependencies {
           implementation("com.dynamsoft:mrzscannerbundle:3.6.2000")
   }
   ```

3. Click **Sync Now**. After the synchronization completes, the SDK is added to the project.

## Building the MRZ Scanner Application

The following steps build the **ScanMRZ** sample app. You can also download the complete project from the GitHub repo, in [Java](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ) or [Kotlin](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZKt).

> [!NOTE]
> The Kotlin snippets in this guide use the package `com.dynamsoft.scanmrz`, matching the project name from Step 1. The downloadable Kotlin sample uses `com.dynamsoft.scanmrzkt` instead, so that it can be installed alongside the Java sample for comparison. The code is otherwise identical.

### Step 1: Create a New Project

1. Open Android Studio and select **File > New > New Project**.
2. Choose **Empty Views Activity** as the project template.
3. Set the app name to *ScanMRZ*, choose a save location, and set the **Minimum SDK** to 21.
4. Choose your preferred **Language** (either **Java** or **Kotlin**) and **Build configuration language** (either **Kotlin DSL** or **Groovy DSL**). The SDK supports all combinations — sample snippets later in this guide cover both languages and both DSLs.

### Step 2: Add the SDK

Follow the instructions in the [Add the SDK](#add-the-sdk) section above to add `mrzscannerbundle` to your project.

### Step 3: Set Up the Layout

Open **activity_main.xml** and replace its contents with the following. The layout contains a single "Scan an MRZ" button centered on the screen. Scan results will be shown in a separate `ResultActivity`, created in [Step 8](#step-8-implement-resultactivity).

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

### Step 4: Configure and Launch the Scanner

Provide your license key on `MRZScannerConfig` (see [Licensing](#licensing) for how to obtain one), register an `ActivityResultLauncher` to launch the scanner, and wire it to the scan button. Each result returns a `resultStatus` of *RS_FINISHED* (MRZ decoded), *RS_CANCELED* (user closed the scanner), or *RS_EXCEPTION* (an error occurred); all three are handled inside `ResultActivity`, created in later steps.

For optional config settings like document type filtering, UI visibility, and image capture, see the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.

You do not need to write any camera-permission code. The `CAMERA` permission is declared by the SDK and merged into your app at build time, and `MRZScannerActivity` requests it on first launch. If access is unavailable the scanner shows a dialog offering **Allow camera access** or **Open Settings**, never starts the camera, and reports the outcome through the usual result path as `RS_EXCEPTION` — handled in [Step 8](#step-8-implement-resultactivity).

> [!NOTE]
> To present your own permission UI instead, call `config.setCameraPermissionPromptEnabled(false)`. The scanner then suppresses its dialog but still reports the denial, and still never starts the camera without access. See [Customize MRZ Scanner](customize-mrz-scanner.md#handling-camera-permission) for the full flow.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
package com.dynamsoft.scanmrz;
import android.content.Intent;
import android.os.Bundle;
import androidx.activity.EdgeToEdge;
import androidx.activity.result.ActivityResultLauncher;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerActivity;
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerConfig;
public class MainActivity extends AppCompatActivity {
       private ActivityResultLauncher<MRZScannerConfig> launcher;
       private final MRZScannerConfig config = new MRZScannerConfig();
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
          config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
          launcher = registerForActivityResult(new MRZScannerActivity.ResultContract(), result -> {
             Intent intent = new Intent(this, ResultActivity.class);
             intent.putExtra(ResultActivity.EXTRA_RESULT, result);
             startActivityForResult(intent, ResultActivity.REQUEST_CODE);
          });
          findViewById(R.id.btn_start).setOnClickListener(v -> launcher.launch(config));
       }
       @Override
       protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
          super.onActivityResult(requestCode, resultCode, data);
          if (requestCode == ResultActivity.REQUEST_CODE && resultCode == RESULT_OK) {
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
package com.dynamsoft.scanmrz
import android.content.Intent
import android.os.Bundle
import android.view.View
import androidx.activity.enableEdgeToEdge
import androidx.activity.result.ActivityResultLauncher
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerActivity
import com.dynamsoft.mrzscannerbundle.ui.MRZScannerConfig
class MainActivity : AppCompatActivity() {
       private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
       private val config = MRZScannerConfig()
       override fun onCreate(savedInstanceState: Bundle?) {
          super.onCreate(savedInstanceState)
          enableEdgeToEdge()
          setContentView(R.layout.activity_main)
          ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
             val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
             v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
             insets
          }
          config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9")
          launcher = registerForActivityResult(MRZScannerActivity.ResultContract()) { result ->
             val intent = Intent(this, ResultActivity::class.java)
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

> [!NOTE]
> The Kotlin `enableEdgeToEdge()` extension requires **androidx.activity 1.8.0** or higher (fresh Android Studio projects from Hedgehog onward already meet this). On older versions, replace `enableEdgeToEdge()` with `EdgeToEdge.enable(this)` and update the import to `import androidx.activity.EdgeToEdge`.

> [!NOTE]
> References to `ResultActivity` (and its constants `EXTRA_RESULT`, `REQUEST_CODE`, `EXTRA_ACTION`, `ACTION_RESCAN`, `ACTION_RETURN_HOME`) will show as unresolved in your IDE at this point. This is expected — `ResultActivity` is created in [Step 8](#step-8-implement-resultactivity), and the errors will clear once that step is complete.

### Step 5: Create the Result Screen Layouts

This step creates the three UI resource files that `ResultActivity` needs, and adds one color to the project's existing **colors.xml**.

**activity_results.xml**

Create **activity_results.xml** in **src/main/res/layout/**. This layout contains the scrollable result area, an error text view for exception states, a header with the portrait and key identity details, a tab-based image pager for document photos, detailed personal and document info fields, the raw MRZ text, and the action buttons:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ResultActivity">

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
                    android:orientation="vertical"
                    android:paddingVertical="8dp">

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
                        android:id="@+id/tv_expiry"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:paddingTop="4dp"
                        android:textSize="14sp" />
                </LinearLayout>

                <View
                    android:layout_width="8dp"
                    android:layout_height="match_parent" />

                <ImageView
                    android:id="@+id/iv_portrait"
                    android:layout_width="0dp"
                    android:layout_height="match_parent"
                    android:layout_weight="1.2"
                    android:contentDescription="Portrait"
                    android:src="@drawable/ic_portrait_placeholder" />

            </LinearLayout>

            <com.google.android.material.tabs.TabLayout
                android:id="@+id/tab_images"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="24dp"
                app:tabGravity="center"
                app:tabMode="fixed" />

            <androidx.viewpager2.widget.ViewPager2
                android:id="@+id/vp_images"
                android:layout_width="match_parent"
                android:layout_height="162dp"
                android:layout_marginTop="8dp"
                android:overScrollMode="never" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:paddingTop="24dp"
                android:text="Personal Info"
                android:textStyle="bold" />

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Given Name" />

                <TextView
                    android:id="@+id/tv_given_name"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Surname" />

                <TextView
                    android:id="@+id/tv_surname"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Date of Birth" />

                <TextView
                    android:id="@+id/tv_date_of_birth"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Gender" />

                <TextView
                    android:id="@+id/tv_gender"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Nationality" />

                <TextView
                    android:id="@+id/tv_nationality"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:paddingTop="24dp"
                android:text="Document Info"
                android:textStyle="bold" />

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Doc. Type" />

                <TextView
                    android:id="@+id/tv_doc_type"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Doc. Number" />

                <TextView
                    android:id="@+id/tv_doc_number"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center_vertical"
                android:orientation="horizontal"
                android:paddingTop="8dp">

                <TextView
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:text="Expiry Date" />

                <TextView
                    android:id="@+id/tv_expiry_date"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:textStyle="bold" />
            </LinearLayout>

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:paddingTop="24dp"
                android:text="Raw MRZ Text"
                android:textStyle="bold" />

            <TextView
                android:id="@+id/tv_raw_mrz"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:fontFamily="monospace"
                android:paddingTop="8dp"
                android:paddingBottom="16dp" />

        </LinearLayout>
    </ScrollView>

    <LinearLayout
        android:id="@+id/ll_buttons"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="16dp"
        android:layout_marginBottom="16dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent">

        <androidx.appcompat.widget.AppCompatButton
            android:id="@+id/btn_rescan"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:text="Re-Scan" />

        <!-- Hidden by default. Takes the Re-Scan slot when the scan failed because
             camera access is unavailable — see Step 8. -->
        <androidx.appcompat.widget.AppCompatButton
            android:id="@+id/btn_open_settings"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:visibility="gone"
            android:text="Open Settings" />

        <androidx.appcompat.widget.AppCompatButton
            android:id="@+id/btn_return_home"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:text="Return Home" />

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

**ic_portrait_placeholder.xml**

Create **ic_portrait_placeholder.xml** in **src/main/res/drawable/**. This vector drawable is shown as the portrait fallback when no portrait image was captured:

```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="88dp"
    android:height="88dp"
    android:viewportWidth="88"
    android:viewportHeight="88">
    <path
        android:pathData="M79.382,75.625C79.141,76.043 78.794,76.39 78.375,76.632C77.957,76.873 77.483,77 77,77H11C10.517,76.999 10.044,76.872 9.626,76.631C9.208,76.389 8.862,76.042 8.621,75.624C8.38,75.206 8.253,74.732 8.253,74.249C8.253,73.767 8.38,73.293 8.621,72.875C13.857,63.824 21.924,57.334 31.34,54.257C26.682,51.485 23.064,47.26 21.04,42.232C19.016,37.204 18.699,31.651 20.137,26.425C21.574,21.199 24.688,16.59 28.999,13.305C33.31,10.02 38.58,8.241 44,8.241C49.42,8.241 54.69,10.02 59.001,13.305C63.312,16.59 66.426,21.199 67.863,26.425C69.301,31.651 68.984,37.204 66.96,42.232C64.936,47.26 61.318,51.485 56.66,54.257C66.076,57.334 74.143,63.824 79.379,72.875C79.621,73.293 79.748,73.767 79.749,74.249C79.749,74.732 79.623,75.207 79.382,75.625Z"
        android:fillColor="#000000"/>
</vector>
```

**colors.xml**

Open **colors.xml** in **src/main/res/values/** and add the amber used to flag MRZ fields that fail validation. Both the error icon below and `ResultActivity` ([Step 8](#step-8-implement-resultactivity)) reference it:

```xml
<color name="warning_amber">#FFC107</color>
```

**ic_error_circle.xml**

Create **ic_error_circle.xml** in **src/main/res/drawable/**. This vector drawable is the Material *error* glyph, displayed next to any field whose check digit fails validation:

```xml
<?xml version="1.0" encoding="utf-8"?>
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="24dp"
    android:height="24dp"
    android:viewportWidth="24"
    android:viewportHeight="24">
    <path
        android:fillColor="@color/warning_amber"
        android:pathData="M12,2C6.48,2 2,6.48 2,12s4.48,10 10,10 10,-4.48 10,-10S17.52,2 12,2zM13,17h-2v-2h2v2zM13,13h-2V7h2v6z" />
</vector>
```

> [!NOTE]
> A circle-exclamation is used rather than a warning triangle because a failed check digit is an invalid-data state, not a caution. This matches Material's convention of reserving the triangle for warnings.

### Step 6: Register ResultActivity in the Manifest

Open **AndroidManifest.xml** and declare `ResultActivity` inside the `<application>` block:

```xml
<activity
    android:name=".ResultActivity"
    android:exported="true"
    android:screenOrientation="portrait" />
```

> [!NOTE]
> `MRZScannerActivity` is already declared in the library manifest with a default `screenOrientation` of `portrait`. If you need to override its orientation, redeclare it in your app manifest with `tools:replace="android:screenOrientation"`.

### Step 7: Create ImagesFragment

`ImagesFragment` is a `Fragment` that programmatically renders one or two document images side by side. It is used by the `ViewPager2` adapter in `ResultActivity` to display cropped and original scan images.

In Android Studio, right-click the package folder (`com.dynamsoft.scanmrz`) in the **Project** pane and select **New > Java Class** (or **New > Kotlin File/Class** for Kotlin), then name it `ImagesFragment`.

The images reach the fragment through `setArguments(Bundle)` rather than a constructor. `FragmentManager` recreates fragments by reflection, so it needs a public no-arg constructor and can only restore state it finds in the arguments `Bundle`. Because `ImageData` is not itself serializable into a `Bundle`, each image is encoded to JPEG bytes on the way in and decoded on the way out.

> [!IMPORTANT]
> Do not pass `ImageData` through the fragment's constructor. That form compiles, but `ResultActivity` will crash at `super.onCreate(...)` whenever the activity is recreated — after a configuration change, under **Don't keep activities**, or following process death. You can reproduce it by enabling **Don't keep activities** in Developer Options, completing a scan, then backgrounding and reopening the app.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
package com.dynamsoft.scanmrz;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.os.Bundle;
import android.view.Gravity;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.LinearLayout;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;
import com.dynamsoft.core.basic_structures.CoreException;
import com.dynamsoft.core.basic_structures.ImageData;
import java.io.ByteArrayOutputStream;
public class ImagesFragment extends Fragment {
       private static final String ARG_IMAGE_1 = "image1";
       private static final String ARG_IMAGE_2 = "image2";
       // The JPEG quality and maximum dimension below keep the Bundle payload well under
       // the ~1 MB Binder transaction limit that carries saved instance state.
       private static final int JPEG_QUALITY = 85;
       private static final int MAX_DIMENSION_PX = 1024;
       // A public no-arg constructor is required: FragmentManager recreates fragments by
       // reflection. The images are passed through setArguments(Bundle) so they survive
       // configuration changes and process death.
       public ImagesFragment() {
          super();
       }
       @NonNull
       public static ImagesFragment newInstance(@Nullable ImageData imageData1, @Nullable ImageData imageData2) {
          ImagesFragment fragment = new ImagesFragment();
          Bundle args = new Bundle();
          byte[] bytes1 = encode(imageData1);
          byte[] bytes2 = encode(imageData2);
          if (bytes1 != null) args.putByteArray(ARG_IMAGE_1, bytes1);
          if (bytes2 != null) args.putByteArray(ARG_IMAGE_2, bytes2);
          fragment.setArguments(args);
          return fragment;
       }
       @Nullable
       private static byte[] encode(@Nullable ImageData imageData) {
          if (imageData == null) return null;
          try {
             Bitmap bmp = downscaleIfNeeded(imageData.toBitmap());
             ByteArrayOutputStream out = new ByteArrayOutputStream();
             bmp.compress(Bitmap.CompressFormat.JPEG, JPEG_QUALITY, out);
             return out.toByteArray();
          } catch (CoreException e) {
             e.printStackTrace();
             return null;
          }
       }
       @NonNull
       private static Bitmap downscaleIfNeeded(@NonNull Bitmap src) {
          int w = src.getWidth();
          int h = src.getHeight();
          int max = Math.max(w, h);
          if (max <= MAX_DIMENSION_PX) return src;
          float scale = (float) MAX_DIMENSION_PX / max;
          return Bitmap.createScaledBitmap(src, Math.round(w * scale), Math.round(h * scale), true);
       }
       @Nullable
       @Override
       public View onCreateView(@NonNull LayoutInflater inflater,
                                @Nullable ViewGroup container,
                                @Nullable Bundle savedInstanceState) {
          LinearLayout root = new LinearLayout(requireContext());
          root.setLayoutParams(new LinearLayout.LayoutParams(
                  ViewGroup.LayoutParams.MATCH_PARENT,
                  ViewGroup.LayoutParams.MATCH_PARENT
          ));
          root.setOrientation(LinearLayout.HORIZONTAL);
          root.setGravity(Gravity.CENTER_VERTICAL);
          root.setBaselineAligned(false);
          root.setClipToPadding(false);
          root.setClipChildren(false);
          return root;
       }
       @Override
       public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
          Bundle args = getArguments();
          if (args == null) return;
          LinearLayout root = (LinearLayout) view;
          byte[] bytes1 = args.getByteArray(ARG_IMAGE_1);
          byte[] bytes2 = args.getByteArray(ARG_IMAGE_2);
          addImageView(root, bytes1);
          if (bytes1 != null && bytes2 != null) {
             // 16dp spacer between the two images
             root.addView(new View(requireContext()),
                     new LinearLayout.LayoutParams(
                             (int) (16 * getResources().getDisplayMetrics().density),
                             ViewGroup.LayoutParams.MATCH_PARENT));
          }
          addImageView(root, bytes2);
       }
       private void addImageView(@NonNull LinearLayout root, @Nullable byte[] bytes) {
          if (bytes == null) return;
          Bitmap bmp = BitmapFactory.decodeByteArray(bytes, 0, bytes.length);
          if (bmp == null) return;
          ImageView iv = new ImageView(requireContext());
          LinearLayout.LayoutParams lp = new LinearLayout.LayoutParams(
                  0, ViewGroup.LayoutParams.MATCH_PARENT, 1f);
          iv.setLayoutParams(lp);
          iv.setScaleType(ImageView.ScaleType.FIT_CENTER);
          iv.setAdjustViewBounds(true);
          iv.setImageBitmap(bmp);
          root.addView(iv);
       }
}
```
2. 
```kotlin
package com.dynamsoft.scanmrz
import android.graphics.Bitmap
import android.graphics.BitmapFactory
import android.os.Bundle
import android.view.Gravity
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.LinearLayout
import androidx.fragment.app.Fragment
import com.dynamsoft.core.basic_structures.CoreException
import com.dynamsoft.core.basic_structures.ImageData
import java.io.ByteArrayOutputStream
import kotlin.math.max
import kotlin.math.roundToInt
// Kotlin supplies the required public no-arg constructor for free: FragmentManager
// recreates fragments by reflection. The images are passed through setArguments(Bundle)
// so they survive configuration changes and process death. Do not be tempted to hand
// ImageData to a Kotlin primary constructor instead.
class ImagesFragment : Fragment() {
       override fun onCreateView(
          inflater: LayoutInflater,
          container: ViewGroup?,
          savedInstanceState: Bundle?
       ): View {
          val root = LinearLayout(requireContext())
          root.layoutParams = LinearLayout.LayoutParams(
             ViewGroup.LayoutParams.MATCH_PARENT,
             ViewGroup.LayoutParams.MATCH_PARENT
          )
          root.orientation = LinearLayout.HORIZONTAL
          root.gravity = Gravity.CENTER_VERTICAL
          root.isBaselineAligned = false
          root.clipToPadding = false
          root.clipChildren = false
          return root
       }
       override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
          val args = arguments ?: return
          val root = view as LinearLayout
          val bytes1 = args.getByteArray(ARG_IMAGE_1)
          val bytes2 = args.getByteArray(ARG_IMAGE_2)
          addImageView(root, bytes1)
          if (bytes1 != null && bytes2 != null) {
             // 16dp spacer between the two images
             root.addView(
                View(requireContext()),
                LinearLayout.LayoutParams(
                   (16 * resources.displayMetrics.density).toInt(),
                   ViewGroup.LayoutParams.MATCH_PARENT
                )
             )
          }
          addImageView(root, bytes2)
       }
       private fun addImageView(root: LinearLayout, bytes: ByteArray?) {
          if (bytes == null) return
          val bmp = BitmapFactory.decodeByteArray(bytes, 0, bytes.size) ?: return
          val iv = ImageView(requireContext())
          iv.layoutParams = LinearLayout.LayoutParams(
             0, ViewGroup.LayoutParams.MATCH_PARENT, 1f
          )
          iv.scaleType = ImageView.ScaleType.FIT_CENTER
          iv.adjustViewBounds = true
          iv.setImageBitmap(bmp)
          root.addView(iv)
       }
       companion object {
          private const val ARG_IMAGE_1 = "image1"
          private const val ARG_IMAGE_2 = "image2"
          // The JPEG quality and maximum dimension below keep the Bundle payload well
          // under the ~1 MB Binder transaction limit that carries saved instance state.
          private const val JPEG_QUALITY = 85
          private const val MAX_DIMENSION_PX = 1024
          fun newInstance(imageData1: ImageData?, imageData2: ImageData?): ImagesFragment {
             val fragment = ImagesFragment()
             val args = Bundle()
             encode(imageData1)?.let { args.putByteArray(ARG_IMAGE_1, it) }
             encode(imageData2)?.let { args.putByteArray(ARG_IMAGE_2, it) }
             fragment.arguments = args
             return fragment
          }
          private fun encode(imageData: ImageData?): ByteArray? {
             if (imageData == null) return null
             return try {
                val bmp = downscaleIfNeeded(imageData.toBitmap())
                val out = ByteArrayOutputStream()
                bmp.compress(Bitmap.CompressFormat.JPEG, JPEG_QUALITY, out)
                out.toByteArray()
             } catch (e: CoreException) {
                e.printStackTrace()
                null
             }
          }
          private fun downscaleIfNeeded(src: Bitmap): Bitmap {
             val w = src.width
             val h = src.height
             val maxDimension = max(w, h)
             if (maxDimension <= MAX_DIMENSION_PX) return src
             val scale = MAX_DIMENSION_PX.toFloat() / maxDimension
             return Bitmap.createScaledBitmap(src, (w * scale).roundToInt(), (h * scale).roundToInt(), true)
          }
       }
}
```

### Step 8: Implement ResultActivity

Create **ResultActivity** in the same package folder using the same steps as above — right-click the package folder and select **New > Java Class** (or **New > Kotlin File/Class**), then name it `ResultActivity`. It receives the `MRZScanResult` passed from `MainActivity`, handles all three result statuses, and populates the result screen with the extracted MRZ data, portrait image, and document images.

Two parts of this activity are worth reading closely.

**Per-field validation.** Each MRZ field carries its own validation status, retrieved with [`getFieldValidationStatus`](../api-reference/mrz-data.md). The `applyField` helper renders any field whose status is `VS_FAILED` in amber with a trailing error icon, underlines it to signal that it is tappable, and opens a short explanation when tapped. A failed status means the value does not match its check digit — the document may be invalid or altered — so the value is shown rather than hidden, letting you decide whether to accept it, prompt for a re-scan, or ask for manual correction.

Note that the top summary block is deliberately left unstyled. It combines several values on one line, so flagging it on a single field's status would imply that everything on that line is suspect.

**Camera-permission failures.** When the scanner cannot start because camera access was denied, it returns `RS_EXCEPTION` with an error code of `EC_CAMERA_PERMISSION_DENIED` or `EC_CAMERA_PERMISSION_RESTRICTED`. `showCameraPermissionAction` replaces **Re-Scan** with **Open Settings** for the first of these, and `onResume` re-checks the permission so that granting it in Settings and returning starts a new scan instead of leaving a stale error on screen.

> [!NOTE]
> **Open Settings** is offered only for `EC_CAMERA_PERMISSION_DENIED`. `EC_CAMERA_PERMISSION_RESTRICTED` means device policy withholds the camera, and the per-app camera toggle is absent from Settings in that state, so sending the user there would be a dead end. See [`MRZScanResult`](../api-reference/mrz-scan-result.md) for both codes.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
package com.dynamsoft.scanmrz;
import android.Manifest;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.graphics.drawable.Drawable;
import android.net.Uri;
import android.os.Bundle;
import android.provider.Settings;
import android.text.SpannableString;
import android.text.Spanned;
import android.text.style.ImageSpan;
import android.text.style.UnderlineSpan;
import android.view.View;
import android.widget.ImageView;
import android.widget.TextView;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.ContextCompat;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import androidx.fragment.app.Fragment;
import androidx.viewpager2.adapter.FragmentStateAdapter;
import androidx.viewpager2.widget.ViewPager2;
import com.dynamsoft.core.basic_structures.CoreException;
import com.dynamsoft.core.basic_structures.ImageData;
import com.dynamsoft.dcp.EnumValidationStatus;
import com.dynamsoft.mrzscannerbundle.ui.EnumDocumentSide;
import com.dynamsoft.mrzscannerbundle.ui.MRZData;
import com.dynamsoft.mrzscannerbundle.ui.MRZScanResult;
import com.google.android.material.tabs.TabLayout;
import com.google.android.material.tabs.TabLayoutMediator;
public class ResultActivity extends AppCompatActivity {
       public static final int REQUEST_CODE = 1024;
       public static final String EXTRA_RESULT = "RESULT";
       public static final String EXTRA_ACTION = "ACTION";
       public static final int ACTION_RESCAN = 0;
       public static final int ACTION_RETURN_HOME = 1;
       // True while this screen is showing a camera-permission denial rather than a result.
       private boolean isShowingCameraPermissionError = false;
       @Override
       protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_results);
          ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
             Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
             v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
             return insets;
          });
          MRZScanResult scanResult = (MRZScanResult) getIntent().getParcelableExtra(EXTRA_RESULT);
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
       @Override
       protected void onResume() {
          super.onResume();
          // The user may have granted camera access in Settings and come straight back.
          // Leaving a stale "access denied" message on screen would tell them to fix
          // something they have just fixed, so hand control back for another scan.
          if (isShowingCameraPermissionError && hasCameraPermission()) {
             setResult(RESULT_OK, getIntent().putExtra(EXTRA_ACTION, ACTION_RESCAN));
             finish();
          }
       }
       private boolean hasCameraPermission() {
          return ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA)
                  == PackageManager.PERMISSION_GRANTED;
       }
       private void showMRZScanResult(MRZScanResult result) {
          if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_CANCELED) {
             setResult(RESULT_OK, getIntent().putExtra(EXTRA_ACTION, ACTION_RETURN_HOME));
             finish();
             return;
          }
          if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_EXCEPTION) {
             findViewById(R.id.result_view).setVisibility(View.GONE);
             TextView tvNoResult = findViewById(R.id.no_result_view);
             tvNoResult.setVisibility(View.VISIBLE);
             tvNoResult.setText(result.getErrorString());
             showCameraPermissionAction(result.getErrorCode());
             return;
          }
          findViewById(R.id.result_view).setVisibility(View.VISIBLE);
          findViewById(R.id.no_result_view).setVisibility(View.GONE);
          MRZData data = result.getData();
          // Sex can be empty when the field was not parsed, so capitalize only if non-empty.
          String sexText = data.getSex();
          String genderText = sexText.isEmpty() ? "" : sexText.substring(0, 1).toUpperCase() + sexText.substring(1).toLowerCase();
          // The top summary is a plain overview with no validation highlighting. Field-level
          // validation is surfaced by the Personal Info and Document Info sections below;
          // flagging a compound line such as "gender, age" on one field's status would
          // imply both values are invalid.
          ((TextView) findViewById(R.id.tv_full_name)).setText((data.getFirstName() + " " + data.getLastName()).trim());
          ((TextView) findViewById(R.id.tv_gender_and_age)).setText(
                  genderText.isEmpty() && data.getAge() == 0
                          ? ""
                          : genderText + ", " + data.getAge() + " years old");
          ((TextView) findViewById(R.id.tv_expiry)).setText(data.getDateOfExpire().isEmpty() ? "" : "Expiry: " + data.getDateOfExpire());
          ImageView ivPortrait = findViewById(R.id.iv_portrait);
          ImageData portraitImage = result.getPortraitImage();
          if (portraitImage != null) {
             try {
                ivPortrait.setImageBitmap(portraitImage.toBitmap());
             } catch (CoreException ignored) {
             }
          } else {
             ivPortrait.setImageResource(R.drawable.ic_portrait_placeholder);
          }
          // Images view pager
          showImages(result);
          // Personal info
          applyField(findViewById(R.id.tv_given_name), data.getFirstName(), data.getFieldValidationStatus("firstName"));
          applyField(findViewById(R.id.tv_surname), data.getLastName(), data.getFieldValidationStatus("lastName"));
          applyField(findViewById(R.id.tv_date_of_birth), data.getDateOfBirth(), data.getFieldValidationStatus("dateOfBirth"));
          applyField(findViewById(R.id.tv_gender), genderText, data.getFieldValidationStatus("sex"));
          applyField(findViewById(R.id.tv_nationality), data.getNationality(), data.getFieldValidationStatus("nationality"));
          // Document info
          String docTypeText;
          switch (data.getDocumentType() == null ? "" : data.getDocumentType()) {
             case "MRTD_TD1_ID":       docTypeText = "ID (TD1)"; break;
             case "MRTD_TD2_ID":       docTypeText = "ID (TD2)"; break;
             case "MRTD_TD3_PASSPORT": docTypeText = "Passport (TD3)"; break;
             default:                  docTypeText = ""; break;
          }
          // documentType comes from the MRZ code type, not an independently validated field.
          applyField(findViewById(R.id.tv_doc_type), docTypeText, EnumValidationStatus.VS_NONE);
          applyField(findViewById(R.id.tv_doc_number), data.getDocumentNumber(), data.getFieldValidationStatus("documentNumber"));
          applyField(findViewById(R.id.tv_expiry_date), data.getDateOfExpire(), data.getFieldValidationStatus("dateOfExpire"));
          // The raw MRZ text is tappable too: a line-level failure can flag the raw MRZ when
          // no individual field failed, for example corruption in a field that carries no
          // check digit of its own such as name, nationality or sex.
          applyField(findViewById(R.id.tv_raw_mrz), data.getMrzText(), data.getFieldValidationStatus("mrzText"));
       }
       // Renders value into tv, appending a circular error icon and colouring the row amber
       // when validation failed. Empty values render as "N/A" so it is clear which fields the
       // parser could not extract at all. Failed rows are tappable and open an explanation.
       private void applyField(TextView tv, String value, int status) {
          boolean failed = status == EnumValidationStatus.VS_FAILED;
          boolean empty = value == null || value.isEmpty();
          String text = empty ? "N/A" : value;
          if (failed) {
             SpannableString spannable = new SpannableString(text + "  ￼");
             Drawable icon = ContextCompat.getDrawable(this, R.drawable.ic_error_circle);
             int iconSize = Math.round(tv.getTextSize() * 1.2f);
             icon.setBounds(0, 0, iconSize, iconSize);
             spannable.setSpan(new ImageSpan(icon, ImageSpan.ALIGN_BOTTOM),
                     spannable.length() - 1, spannable.length(), Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
             spannable.setSpan(new UnderlineSpan(), 0, text.length(), Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
             tv.setText(spannable);
          } else {
             tv.setText(text);
          }
          tv.setTextColor(ContextCompat.getColor(this, failed ? R.color.warning_amber : R.color.white));
          if (failed) {
             tv.setOnClickListener(v -> showValidationInfoDialog());
          } else {
             tv.setOnClickListener(null);
             tv.setClickable(false);
          }
       }
       private void showValidationInfoDialog() {
          new AlertDialog.Builder(this)
                  .setTitle("Field validation warning")
                  .setMessage("This value doesn't match its check digit. The document may be invalid or altered.")
                  .setPositiveButton("OK", null)
                  .show();
       }
       // Swaps Re-Scan for Open Settings when the scan failed because camera access was
       // unavailable. Re-Scan is dropped deliberately: reaching this screen means the user
       // already saw the scanner's own permission dialog and cancelled it, so retrying would
       // only replay what they declined, and once the denial is permanent it loops back here.
       private void showCameraPermissionAction(int errorCode) {
          // EC_CAMERA_PERMISSION_RESTRICTED means device policy withholds the camera and
          // Settings has no toggle to offer, so leave both buttons hidden in that case.
          if (errorCode != MRZScanResult.EnumErrorCode.EC_CAMERA_PERMISSION_DENIED) {
             return;
          }
          isShowingCameraPermissionError = true;
          findViewById(R.id.btn_rescan).setVisibility(View.GONE);
          View btnOpenSettings = findViewById(R.id.btn_open_settings);
          btnOpenSettings.setVisibility(View.VISIBLE);
          btnOpenSettings.setOnClickListener(v -> startActivity(
                  new Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS,
                          Uri.fromParts("package", getPackageName(), null))));
       }
       private void showImages(MRZScanResult result) {
          ImageData mrzSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ);
          ImageData oppositeSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE);
          ImageData mrzSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_MRZ);
          ImageData oppositeSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_OPPOSITE);
          TabLayout tabImages = findViewById(R.id.tab_images);
          ViewPager2 vpImages = findViewById(R.id.vp_images);
          // A tab is shown only when its own set of images came back. Original images are off
          // by default, so normally only "Processed" appears — call
          // config.setReturnOriginalImage(true) in MainActivity to get both.
          boolean hasProcessed = mrzSideDocumentImage != null || oppositeSideDocumentImage != null;
          boolean hasOriginal = mrzSideOriginalImage != null || oppositeSideOriginalImage != null;
          if (!hasProcessed && !hasOriginal) {
             tabImages.setVisibility(View.GONE);
             vpImages.setVisibility(View.GONE);
             return;
          }
          tabImages.setVisibility(View.VISIBLE);
          vpImages.setVisibility(View.VISIBLE);
          vpImages.setAdapter(new FragmentStateAdapter(this) {
             @NonNull
             @Override
             public Fragment createFragment(int position) {
                // Page 0 is the processed pair whenever processed images exist; otherwise
                // the single page is the original pair.
                if (position == 0 && hasProcessed) {
                   return ImagesFragment.newInstance(mrzSideDocumentImage, oppositeSideDocumentImage);
                } else {
                   return ImagesFragment.newInstance(mrzSideOriginalImage, oppositeSideOriginalImage);
                }
             }
             @Override
             public int getItemCount() {
                return hasProcessed && hasOriginal ? 2 : 1;
             }
          });
          // Always attached, so the surviving tab is still labelled when there is only one.
          new TabLayoutMediator(tabImages, vpImages, (tab, position) -> {
             if (position == 0 && hasProcessed) {
                tab.setText("Processed");
             } else {
                tab.setText("Original");
             }
          }).attach();
       }
}
```
2. 
```kotlin
package com.dynamsoft.scanmrz
import android.Manifest
import android.content.Intent
import android.content.pm.PackageManager
import android.net.Uri
import android.os.Bundle
import android.provider.Settings
import android.text.SpannableString
import android.text.Spanned
import android.text.style.ImageSpan
import android.text.style.UnderlineSpan
import android.view.View
import android.widget.ImageView
import android.widget.TextView
import androidx.appcompat.app.AlertDialog
import androidx.appcompat.app.AppCompatActivity
import androidx.core.content.ContextCompat
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import androidx.fragment.app.Fragment
import androidx.viewpager2.adapter.FragmentStateAdapter
import androidx.viewpager2.widget.ViewPager2
import com.dynamsoft.core.basic_structures.CoreException
import com.dynamsoft.dcp.EnumValidationStatus
import com.dynamsoft.mrzscannerbundle.ui.EnumDocumentSide
import com.dynamsoft.mrzscannerbundle.ui.MRZScanResult
import com.google.android.material.tabs.TabLayout
import com.google.android.material.tabs.TabLayoutMediator
import kotlin.math.roundToInt
class ResultActivity : AppCompatActivity() {
       // True while this screen is showing a camera-permission denial rather than a result.
       private var isShowingCameraPermissionError = false
       override fun onCreate(savedInstanceState: Bundle?) {
          super.onCreate(savedInstanceState)
          setContentView(R.layout.activity_results)
          ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
             val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
             v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
             insets
          }
          @Suppress("DEPRECATION")
          val scanResult = intent.getParcelableExtra<MRZScanResult>(EXTRA_RESULT)
          if (scanResult != null) {
             showMRZScanResult(scanResult)
          }
          findViewById<View>(R.id.btn_rescan).setOnClickListener {
             setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RESCAN))
             finish()
          }
          findViewById<View>(R.id.btn_return_home).setOnClickListener {
             setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RETURN_HOME))
             finish()
          }
       }
       override fun onResume() {
          super.onResume()
          // The user may have granted camera access in Settings and come straight back.
          // Leaving a stale "access denied" message on screen would tell them to fix
          // something they have just fixed, so hand control back for another scan.
          if (isShowingCameraPermissionError && hasCameraPermission()) {
             setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RESCAN))
             finish()
          }
       }
       private fun hasCameraPermission(): Boolean =
          ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA) ==
                  PackageManager.PERMISSION_GRANTED
       private fun showMRZScanResult(result: MRZScanResult) {
          if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_CANCELED) {
             setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RETURN_HOME))
             finish()
             return
          }
          if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_EXCEPTION) {
             findViewById<View>(R.id.result_view).visibility = View.GONE
             val tvNoResult = findViewById<TextView>(R.id.no_result_view)
             tvNoResult.visibility = View.VISIBLE
             tvNoResult.text = result.errorString
             showCameraPermissionAction(result.errorCode)
             return
          }
          findViewById<View>(R.id.result_view).visibility = View.VISIBLE
          findViewById<View>(R.id.no_result_view).visibility = View.GONE
          val data = result.data
          // Sex can be empty when the field was not parsed, so capitalize only if non-empty.
          val sexText = data.sex
          val genderText = if (sexText.isEmpty()) ""
          else sexText.substring(0, 1).uppercase() + sexText.substring(1).lowercase()
          // The top summary is a plain overview with no validation highlighting. Field-level
          // validation is surfaced by the Personal Info and Document Info sections below;
          // flagging a compound line such as "gender, age" on one field's status would
          // imply both values are invalid.
          findViewById<TextView>(R.id.tv_full_name).text = (data.firstName + " " + data.lastName).trim()
          findViewById<TextView>(R.id.tv_gender_and_age).text =
             if (genderText.isEmpty() && data.age == 0) ""
             else "$genderText, ${data.age} years old"
          findViewById<TextView>(R.id.tv_expiry).text =
             if (data.dateOfExpire.isEmpty()) "" else "Expiry: ${data.dateOfExpire}"
          val ivPortrait = findViewById<ImageView>(R.id.iv_portrait)
          val portraitImage = result.portraitImage
          if (portraitImage != null) {
             try {
                ivPortrait.setImageBitmap(portraitImage.toBitmap())
             } catch (ignored: CoreException) {
             }
          } else {
             ivPortrait.setImageResource(R.drawable.ic_portrait_placeholder)
          }
          // Images view pager
          showImages(result)
          // Personal info
          applyField(findViewById(R.id.tv_given_name), data.firstName, data.getFieldValidationStatus("firstName"))
          applyField(findViewById(R.id.tv_surname), data.lastName, data.getFieldValidationStatus("lastName"))
          applyField(findViewById(R.id.tv_date_of_birth), data.dateOfBirth, data.getFieldValidationStatus("dateOfBirth"))
          applyField(findViewById(R.id.tv_gender), genderText, data.getFieldValidationStatus("sex"))
          applyField(findViewById(R.id.tv_nationality), data.nationality, data.getFieldValidationStatus("nationality"))
          // Document info
          val docTypeText = when (data.documentType ?: "") {
             "MRTD_TD1_ID" -> "ID (TD1)"
             "MRTD_TD2_ID" -> "ID (TD2)"
             "MRTD_TD3_PASSPORT" -> "Passport (TD3)"
             else -> ""
          }
          // documentType comes from the MRZ code type, not an independently validated field.
          applyField(findViewById(R.id.tv_doc_type), docTypeText, EnumValidationStatus.VS_NONE)
          applyField(findViewById(R.id.tv_doc_number), data.documentNumber, data.getFieldValidationStatus("documentNumber"))
          applyField(findViewById(R.id.tv_expiry_date), data.dateOfExpire, data.getFieldValidationStatus("dateOfExpire"))
          // The raw MRZ text is tappable too: a line-level failure can flag the raw MRZ when
          // no individual field failed, for example corruption in a field that carries no
          // check digit of its own such as name, nationality or sex.
          applyField(findViewById(R.id.tv_raw_mrz), data.mrzText, data.getFieldValidationStatus("mrzText"))
       }
       // Renders value into tv, appending a circular error icon and colouring the row amber
       // when validation failed. Empty values render as "N/A" so it is clear which fields the
       // parser could not extract at all. Failed rows are tappable and open an explanation.
       private fun applyField(tv: TextView, value: String?, status: Int) {
          val failed = status == EnumValidationStatus.VS_FAILED
          val text = if (value.isNullOrEmpty()) "N/A" else value
          if (failed) {
             val spannable = SpannableString("$text  ￼")
             val icon = ContextCompat.getDrawable(this, R.drawable.ic_error_circle)!!
             val iconSize = (tv.textSize * 1.2f).roundToInt()
             icon.setBounds(0, 0, iconSize, iconSize)
             spannable.setSpan(
                ImageSpan(icon, ImageSpan.ALIGN_BOTTOM),
                spannable.length - 1, spannable.length, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE
             )
             spannable.setSpan(UnderlineSpan(), 0, text.length, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE)
             tv.text = spannable
          } else {
             tv.text = text
          }
          tv.setTextColor(
             ContextCompat.getColor(this, if (failed) R.color.warning_amber else R.color.white)
          )
          if (failed) {
             tv.setOnClickListener { showValidationInfoDialog() }
          } else {
             tv.setOnClickListener(null)
             tv.isClickable = false
          }
       }
       private fun showValidationInfoDialog() {
          AlertDialog.Builder(this)
             .setTitle("Field validation warning")
             .setMessage("This value doesn't match its check digit. The document may be invalid or altered.")
             .setPositiveButton("OK", null)
             .show()
       }
       // Swaps Re-Scan for Open Settings when the scan failed because camera access was
       // unavailable. Re-Scan is dropped deliberately: reaching this screen means the user
       // already saw the scanner's own permission dialog and cancelled it, so retrying would
       // only replay what they declined, and once the denial is permanent it loops back here.
       private fun showCameraPermissionAction(errorCode: Int) {
          // EC_CAMERA_PERMISSION_RESTRICTED means device policy withholds the camera and
          // Settings has no toggle to offer, so leave both buttons hidden in that case.
          if (errorCode != MRZScanResult.EnumErrorCode.EC_CAMERA_PERMISSION_DENIED) {
             return
          }
          isShowingCameraPermissionError = true
          findViewById<View>(R.id.btn_rescan).visibility = View.GONE
          val btnOpenSettings = findViewById<View>(R.id.btn_open_settings)
          btnOpenSettings.visibility = View.VISIBLE
          btnOpenSettings.setOnClickListener {
             startActivity(
                Intent(
                   Settings.ACTION_APPLICATION_DETAILS_SETTINGS,
                   Uri.fromParts("package", packageName, null)
                )
             )
          }
       }
       private fun showImages(result: MRZScanResult) {
          val mrzSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ)
          val oppositeSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE)
          val mrzSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_MRZ)
          val oppositeSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_OPPOSITE)
          val tabImages = findViewById<TabLayout>(R.id.tab_images)
          val vpImages = findViewById<ViewPager2>(R.id.vp_images)
          // A tab is shown only when its own set of images came back. Original images are off
          // by default, so normally only "Processed" appears — set
          // config.isReturnOriginalImage = true in MainActivity to get both.
          val hasProcessed = mrzSideDocumentImage != null || oppositeSideDocumentImage != null
          val hasOriginal = mrzSideOriginalImage != null || oppositeSideOriginalImage != null
          if (!hasProcessed && !hasOriginal) {
             tabImages.visibility = View.GONE
             vpImages.visibility = View.GONE
             return
          }
          tabImages.visibility = View.VISIBLE
          vpImages.visibility = View.VISIBLE
          vpImages.adapter = object : FragmentStateAdapter(this) {
             override fun createFragment(position: Int): Fragment {
                // Page 0 is the processed pair whenever processed images exist; otherwise
                // the single page is the original pair.
                return if (position == 0 && hasProcessed) {
                   ImagesFragment.newInstance(mrzSideDocumentImage, oppositeSideDocumentImage)
                } else {
                   ImagesFragment.newInstance(mrzSideOriginalImage, oppositeSideOriginalImage)
                }
             }
             override fun getItemCount(): Int {
                return if (hasProcessed && hasOriginal) 2 else 1
             }
          }
          // Always attached, so the surviving tab is still labelled when there is only one.
          TabLayoutMediator(tabImages, vpImages) { tab, position ->
             tab.text = if (position == 0 && hasProcessed) "Processed" else "Original"
          }.attach()
       }
       companion object {
          const val REQUEST_CODE = 1024
          const val EXTRA_RESULT = "RESULT"
          const val EXTRA_ACTION = "ACTION"
          const val ACTION_RESCAN = 0
          const val ACTION_RETURN_HOME = 1
       }
}
```

> [!NOTE]
> - `EnumDocumentSide.DS_MRZ` refers to the side of the document containing the machine-readable zone; `DS_OPPOSITE` is the reverse side (relevant for two-sided documents like TD1 ID cards).
> - Image retrieval methods on `MRZScanResult` (`getDocumentImage()`, `getOriginalImage()`, `getPortraitImage()`) return `null` if the corresponding option was disabled in the config or if no image was captured for that side.

For the full list of fields available on `MRZData`, see the [MRZData API reference](../api-reference/mrz-data.md).

### Step 9: Run the Project

Before running, complete these steps on your Android device:

1. **Enable USB Debugging** — Go to **Settings > About Phone** and tap **Build Number** seven times to unlock Developer Options. Then go to **Settings > Developer Options** and enable **USB Debugging**.

2. **Connect your device** — Connect your Android device to your development machine via USB. If prompted on the device, tap **Allow** to authorize the debugging connection.

3. **Select your device** — In Android Studio, select your connected device from the run configuration dropdown at the top of the IDE.

4. **Click Run.**

When the scanner finishes, the result is passed to `ResultActivity`, where the extracted MRZ data and any captured images are displayed.

> [!NOTE]
> A physical Android device is required. The camera is not available on the Android Emulator.

## Next Steps

- **Samples** — Explore the complete sample on GitHub, in [Java](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ) or [Kotlin](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZKt), or browse the [Demo and Samples](../samples/index.md) page.
- **Customize** — Learn how to configure document type, UI elements, and feedback in the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.
- **API Reference** — Browse the full [Android API Reference](../api-reference/index.md) for all classes and methods.
- **License** — See the [License Activation](license-activation.md) guide for production license setup.
- **Support** — Contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/) for help or custom requirements.
