---
layout: default-layout
title: License Initialization - Dynamsoft MRZ Scanner Android Edition
description: Initialize the license of Dynamsoft MRZ Scanner Android edition.
keywords: license initialization, licensing
needAutoGenerateSidebar: true
---

# License Initialization

A license key is required to use the MRZ Scanner. Follow the steps below to obtain and configure one.

## Get a Trial License

You can request a 30-day trial license via the [Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&package=mobile&utm_source=docs){:target="_blank"} portal.

## Get a Full License

<a href="https://www.dynamsoft.com/company/contact" target="_blank">Contact us</a> to purchase a full license.

## Set the License in the Code

Set the license key on `MRZScannerConfig` before launching the scanner.

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
MRZScannerConfig config = new MRZScannerConfig();
config.setLicense("YOUR-LICENSE-KEY");
```
2. 
```kotlin
val config = MRZScannerConfig().apply {
   license = "YOUR-LICENSE-KEY"
}
```
