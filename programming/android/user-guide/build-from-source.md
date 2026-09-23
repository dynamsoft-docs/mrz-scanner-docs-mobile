---
layout: default-layout
title: Building from Source - Dynamsoft MRZ Scanner Android Edition
description: Build the Dynamsoft MRZ Scanner component from its published source code and use your own build in an Android app.
keywords: build from source, open source, customize, Android, AAR, Gradle
breadcrumbText: Build from Source
noTitleIndex: true
needGenerateH3Content: true
needAutoGenerateSidebar: true
---

# Building the MRZ Scanner from Source

[`MRZScannerConfig`](customize-mrz-scanner.md) covers most of what an integration needs to change. When it does not go far enough, the source of the `MRZScannerBundle` component is published on GitHub, and you can build your own copy of the library.

## When to Build from Source

Build from source when you need behavior the configuration API cannot express — restructuring the scanner screen, replacing its controls, or changing how results are assembled before they reach your app.

Stay on the published library when `MRZScannerConfig` can do the job. Building from source means you own the merge at every SDK release: your changes have to be re-applied to each new version, and you no longer pick up fixes by changing a version number.

> [!NOTE]
> If you are unsure whether you need a source build, or get stuck along the way, contact the [Dynamsoft Support Team](https://www.dynamsoft.com/contact/).

## What the Source Includes

The published source is the `MRZScannerBundle` component — the scanner activity, its configuration and result types, and the whole scanner UI. The MRZ detection engine itself is not part of it: `MRZScannerBundle` depends on Dynamsoft Capture Vision, which stays a binary dependency resolved from Maven.

The component's native layer ships as prebuilt `.so` libraries for `arm64-v8a`, `armeabi-v7a`, `x86`, and `x86_64`, already committed under `dynamsoftmrzscannerbundle/src/main/jniLibs`. You do not compile any native code, and you do not need the Android NDK.

## Prerequisites

- **Android Studio** — any version that supports Android Gradle Plugin 8.9.
- **JDK 17 or newer.** The library targets Java 11 bytecode, but the build itself needs a modern JDK.
- **Android SDK Platform 36**, matching the library's `compileSdk`.
- **Network access to `download2.dynamsoft.com`**, where the Capture Vision dependency is hosted.

Gradle itself does not need to be installed — the project ships a wrapper that fetches the version it needs.

## Get the Source

Clone the repository:

```bash
git clone https://github.com/Dynamsoft/mrz-scanner-mobile.git
```

The library project is at `android/src/DynamsoftMRZScannerBundle`. Two modules sit inside it:

| Module | Contents |
| ------ | -------- |
| `dynamsoftmrzscannerbundle` | The library itself — Java sources, resources, and the prebuilt native libraries. This is what you build. |
| `mrzbundlejni` | The JNI layer that produced those prebuilt libraries. |

## Prepare the Project

Open `android/src/DynamsoftMRZScannerBundle/settings.gradle` and delete the `mrzbundlejni` line (line 24 in the original file):

```groovy
rootProject.name = "DynamsoftMRZScannerBundle"
include ':dynamsoftmrzscannerbundle'
```

> [!IMPORTANT]
> This step is required, not optional. Gradle configures every module listed in `settings.gradle`, and `mrzbundlejni` pins a specific NDK version — so leaving the line in place fails the build with `NDK not configured` before the library is even compiled.
>
> `mrzbundlejni` also cannot be built from the published source: it links against native Dynamsoft libraries that are not part of the repository, and compiling it fails at the link step with undefined symbols. Its output is not needed. The `.so` files it would produce are already committed in `dynamsoftmrzscannerbundle/src/main/jniLibs`, so removing the line leaves the library complete and takes the NDK requirement with it.

## Build the Library

From `android/src/DynamsoftMRZScannerBundle`:

```bash
./gradlew :dynamsoftmrzscannerbundle:assembleRelease
```

The AAR lands in `dynamsoftmrzscannerbundle/build/outputs/aar/`:

```
MRZScannerBundle-3.6.2000-release.aar
```

The version in that filename comes from `releaseVersion` in `dynamsoftmrzscannerbundle/build.gradle`. Change it there if you want your builds to carry their own version.

## Use Your Build in an App

Publishing to your local Maven repository is the least invasive way to consume the result: your build keeps the same coordinates as the released library, so nothing in the consuming app's dependency list changes.

1. Create `dynamsoftmrzscannerbundle/local_publish.gradle`:

   ```groovy
   apply plugin: 'maven-publish'
   afterEvaluate {
       publishing {
           publications {
               release(MavenPublication) {
                   from components.release
                   groupId = 'com.dynamsoft'
                   artifactId = 'mrzscannerbundle'
                   version = project.ext.releaseVersion
               }
           }
       }
   }
   ```

   The library's `build.gradle` applies this file when it exists and ignores it when it does not, so no other change is needed.

2. Publish it:

   ```bash
   ./gradlew :dynamsoftmrzscannerbundle:publishToMavenLocal
   ```

   This writes the AAR, its POM, and a sources JAR to `~/.m2/repository/com/dynamsoft/mrzscannerbundle/`. The sources JAR lets Android Studio step into the component's code while debugging.

3. In the consuming app, add `mavenLocal()` ahead of the Dynamsoft repository so your build is found first:

   <div class="sample-code-prefix"></div>
   >- Groovy
   >- Kotlin
   >
   >1. 
   ```groovy
   repositories {
           mavenLocal()
           maven { url "https://download2.dynamsoft.com/maven/aar" }
           google()
           mavenCentral()
   }
   ```
   2. 
   ```kotlin
   repositories {
           mavenLocal()
           maven { url = uri("https://download2.dynamsoft.com/maven/aar") }
           google()
           mavenCentral()
   }
   ```

The app's existing dependency is unchanged — `com.dynamsoft:mrzscannerbundle:3.6.2000` now resolves to your build. The Capture Vision dependency comes through automatically, so there is nothing else to declare.

> [!WARNING]
> `mavenLocal()` applies to every dependency in the project, and it stays in effect until you remove it. Take it out when you are finished so the app goes back to the released library.

## Confirm You Are Running Your Build

An unmodified build of the published source produces the same compiled output as the released library, so a successful build is not by itself evidence that your copy is the one being used.

To confirm the wiring before you rely on it, make a change you can see — a string in the scanner UI is enough — then rebuild, republish, and run the app.

## Keeping Up with SDK Releases

Your changes live in your own copy of the source, so each new SDK release is a merge:

1. Pull the new version of the repository.
2. Re-apply your changes to the updated source.
3. Rebuild and republish.

Check `releaseVersion` and the Capture Vision version in `dynamsoftmrzscannerbundle/build.gradle` after each pull — both move with the release, and the app's dependency has to match whatever you publish.
