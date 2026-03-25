---
layout: default-layout
title: MRZScannerActivity Class - Dynamsoft MRZ Scanner Android Edition
description: MRZScannerActivity of DynamsoftMRZScanner Android is an activity class that implements MRZ scanning features.
keywords: MRZ, scanner, activity
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScannerActivity
---

# Class MRZScannerActivity

`MRZScannerActivity` is an activity class that implements MRZ scanning features.

## Definition

*Assembly:* DynamsoftMRZScanner.aar

*Namespace:* com.dynamsoft.mrzscanner.ui

```java
class MRZScannerActivity extends AppCompatActivity
```

## ResultContract

A contract specifying that the activity `MRZScannerActivity` can be called with an input of type `MRZScannerConfig` and produce an output of type `MRZScanResult`.

```java
public static final class ResultContract extends ActivityResultContract<MRZScannerConfig, MRZScanResult>
```

## How to Use

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
          // ...
          // Set up the license via the MRZScannerConfig
          MRZScannerConfig config = new MRZScannerConfig();
          config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
          // Set up a launcher and register the result callback for MRZScannerActivity.
          launcher = registerForActivityResult(new MRZScannerActivity.ResultContract(), result -> {
             if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_FINISHED && result.getData() != null) {
                // user code after a successful scan
             } else if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_CANCELED) {
                // user code after a canceled scan
             }
             if (result.getErrorString() != null && !result.getErrorString().isEmpty()) {
                // user code after a failed scan
             }
          });
          // Config a button on the UI. Launch the MRZScannerActivity when button clicked.
          findViewById(R.id.btn_navigate).setOnClickListener(v -> launcher.launch(config));
   }
}
```
2. 
```kotlin
class MainActivity : AppCompatActivity() {
   private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
   override fun onCreate(savedInstanceState: Bundle?) {
          // Set up the license via the MRZScannerConfig
          val config = MRZScannerConfig().apply {
             license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9";
          }
          // Set up a launcher and register the result callback for MRZScannerActivity.
          launcher = registerForActivityResult(MRZScannerActivity.ResultContract()) { result ->
             if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_FINISHED && result.data != null) {
                    // user code after a successful scan
             } else if (result.resultStatus == MRZScanResult.EnumResultStatus.RS_CANCELED) {
                    // user code after a canceled scan
             }
             if (result.errorString != null && result.errorString.isNotEmpty()) {
                    // user code after a failed scan
             }
          }
          // Config a button on the UI. Launch the MRZScannerActivity when button clicked.
          findViewById<View>(R.id.btn_navigate).setOnClickListener {
             launcher.launch(
                    config
             )
          }
   }
}
```
