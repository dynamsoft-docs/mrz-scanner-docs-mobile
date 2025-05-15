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

There are 2 ways in which you can include the `DynamsoftMRZScannerBundle` library in your app:

### Option 1: Add the xcframeworks via Swift Package Manager

1. In your Xcode project, go to **File --> AddPackages**.

2. In the top-right section of the window, search "https://github.com/Dynamsoft/barcode-reader-spm"

3. Select `barcode-reader-spm`, choose `Up to Next Major Version`, then click **Add Package**.

4. Check all the **xcframeworks** and add.

### Option 2: Add the Frameworks via CocoaPods

1. Add the frameworks in your **Podfile**, replace `TargetName` with your real target name.

   ```sh
   target 'ScanMRZ' do
      use_frameworks!

   pod 'DynamsoftMRZScannerBundle','{version-number}'

   end
   ```

   > Note: Please view [user guide](user-guide.md#option-1-add-the-library-via-maven) for the correct version number.


2. Execute the pod command to install the frameworks and generate workspace(**[TargetName].xcworkspace**):

   ```sh
   pod install
   ```
