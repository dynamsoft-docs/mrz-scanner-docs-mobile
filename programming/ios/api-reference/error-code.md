---
layout: default-layout
title: DSMRZErrorCode - Dynamsoft MRZ Scanner iOS Edition
description: DSMRZErrorCode of DynamsoftMRZScanner iOS is an enumeration that defines the error codes owned by the MRZ Scanner bundle.
keywords: MRZScanner, MRZScanResult, error code, camera permission
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: DSMRZErrorCode
---

# DSMRZErrorCode

`DSMRZErrorCode` is an enumeration that defines the error codes owned by the MRZ Scanner bundle. These are reported through [`errorCode`](mrz-scan-result.md#errorcode) alongside a result status of `.exception`.

## Definition

*Assembly:* DynamsoftMRZScannerBundle.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
typedef NS_ENUM(NSInteger, DSMRZErrorCode)
{
        DSMRZErrorCodeCameraPermissionDenied = 1001,
        DSMRZErrorCodeCameraPermissionRestricted = 1002
};
```
2. 
```swift
@objc public enum ErrorCode: Int {
        case cameraPermissionDenied = 1001
        case cameraPermissionRestricted = 1002
}
```

> [!NOTE]
> The Objective-C name keeps the `MRZ` infix, unlike the other enumerations in this bundle, because a bare `DSErrorCode` is a plausible future Capture Vision symbol and Objective-C has no namespaces. The Swift name is simply `ErrorCode`.

## Members

| Member | Value | Description |
| ------ | ----- | ----------- |
| `DSMRZErrorCodeCameraPermissionDenied` / `cameraPermissionDenied` | 1001 | The user denied camera access. They can still grant it through the app's page in Settings. |
| `DSMRZErrorCodeCameraPermissionRestricted` / `cameraPermissionRestricted` | 1002 | Camera access is withheld by device policy — Screen Time restrictions, parental controls, or an MDM profile — and the user cannot grant it themselves. |

Each code arrives with a matching [`errorString`](mrz-scan-result.md#errorstring):

| Code | `errorString` |
| ---- | ------------- |
| 1001 | Camera access is denied. Enable camera access for this app in Settings to scan. |
| 1002 | Camera access is restricted on this device and cannot be granted by the user. |

## Remarks

**This is not an exhaustive list of the values `errorCode` can return.** Failures originating in Dynamsoft Capture Vision surface their own error codes through the same property, and those are all less than or equal to `0`. The MRZ Scanner bundle claims the positive range **1000 to 1999**, so the two code spaces can never collide and the sign alone tells you which one you are looking at.

**The two codes differ by remediation, not by severity.** `cameraPermissionDenied` describes a state the user can resolve, so offering a route into the app's settings page is useful. `cameraPermissionRestricted` describes a state they cannot: the per-app camera toggle is absent from Settings when device policy withholds the camera, so sending the user there is a dead end. Explain the restriction instead.

**You do not need to handle these codes to get a working flow.** By default the scanner presents its own alert before reporting, and it never starts the camera without access. Set [`isCameraPermissionPromptEnabled`](mrz-scanner-config.md#iscamerapermissionpromptenabled) to `false` only if you intend to present your own permission UI; the denial is still reported either way.

> [!NOTE]
> These values match the Android bundle's `EnumErrorCode` exactly, so the Flutter, React Native and MAUI wrappers need no platform branching.

## Recovering from a denial

iOS presents its camera permission alert **once per install**, so a denial cannot be re-requested from inside the app. That shapes the whole recovery path:

1. The user denies camera access. `MRZScannerViewController` does not start the camera.
2. The scanner presents a **Camera Access Needed** alert offering **Open Settings** and **Cancel**.
3. **Open Settings** deep-links to the app's own page in Settings, where the Camera toggle lives.
4. Changing a privacy setting there **terminates the app**. This is iOS behavior and cannot be avoided.
5. On relaunch, opening the scanner re-reads the authorization status. If access was granted the camera starts normally; if it was left denied, the same alert appears again.

Because the app is terminated at step 4, a screen that displays the denial should keep its own route into Settings — the scanner's alert is long gone by the time the user returns. The [ScanMRZ sample](../samples/scanmrz-walkthrough.md) does this with an **Open Settings** button that appears on the home screen only when `errorCode` is `cameraPermissionDenied`.

For `cameraPermissionRestricted` the alert carries no **Open Settings** action, since the toggle the user would need is not there.

> [!NOTE]
> This differs from Android, which recovers in place: granting the permission in Settings does not kill the Android process, so an activity showing a denial can re-check in `onResume` and start a new scan without a relaunch.
