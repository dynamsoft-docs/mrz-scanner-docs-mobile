---
layout: default-layout
title: Configure MRZ Scanner - Dynamsoft MRZ Scanner Android Edition
description: Configure the MRZ scan settings via MRZScannerConfig class when using MRZ Scanner Android Edition
keywords: Configure, config, MRZScannerConfig, Android
breadcrumbText: Configure MRZ Scanner
noTitleIndex: true
needGenerateH3Content: true
needAutoGenerateSidebar: true
---

# Configure MRZ Scanner

When developing with MRZScannerActivity, you can add configurations via the `MRZScannerConfig` class. This page will guide you on how to configure the settings.

## Scan Document Type

### Set the document type via APIs

Specifies the type of document to scan, such as ID cards or passports. It also improves the processing speed and the accuracy.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
MRZScannerConfig config = new MRZScannerConfig();
config.setDocumentType(EnumDocumentType.DT_PASSPORT);
```
2. 
```kotlin
val config = MRZScannerConfig().apply {
   documentType = EnumDocumentType.DT_PASSPORT
}
```

### Setup a customized template file

A template file is a JSON file that includes a series of algorithm parameter settings. It is always used to customize the performance for different usage scenarios. [Contact us](https://www.dynamsoft.com/company/customer-service/#contact) to get a customized template for your scanner.

1. Add a **Templates** folder to the assets folder of your project at **src\main\assets\Templates**. Put your JSON file in the **Templates** folder.

2. Specify the template file via setTemplateFilePath

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
MRZScannerConfig config = new MRZScannerConfig();
config.setTemplateFilePath("CustomizedTemplate.json");
```
2. 
```kotlin
val config = MRZScannerConfig().apply {
   templateFilePath = "CustomizedTemplate.json"
}
```

**Related APIs**

- [`setDocumentType`]({{ site.android_api }}mrz-scanner-config.html#setdocumenttype)
- [`setTemplateFilePath`]({{ site.android_api }}mrz-scanner-config.html#settemplatefilepath)

## Configure the UI Elements

<div align="center">
    <p><img src="../../assets/mrz-scanner-ui.png" width="70%" alt="mrz-scanner"></p>
    <p>MRZ Scanner UI Component</p>
</div>

- Close button: Stop MRZ scanning and go back to the previous activity.
- Torch button: A clickable button that can turn on/off the torch.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
MRZScannerConfig config = new MRZScannerConfig();
config.setTorchButtonVisible(false);
config.setGuideFrameVisible(false);
config.setCloseButtonVisible(false);
```
2. 
```kotlin
val config = MRZScannerConfig().apply {
   torchButtonVisible = false
   guideFrameVisible = false
   closeButtonVisible = false
}
```

**Related APIs**

- [`setTorchButtonVisible`]({{ site.android_api }}mrz-scanner-config.html#settorchbuttonvisible)
- [`setCloseButtonVisible`]({{ site.android_api }}mrz-scanner-config.html#setclosebuttonvisible)

### Beep

Let the app to trigger a beep sound when MRZ is scanned successfully.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
MRZScannerConfig config = new MRZScannerConfig();
config.setBeepEnabled(true);
```
2. 
```kotlin
val config = MRZScannerConfig().apply {
   isBeepEnabled = true
}
```

**Related API**

- [`setBeepEnabled`]({{ site.android_api }}mrz-scanner-config.html#setbeepenabled)

## Further Customization

If you have other customization requirements on the `MRZScanner` component, you can modify it with the [open source code on GitHub](https://github.com/Dynamsoft/mrz-scanner-mobile/).
