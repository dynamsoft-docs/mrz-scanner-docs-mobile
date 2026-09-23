---
layout: default-layout
title: How to update - Dynamsoft MRZ Scanner for Android
description: Follow the upgrade instructions to learn to upgrade MRZ Scanner SDK Android edition.
keywords: updates guide, android
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
multiProgrammingLanguage: true
enableLanguageSelection: true
---

# How to Upgrade

## From v3.4.x to v3.6.x

### Update the Libraries

1. Open the file `[App Project Root Path]\app\build.gradle` and update the dependency version:

   <div class="sample-code-prefix"></div>
   >- groovy
   >- kts
   >
   >1. 
   ```groovy
   dependencies {
      implementation 'com.dynamsoft:mrzscannerbundle:{version-number}'
   }
   ```
   2. 
   ```kotlin
   dependencies {
      implementation("com.dynamsoft:mrzscannerbundle:{version-number}")
   }
   ```

   > [!NOTE] Please view [user guide](index.md#add-the-sdk) for the correct version number.

2. Click **Sync Now**. After the synchronization is complete, the updated SDK is added to the project.

### Handle Behavior Changes

No public API was removed or renamed in 3.6.x, so an existing integration keeps compiling. Three changes to how the scanner *behaves* can still affect it.

#### Results That Fail Check-Digit Validation Are Now Delivered

Through 3.4.x, the scanner discarded any result whose MRZ lines failed check-digit validation and simply carried on scanning. A damaged, misread, or altered document produced **no result at all** — from the app's point of view the scan never finished.

3.6.x delivers the result and reports the failure per field instead:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
int status = data.getFieldValidationStatus("documentNumber");
if (status == EnumValidationStatus.VS_FAILED) {
       // The value is present but disagrees with its check digit.
}
```
2. 
```kotlin
val status = data.getFieldValidationStatus("documentNumber")
if (status == EnumValidationStatus.VS_FAILED) {
       // The value is present but disagrees with its check digit.
}
```

Any code that assumed every delivered result was check-digit-clean now has to make that check explicitly. See [Reading a field's validation status](index.md#step-5-launch-the-scanner-and-show-the-result) in the user guide, and [`getFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus) for the accepted field names.

#### Camera Access Is Now Gated and Reported

Through 3.4.x, `MRZScannerActivity` opened the camera regardless of whether the `CAMERA` permission was held. Without it the preview stayed blank and nothing was reported — the symptom customers described as being stuck on a loading screen.

3.6.x requests the permission on first launch, never opens the camera without it, and reports a denial as `RS_EXCEPTION` carrying [`EC_CAMERA_PERMISSION_DENIED`](../api-reference/error-code.md) (1001) or [`EC_CAMERA_PERMISSION_RESTRICTED`](../api-reference/error-code.md) (1002).

Two things to check in existing code:

