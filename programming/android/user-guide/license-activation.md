---
layout: default-layout
title: License Activation - Dynamsoft MRZ Scanner Android Edition
description: Initialize the license of Dynamsoft MRZ Scanner Android edition.
keywords: license initialization, licensing
needAutoGenerateSidebar: true
---

# License Initialization

## Get a trial license

You can request a 30-day trial license via the [Request a Trial License](https://www.dynamsoft.com/customer/license/trialLicense?product=mrz&package=mobile&utm_source=docs){:target="_blank"} link.

## Get a Full License

<a href="https://www.dynamsoft.com/company/contact" target="_blank">Contact us</a> to purchase a full license.

## Set the License In the Code

<div class="sample-code-prefix"></div>
>- Java
>- Kotlin
>
>1. 
```java
MRZScannerConfig config = new MRZScannerConfig();
config.setLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");
```
2. 
```kotlin
val config = MRZScannerConfig().apply {
   license = "DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9"
}
```
