---
layout: default-layout
title: EnumErrorCode - Dynamsoft MRZ Scanner Android Edition
description: EnumErrorCode of DynamsoftMRZScanner Android is an enumeration class that defines the error codes owned by the MRZ Scanner bundle.
keywords: MRZScanner, MRZScanResult, error code, camera permission
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumErrorCode
---

# EnumErrorCode

`EnumErrorCode` is an enumeration class that defines the error codes owned by the MRZ Scanner bundle. These are reported through [`getErrorCode`](mrz-scan-result.md#geterrorcode) alongside a result status of `RS_EXCEPTION`.

## Definition

*Assembly:* MRZScannerBundle.aar

*Namespace:* com.dynamsoft.mrzscannerbundle.ui

```java
@IntDef(value = {EC_CAMERA_PERMISSION_DENIED, EC_CAMERA_PERMISSION_RESTRICTED})
public @interface EnumErrorCode {
    int EC_CAMERA_PERMISSION_DENIED = 1001;
    int EC_CAMERA_PERMISSION_RESTRICTED = 1002;
}
```

## Members

| Member | Value | Description |
| ------ | ----- | ----------- |
| `EC_CAMERA_PERMISSION_DENIED` | 1001 | The user denied camera access but can still grant it, either by allowing a fresh in-app request or through the app's page in Settings once the denial has become permanent. |
| `EC_CAMERA_PERMISSION_RESTRICTED` | 1002 | Camera access is withheld by device policy and the user cannot grant it at all. |

## Remarks

**This is not an exhaustive list of the values `getErrorCode` can return.** Failures originating in Dynamsoft Capture Vision surface their own error codes through the same field, and those are all less than or equal to `0`. The MRZ Scanner bundle claims the positive range **1000 to 1999**, so the two code spaces can never collide and the sign alone tells you which one you are looking at.

**The two codes differ by remediation, not by severity.** `EC_CAMERA_PERMISSION_DENIED` describes a state the user can resolve, so offering a route into the app's settings page is useful. `EC_CAMERA_PERMISSION_RESTRICTED` describes a state they cannot: the per-app camera toggle is absent from Settings when device policy withholds the camera, so sending the user there is a dead end. Explain the restriction instead.

**You do not need to handle these codes to get a working flow.** By default the scanner presents its own dialog before reporting, and it never starts the camera without access. Set [`setCameraPermissionPromptEnabled`](mrz-scanner-config.md#setcamerapermissionpromptenabled) to `false` only if you intend to present your own permission UI; the denial is still reported either way.

> [!NOTE]
> These values match the iOS bundle's `DSMRZErrorCode` exactly, so the Flutter, React Native and MAUI wrappers need no platform branching.

## Recovering from a denial

Granting the permission in Settings does not kill the Android process, so an activity that is showing a denial can re-check the permission in `onResume` and start a new scan in place. This differs from iOS, where changing the permission in Settings terminates the app and it recovers by relaunching.

See the [ScanMRZ Sample Walkthrough](../samples/scanmrz-walkthrough.md#resultactivity) for a worked example.
