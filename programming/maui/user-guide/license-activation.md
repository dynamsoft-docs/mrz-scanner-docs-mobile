---
layout: default-layout
title: License Initialization - Dynamsoft MRZ Scanner MAUI Edition
description: Initialize the license of Dynamsoft MRZ Scanner MAUI edition.
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

```csharp
var config = new MRZScannerConfig("YOUR-LICENSE-KEY");
```