- **Handle `RS_EXCEPTION`.** A branch that ignored it will now silently swallow a permission denial that the SDK is reporting properly.
- **If your app already requests the camera permission itself or presents its own rationale UI**, suppress the scanner's dialog so the user does not see two:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
config.setCameraPermissionPromptEnabled(false);
```
2. 
```kotlin
config.isCameraPermissionPromptEnabled = false
```

The denial is still reported either way. See [Handling Camera Permission](customize-mrz-scanner.md#handling-camera-permission) for the full flow.

> [!NOTE]
> Choosing **Open Settings** in the scanner's dialog does not finish the activity. Granting the permission in Settings does not kill the Android process either, so the scanner starts the camera in place when the user returns — no result is reported and no restart is needed. This differs from iOS, where changing the setting terminates the app.

#### The Scan Region Is Now the Guide Frame

Through 3.4.x the whole camera preview was analyzed, so a document held outside the guide frame could still be read. 3.6.x limits capture to the area inside the frame, which is what the frame appeared to promise all along.

If you relied on the wider area — or your users are used to aiming loosely — hiding the guide frame lifts the restriction back to the whole preview. Note that it also hides the prompt text; see [Hiding the guide frame](customize-mrz-scanner.md#hiding-the-guide-frame).

### Adopt the New APIs

These are additive, so adopting them is optional:

- [`getFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus) — per-field check-digit status.
- [`EnumErrorCode`](../api-reference/error-code.md) — the bundle's own error codes, currently both about camera access.
- [`setCameraPermissionPromptEnabled`](../api-reference/mrz-scanner-config.md#setcamerapermissionpromptenabled) — suppress the built-in permission dialog.

Your users will also notice two additions to the scanner UI that need no code from you: a progress spinner while MRZ-like text is being processed, and a flip prompt for TD1 and TD2 ID cards whose portrait is on the opposite side. Both are described in [The Scanner Screen](index.md#the-scanner-screen).

## From v3.2.x to v3.4.x

### Update the Libraries

1. Open the file `[App Project Root Path]\settings.gradle` and add the Maven repository:

   <div class="sample-code-prefix"></div>
   >- groovy
   >- kts
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

   > [!NOTE] If you are using gradle 6.x or older version, the maven dependencies should be configured in  `[App Project Root Path]\app\build.gradle`

2. Open the file `[App Project Root Path]\app\build.gradle` and add the dependencies:

   <div class="sample-code-prefix"></div>
   >- groovy
   >- kts
   >
   >1. 
   ```groovy
   dependencies {
      implementation 'com.dynamsoft:mrzscannerbundle:{version-number}'
   }
   ```
   2. 
   ```kotlin
   dependencies {
      implementation("com.dynamsoft:mrzscannerbundle:{version-number}")
   }
   ```

   > [!NOTE] Please view [user guide](index.md#add-the-sdk) for the correct version number.

3. Click **Sync Now**. After the synchronization is complete, the SDK is added to the project.

### Adopt the New Image Capture APIs

v3.4.x adds the ability to retrieve captured images alongside the parsed MRZ data. Three types of images are available via [`MRZScanResult`](../api-reference/mrz-scan-result.md):

- **Document image** — a cropped, perspective-corrected image of the document. Enabled by default.
- **Portrait image** — the portrait extracted from the document. Enabled by default.
- **Original image** — the raw full-frame camera capture. Disabled by default.

Control which images are returned using the new [`MRZScannerConfig`](../api-reference/mrz-scanner-config.md) methods:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
MRZScannerConfig config = new MRZScannerConfig();
config.setReturnDocumentImage(true);  // default: true
config.setReturnPortraitImage(true);  // default: true
config.setReturnOriginalImage(false); // default: false — opt in to enable
```
2. 
```kotlin
val config = MRZScannerConfig()
config.setReturnDocumentImage(true)   // default: true
config.setReturnPortraitImage(true)   // default: true
config.setReturnOriginalImage(false)  // default: false — opt in to enable
```

Retrieve the images from the scan result:

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
ImageData portrait = result.getPortraitImage();
ImageData docImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ);
ImageData original = result.getOriginalImage(EnumDocumentSide.DS_MRZ);
// For two-sided ID cards, also retrieve the opposite side:
ImageData opposite = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE);
```
2. 
```kotlin
val portrait = result.getPortraitImage()
val docImage = result.getDocumentImage(EnumDocumentSide.DS_MRZ)
val original = result.getOriginalImage(EnumDocumentSide.DS_MRZ)
// For two-sided ID cards, also retrieve the opposite side:
val opposite = result.getDocumentImage(EnumDocumentSide.DS_OPPOSITE)
```

> [!NOTE] All three methods return `null` if the corresponding return flag is disabled or the image was not captured. `getDocumentImage(DS_OPPOSITE)` and `getOriginalImage(DS_OPPOSITE)` also return `null` for single-sided documents such as passports.

## From v2 to v3

### Update the Libraries

1. Open the file `[App Project Root Path]\settings.gradle` and add the Maven repository:

   <div class="sample-code-prefix"></div>
   >- groovy
   >- kts
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

   > [!NOTE] If you are using gradle 6.x or older version, the maven dependencies should be configured in  `[App Project Root Path]\app\build.gradle`

2. Open the file `[App Project Root Path]\app\build.gradle` and add the dependencies:

   <div class="sample-code-prefix"></div>
   >- groovy
   >- kts
   >
   >1. 
   ```groovy
   dependencies {
      implementation 'com.dynamsoft:mrzscannerbundle:{version-number}'
   }
   ```
   2. 
   ```kotlin
   dependencies {
      implementation("com.dynamsoft:mrzscannerbundle:{version-number}")
   }
   ```

   > [!NOTE] Please view [user guide](index.md#add-the-sdk) for the correct version number.

3. Click **Sync Now**. After the synchronization is complete, the SDK is added to the project.
