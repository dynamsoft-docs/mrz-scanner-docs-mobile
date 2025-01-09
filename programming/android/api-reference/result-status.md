---
layout: default-layout
title: EnumResultStatus - Dynamsoft MRZ Scanner Android Edition
description: EnumResultStatus of DynamsoftMRZScanner Android is an enumeration class that defines the result status of the MRZScanResult.
keywords: MRZScanner, MRZScanResult
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumResultStatus
---

# EnumResultStatus

`EnumResultStatus` is a enumeration class that defines the result status of the `MRZScanResult`.

## Definition

*Assembly:* DynamsoftMRZScanner.aar

*Namespace:* com.dynamsoft.mrzscanner.ui

```java
@IntDef(value = {RS_FINISHED, RS_CANCELED, RS_EXCEPTION})
public @interface EnumResultStatus {
    int RS_FINISHED = 0;
    int RS_CANCELED = 1;
    int RS_EXCEPTION = 2;
}
```
