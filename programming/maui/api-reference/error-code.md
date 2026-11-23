---
layout: default-layout
title: EnumErrorCode - Dynamsoft MRZ Scanner MAUI Edition
description: EnumErrorCode of DynamsoftMRZScanner MAUI is an enumeration that defines the error codes owned by the MRZ Scanner bundle.
keywords: MRZScanner, MRZScanResult, error code, camera permission
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: EnumErrorCode
---

# EnumErrorCode

`EnumErrorCode` is an enumeration that defines the error codes owned by the MRZ Scanner bundle. These are reported through [`ErrorCode`](mrz-scan-result.md#errorcode) alongside a result status of `EnumResultStatus.Exception`.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public enum EnumErrorCode
{
    CameraPermissionDenied = 1001,
    CameraPermissionRestricted = 1002
}
```

## Members

| Member | Value | Description |
| ------ | ----- | ----------- |
| `CameraPermissionDenied` | 1001 | The user denied camera access but can grant it in Settings (on Android, also through a new in-app request). |
| `CameraPermissionRestricted` | 1002 | Device policy withholds camera access (for example Screen Time, parental controls or MDM), and the user can't grant it. |

`ErrorCode` is an `int`, so compare it with a cast:

```csharp
if (result.ErrorCode == (int)EnumErrorCode.CameraPermissionDenied)
{
    // Offer a route into the app's settings page.
}
```

## Remarks

- **Not an exhaustive list.** Capture Vision errors use the same property with codes less than `0`. The bundle's own codes are `1000`–`1999`.
- **The codes differ by remedy.** For `CameraPermissionDenied`, offer a route into the app's settings. For `CameraPermissionRestricted`, explain the restriction instead: Settings has no camera toggle in that state.
- **Handling them is optional.** By default the scanner shows its own alert or dialog first. Set [`IsCameraPermissionPromptEnabled`](mrz-scanner-config.md#iscamerapermissionpromptenabled) to `false` only to present your own UI; the denial is reported either way.

> [!NOTE]
> The codes are the same on Android and iOS. The [`ErrorString`](mrz-scan-result.md#errorstring) wording comes from each platform's scanner and can differ.

### Errors reported by the MAUI layer

Failures in the MAUI layer have no native code. They're reported as `EnumResultStatus.Exception` with an `ErrorCode` of `0`, and [`ErrorString`](mrz-scan-result.md#errorstring) gives the reason. They occur when:

- **Android:** the app has no activity to launch the scanner from, or the scanner closes without a result.
- **iOS:** the app has no page on screen to present the scanner from, or another page is still animating (see [Starting the Scanner from a Modal Page](../user-guide/index.md#starting-the-scanner-from-a-modal-page)).

## Recovering from a denial

- **iOS** asks for camera access only once per install. After a denial, the scanner's alert offers **Open Settings**, and changing the permission there **terminates the app**. The next scan after relaunch picks up the new setting, so keep your own route into Settings on the page that shows the denial, for example with [`AppInfo.ShowSettingsUI()`](https://learn.microsoft.com/dotnet/maui/platform-integration/appmodel/app-information){:target="_blank"}.
- **Android** recovers in place: the app keeps running after the permission is granted in Settings, and the next scan uses it.

The [ScanMRZ Demo App](../samples/scanmrz-walkthrough.md) shows an **Open Settings** button only for `CameraPermissionDenied`.
