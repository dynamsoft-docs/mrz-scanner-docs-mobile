---
layout: default-layout
title: How to update - Dynamsoft MRZ Scanner for Android
description: Follow the upgrade instructions to learn to upgrade MRZ Scanner SDK Android edition from 2 to 3.
keywords: updates guide, android
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
---

# How to Upgrade

## Update the Libraries

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
