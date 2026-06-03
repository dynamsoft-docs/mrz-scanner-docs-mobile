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
       implementation 'com.dynamsoft:mrzscannerbundle:3.4.1300'
   }
   ```
   2. 
   ```kotlin
   dependencies {
       implementation("com.dynamsoft:mrzscannerbundle:3.4.1300")
   }
   ```

3. Click **Sync Now**. After the synchronization completes, the SDK is added to the project.

## Building the MRZ Scanner Application

The following steps build the **ScanMRZ** sample app. You can also download the complete project from the [GitHub repo](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ).

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
import androidx.core.graphics.Insets
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

This step creates all three UI resource files that `ResultActivity` needs.

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

        <androidx.appcompat.widget.AppCompatButton
            android:id="@+id/btn_return_home"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:text="Return Home" />

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

**image_view_pager.xml**

Create **image_view_pager.xml** in **src/main/res/layout/**. This layout defines the tab bar and pager used to display scanned document images:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="8dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <com.google.android.material.tabs.TabLayout
            android:id="@+id/tab_images"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabGravity="fill"
            app:tabMode="fixed" />

        <androidx.viewpager2.widget.ViewPager2
            android:id="@+id/vp_images"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:overScrollMode="never" />

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

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
package com.dynamsoft.scanmrz;
import android.graphics.Bitmap;
import android.os.Bundle;
import android.view.Gravity;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.LinearLayout;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;
import com.dynamsoft.core.basic_structures.CoreException;
import com.dynamsoft.core.basic_structures.ImageData;
public class ImagesFragment extends Fragment {
       private final ImageData imageData1;
       private final ImageData imageData2;
       public ImagesFragment(ImageData imageData1, ImageData imageData2) {
          super();
          this.imageData1 = imageData1;
          this.imageData2 = imageData2;
       }
       @NonNull
       public static ImagesFragment newInstance(@Nullable ImageData imageData1, @Nullable ImageData imageData2) {
          return new ImagesFragment(imageData1, imageData2);
       }
       @Nullable
       @Override
       public View onCreateView(@NonNull android.view.LayoutInflater inflater,
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
          LinearLayout root = (LinearLayout) view;
          ImageData[] imageDatas = new ImageData[]{imageData1, imageData2};
          for (int i = 0; i < imageDatas.length; i++) {
             ImageData imageData = imageDatas[i];
             if (imageData1 != null && imageData2 != null && i == 1) {
                root.addView(new View(requireContext()),
                        new LinearLayout.LayoutParams(
                                (int)(16 * getResources().getDisplayMetrics().density),
                                ViewGroup.LayoutParams.MATCH_PARENT));
             }
             if (imageData != null) {
                try {
                   Bitmap bmp = imageData.toBitmap();
                   ImageView iv = new ImageView(requireContext());
                   LinearLayout.LayoutParams lp = new LinearLayout.LayoutParams(0, ViewGroup.LayoutParams.MATCH_PARENT, 1f);
                   iv.setLayoutParams(lp);
                   iv.setScaleType(ImageView.ScaleType.FIT_CENTER);
                   iv.setAdjustViewBounds(true);
                   iv.setImageBitmap(bmp);
                   root.addView(iv);
                } catch (CoreException e) {
                   e.printStackTrace();
                }
             }
          }
       }
}
```
2. 
```kotlin
package com.dynamsoft.scanmrz
import android.graphics.Bitmap
import android.os.Bundle
import android.view.Gravity
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.LinearLayout
import androidx.fragment.app.Fragment
import com.dynamsoft.core.basic_structures.CoreException
import com.dynamsoft.core.basic_structures.ImageData
class ImagesFragment(
       private val imageData1: ImageData?,
       private val imageData2: ImageData?
) : Fragment() {
       companion object {
          fun newInstance(imageData1: ImageData?, imageData2: ImageData?): ImagesFragment {
             return ImagesFragment(imageData1, imageData2)
          }
       }
       override fun onCreateView(
          inflater: android.view.LayoutInflater,
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
          val root = view as LinearLayout
          val imageDatas = arrayOf(imageData1, imageData2)
          for (i in imageDatas.indices) {
             val imageData = imageDatas[i]
             if (imageData1 != null && imageData2 != null && i == 1) {
                root.addView(
                   View(requireContext()),
                   LinearLayout.LayoutParams(
                      (16 * resources.displayMetrics.density).toInt(),
                      ViewGroup.LayoutParams.MATCH_PARENT
                   )
                )
             }
             if (imageData != null) {
                try {
                   val bmp: Bitmap = imageData.toBitmap()
                   val iv = ImageView(requireContext())
                   val lp = LinearLayout.LayoutParams(0, ViewGroup.LayoutParams.MATCH_PARENT, 1f)
                   iv.layoutParams = lp
                   iv.scaleType = ImageView.ScaleType.FIT_CENTER
                   iv.adjustViewBounds = true
                   iv.setImageBitmap(bmp)
                   root.addView(iv)
                } catch (e: CoreException) {
                   e.printStackTrace()
                }
             }
          }
       }
}
```

