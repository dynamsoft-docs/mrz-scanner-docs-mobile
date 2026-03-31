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
