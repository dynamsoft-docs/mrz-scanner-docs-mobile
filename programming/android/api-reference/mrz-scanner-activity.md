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
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          // Configure the MRZ scanner.
          MRZScannerConfig config = new MRZScannerConfig();
          config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
          // Set the document type to scan (default: DT_ALL).
          config.setDocumentType(EnumDocumentType.DT_ALL);
          // Configure which images to include in the result.
          config.setReturnDocumentImage(true);   // Cropped document image (default: true).
          config.setReturnPortraitImage(true);   // Portrait image (default: true).
          config.setReturnOriginalImage(false);  // Original full-frame image (default: false).
          // Configure UI element visibility.
          config.setCloseButtonVisible(true);
          config.setTorchButtonVisible(true);
          config.setCameraToggleButtonVisible(true);
          config.setBeepButtonVisible(true);
          config.setVibrateButtonVisible(true);
          config.setFormatSelectorVisible(true);
          // Configure feedback on successful scan.
          config.setBeepEnabled(true);
          config.setVibrateEnabled(false);
          // Register the activity result callback.
          launcher = registerForActivityResult(new MRZScannerActivity.ResultContract(), result -> {
             if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_FINISHED) {
                // Scan completed successfully.
                MRZData data = result.getData();
                String mrzText = data.getMrzText();
                String firstName = data.getFirstName();
                String lastName = data.getLastName();
                // ... access other MRZData fields as needed.
                // Access the cropped document images.
                // These may be null — see MRZScanResult.getDocumentImage for details.
                ImageData mrzSideImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ);
                ImageData oppositeSideImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE);
                // Access the portrait image. Null if not detected or if setReturnPortraitImage(false).
                ImageData portraitImage = result.getPortraitImage();
                // If you need to pass the result to another activity via Intent,
                // call result.retainAllImageInstances() before startActivity()
                // to prevent the native image instances from being recycled.
             } else if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_CANCELED) {
                // The user closed the scanner before completing a scan.
             } else if (result.getResultStatus() == MRZScanResult.EnumResultStatus.RS_EXCEPTION) {
                // An error occurred during initialization or scanning.
                int errorCode = result.getErrorCode();
                String errorMessage = result.getErrorString();
             }
          });
          // Launch the MRZScannerActivity when the button is clicked.
          findViewById(R.id.btn_start).setOnClickListener(v -> launcher.launch(config));
   }
}
```
2. 
```kotlin
class MainActivity : AppCompatActivity() {
   private lateinit var launcher: ActivityResultLauncher<MRZScannerConfig>
   override fun onCreate(savedInstanceState: Bundle?) {
          super.onCreate(savedInstanceState)
          setContentView(R.layout.activity_main)
          // Configure the MRZ scanner.
          val config = MRZScannerConfig().apply {
             setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9")
             // Set the document type to scan (default: DT_ALL).
             setDocumentType(EnumDocumentType.DT_ALL)
             // Configure which images to include in the result.
             setReturnDocumentImage(true)   // Cropped document image (default: true).
             setReturnPortraitImage(true)   // Portrait image (default: true).
             setReturnOriginalImage(false)  // Original full-frame image (default: false).
             // Configure UI element visibility.
             setCloseButtonVisible(true)
             setTorchButtonVisible(true)
             setCameraToggleButtonVisible(true)
             setBeepButtonVisible(true)
             setVibrateButtonVisible(true)
             setFormatSelectorVisible(true)
             // Configure feedback on successful scan.
             setBeepEnabled(true)
             setVibrateEnabled(false)
          }
          // Register the activity result callback.
          launcher = registerForActivityResult(MRZScannerActivity.ResultContract()) { result ->
             when (result.getResultStatus()) {
                MRZScanResult.EnumResultStatus.RS_FINISHED -> {
                   // Scan completed successfully.
                   val data = result.getData()
                   val mrzText = data.getMrzText()
                   val firstName = data.getFirstName()
                   val lastName = data.getLastName()
                   // ... access other MRZData fields as needed.
                   // Access the cropped document images.
                   // These may be null — see MRZScanResult.getDocumentImage for details.
                   val mrzSideImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ)
                   val oppositeSideImage = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE)
                   // Access the portrait image. Null if not detected or if setReturnPortraitImage(false).
                   val portraitImage = result.getPortraitImage()
                   // If you need to pass the result to another activity via Intent,
                   // call result.retainAllImageInstances() before startActivity()
                   // to prevent the native image instances from being recycled.
                }
                MRZScanResult.EnumResultStatus.RS_CANCELED -> {
                   // The user closed the scanner before completing a scan.
                }
                else -> {
                   // RS_EXCEPTION: an error occurred during initialization or scanning.
                   val errorCode = result.getErrorCode()
                   val errorMessage = result.getErrorString()
                }
             }
          }
          // Launch the MRZScannerActivity when the button is clicked.
          findViewById<View>(R.id.btn_start).setOnClickListener {
             launcher.launch(config)
          }
   }
}
```