### Step 8: Implement ResultActivity

Create **ResultActivity** in the same package folder using the same steps as above — right-click the package folder and select **New > Java Class** (or **New > Kotlin File/Class**), then name it `ResultActivity`. It receives the `MRZScanResult` passed from `MainActivity`, handles all three result statuses, and populates the result screen with the extracted MRZ data, portrait image, and document images.

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
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import androidx.fragment.app.Fragment;
import androidx.viewpager2.adapter.FragmentStateAdapter;
import androidx.viewpager2.widget.ViewPager2;
import com.dynamsoft.core.basic_structures.CoreException;
import com.dynamsoft.core.basic_structures.ImageData;
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
             return;
          }
          findViewById(R.id.result_view).setVisibility(View.VISIBLE);
          findViewById(R.id.no_result_view).setVisibility(View.GONE);
          MRZData data = result.getData();
          String genderText = data.getSex().substring(0, 1).toUpperCase() + data.getSex().substring(1).toLowerCase();
          // Main info
          TextView tvFullName = findViewById(R.id.tv_full_name);
          tvFullName.setText(data.getFirstName() + " " + data.getLastName());
          TextView tvGenderAndAge = findViewById(R.id.tv_gender_and_age);
          tvGenderAndAge.setText(genderText + ", " + data.getAge() + " years old");
          TextView tvExpiry = findViewById(R.id.tv_expiry);
          tvExpiry.setText("Expiry: " + data.getDateOfExpire());
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
          TextView tvGivenName = findViewById(R.id.tv_given_name);
          tvGivenName.setText(data.getFirstName());
          TextView tvSurname = findViewById(R.id.tv_surname);
          tvSurname.setText(data.getLastName());
          TextView tvDateOfBirth = findViewById(R.id.tv_date_of_birth);
          tvDateOfBirth.setText(data.getDateOfBirth());
          TextView tvGender = findViewById(R.id.tv_gender);
          tvGender.setText(genderText);
          TextView tvNationality = findViewById(R.id.tv_nationality);
          tvNationality.setText(data.getNationality());
          // Document info
          TextView tvDocType = findViewById(R.id.tv_doc_type);
          switch (data.getDocumentType()) {
             case "MRTD_TD1_ID":
                tvDocType.setText("ID (TD1)");
                break;
             case "MRTD_TD2_ID":
                tvDocType.setText("ID (TD2)");
                break;
             case "MRTD_TD3_PASSPORT":
                tvDocType.setText("Passport (TD3)");
                break;
          }
          TextView tvDocNumber = findViewById(R.id.tv_doc_number);
          tvDocNumber.setText(data.getDocumentNumber());
          TextView tvExpiryDate = findViewById(R.id.tv_expiry_date);
          tvExpiryDate.setText(data.getDateOfExpire());
          // Raw MRZ text
          TextView tvRawMRZ = findViewById(R.id.tv_raw_mrz);
          tvRawMRZ.setText(data.getMrzText());
       }
       private void showImages(MRZScanResult result) {
          ImageData mrzSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ);
          ImageData oppositeSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE);
          ImageData mrzSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_MRZ);
          ImageData oppositeSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_OPPOSITE);
          TabLayout tabImages = findViewById(R.id.tab_images);
          ViewPager2 vpImages = findViewById(R.id.vp_images);
          if (mrzSideDocumentImage == null && mrzSideOriginalImage == null) {
             tabImages.setVisibility(View.GONE);
             vpImages.setVisibility(View.GONE);
             return;
          } else {
             tabImages.setVisibility(View.VISIBLE);
             vpImages.setVisibility(View.VISIBLE);
          }
          vpImages.setAdapter(new FragmentStateAdapter(this) {
             @NonNull
             @Override
             public Fragment createFragment(int position) {
                if (position == 0 && mrzSideDocumentImage != null) {
                   return ImagesFragment.newInstance(mrzSideDocumentImage, oppositeSideDocumentImage);
                } else {
                   return ImagesFragment.newInstance(mrzSideOriginalImage, oppositeSideOriginalImage);
                }
             }
             @Override
             public int getItemCount() {
                if (mrzSideDocumentImage != null && mrzSideOriginalImage != null) {
                   return 2;
                } else {
                   return 1;
                }
             }
          });
          if (mrzSideOriginalImage != null || oppositeSideOriginalImage != null) {
             new TabLayoutMediator(tabImages, vpImages, (tab, position) -> {
                if (position == 0 && mrzSideDocumentImage != null) {
                   tab.setText("Processed");
                } else {
                   tab.setText("Original");
                }
             }).attach();
          }
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
import androidx.fragment.app.Fragment
import androidx.viewpager2.adapter.FragmentStateAdapter
import androidx.viewpager2.widget.ViewPager2
import com.dynamsoft.core.basic_structures.CoreException
import com.dynamsoft.core.basic_structures.ImageData
import com.dynamsoft.mrzscannerbundle.ui.EnumDocumentSide
import com.dynamsoft.mrzscannerbundle.ui.MRZData
import com.dynamsoft.mrzscannerbundle.ui.MRZScanResult
import com.google.android.material.tabs.TabLayout
import com.google.android.material.tabs.TabLayoutMediator
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
          val scanResult = intent.getParcelableExtra<MRZScanResult>(EXTRA_RESULT)
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
             setResult(RESULT_OK, intent.putExtra(EXTRA_ACTION, ACTION_RETURN_HOME))
             finish()
             return
          }
          if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_EXCEPTION) {
             findViewById<View>(R.id.result_view).visibility = View.GONE
             val tvNoResult = findViewById<TextView>(R.id.no_result_view)
             tvNoResult.visibility = View.VISIBLE
             tvNoResult.text = result.errorString
             return
          }
          findViewById<View>(R.id.result_view).visibility = View.VISIBLE
          findViewById<View>(R.id.no_result_view).visibility = View.GONE
          val data = result.data
          val genderText = data.sex.substring(0, 1).uppercase() + data.sex.substring(1).lowercase()
          // Main info
          val tvFullName = findViewById<TextView>(R.id.tv_full_name)
          tvFullName.text = "${data.firstName} ${data.lastName}"
          val tvGenderAndAge = findViewById<TextView>(R.id.tv_gender_and_age)
          tvGenderAndAge.text = "$genderText, ${data.age} years old"
          val tvExpiry = findViewById<TextView>(R.id.tv_expiry)
          tvExpiry.text = "Expiry: ${data.dateOfExpire}"
          val ivPortrait = findViewById<ImageView>(R.id.iv_portrait)
          val portraitImage = result.getPortraitImage()
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
          val tvGivenName = findViewById<TextView>(R.id.tv_given_name)
          tvGivenName.text = data.firstName
          val tvSurname = findViewById<TextView>(R.id.tv_surname)
          tvSurname.text = data.lastName
          val tvDateOfBirth = findViewById<TextView>(R.id.tv_date_of_birth)
          tvDateOfBirth.text = data.dateOfBirth
          val tvGender = findViewById<TextView>(R.id.tv_gender)
          tvGender.text = genderText
          val tvNationality = findViewById<TextView>(R.id.tv_nationality)
          tvNationality.text = data.nationality
          // Document info
          val tvDocType = findViewById<TextView>(R.id.tv_doc_type)
          when (data.documentType) {
             "MRTD_TD1_ID" -> tvDocType.text = "ID (TD1)"
             "MRTD_TD2_ID" -> tvDocType.text = "ID (TD2)"
             "MRTD_TD3_PASSPORT" -> tvDocType.text = "Passport (TD3)"
          }
          val tvDocNumber = findViewById<TextView>(R.id.tv_doc_number)
          tvDocNumber.text = data.documentNumber
          val tvExpiryDate = findViewById<TextView>(R.id.tv_expiry_date)
          tvExpiryDate.text = data.dateOfExpire
          // Raw MRZ text
          val tvRawMRZ = findViewById<TextView>(R.id.tv_raw_mrz)
          tvRawMRZ.text = data.mrzText
       }
       private fun showImages(result: MRZScanResult) {
          val mrzSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ)
          val oppositeSideDocumentImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE)
          val mrzSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_MRZ)
          val oppositeSideOriginalImage = result.getOriginalImage(EnumDocumentSide.DS_OPPOSITE)
          val tabImages = findViewById<TabLayout>(R.id.tab_images)
          val vpImages = findViewById<ViewPager2>(R.id.vp_images)
          if (mrzSideDocumentImage == null && mrzSideOriginalImage == null) {
             tabImages.visibility = View.GONE
             vpImages.visibility = View.GONE
             return
          } else {
             tabImages.visibility = View.VISIBLE
             vpImages.visibility = View.VISIBLE
          }
          vpImages.adapter = object : FragmentStateAdapter(this) {
             override fun createFragment(position: Int): Fragment {
                return if (position == 0 && mrzSideDocumentImage != null) {
                   ImagesFragment.newInstance(mrzSideDocumentImage, oppositeSideDocumentImage)
                } else {
                   ImagesFragment.newInstance(mrzSideOriginalImage, oppositeSideOriginalImage)
                }
             }
             override fun getItemCount(): Int {
                return if (mrzSideDocumentImage != null && mrzSideOriginalImage != null) 2 else 1
             }
          }
          if (mrzSideOriginalImage != null || oppositeSideOriginalImage != null) {
             TabLayoutMediator(tabImages, vpImages) { tab, position ->
                tab.text = if (position == 0 && mrzSideDocumentImage != null) "Processed" else "Original"
             }.attach()
          }
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

- **Samples** — Explore the complete [ScanMRZ sample on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/tree/main/android/samples/ScanMRZ).
- **Customize** — Learn how to configure document type, UI elements, and feedback in the [Customize MRZ Scanner](customize-mrz-scanner.md) guide.
- **API Reference** — Browse the full [Android API Reference](../api-reference/index.md) for all classes and methods.
- **License** — See the [License Activation](license-activation.md) guide for production license setup.
- **Support** — Contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/) for help or custom requirements.
