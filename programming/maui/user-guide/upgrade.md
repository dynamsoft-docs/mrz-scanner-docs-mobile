---
layout: default-layout
title: How to update - Dynamsoft MRZ Scanner for MAUI
description: Follow the upgrade instructions to learn to upgrade MRZ Scanner SDK MAUI edition.
keywords: updates guide, maui
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
multiProgrammingLanguage: true
enableLanguageSelection: true
---

# How to Upgrade

## From v3.4.x to v3.6.x

### Update the Package

1. Update the package to the new version:

   ```bash
   dotnet add package Dynamsoft.MRZScannerBundle.Maui --version 3.6.2000
   ```

2. Raise the iOS minimum to **16.0**, as required by the scanner's Core ML models:

   ```xml
   <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'ios'">16.0</SupportedOSPlatformVersion>
   ```

The .NET 10 and .NET MAUI 10.0.20 requirements are unchanged.

> [!NOTE]
> If you also move to Xcode 27, your app must adopt the iOS UIScene lifecycle. See the note in [Step 1](index.md#step-1-create-a-new-project) of the user guide.

### Handle Behavior Changes

No public API was removed or renamed in 3.6.x, so existing code keeps compiling. These behavior changes can still affect it.

#### Results That Fail Check-Digit Validation Are Now Delivered

Through 3.4.x, the scanner discarded results that failed check-digit validation and kept scanning, so a damaged or altered document produced no result. 3.6.x returns the result and reports the failure per field:

```csharp
using Dynamsoft.CodeParser.Maui;

if (result.Data?.GetFieldValidationStatus("documentNumber") == EnumValidationStatus.VS_FAILED)
{
    // Accept the value, re-scan, or ask for a correction.
}
```

If your code assumed every result was check-digit-clean, add this check. See [Reading a field's validation status](index.md#step-5-launch-the-scanner-and-show-the-result) and [`GetFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus).

#### Camera Access Is Now Gated and Reported

Through 3.4.x, the scanner opened the camera without checking access, so a denied permission left the user on a blank preview. 3.6.x never opens the camera without access, and reports the denial as `EnumResultStatus.Exception` with [`CameraPermissionDenied`](../api-reference/error-code.md) (1001) or [`CameraPermissionRestricted`](../api-reference/error-code.md) (1002).

- **Handle `Exception`.** Code that ignored it will now miss a denial the SDK reports.
- **If your app already shows its own permission UI**, turn off the scanner's prompt so the user doesn't see two:

  ```csharp
  config.IsCameraPermissionPromptEnabled = false;
  ```

See [Handling Camera Permission](customize-mrz-scanner.md#handling-camera-permission).

#### The Scan Region Is Now the Guide Frame

Through 3.4.x, the whole camera preview was scanned, so a document outside the guide frame could still be read. 3.6.x scans only inside the frame. Hiding the frame restores the whole-preview behavior; see [Hiding the guide frame](customize-mrz-scanner.md#hiding-the-guide-frame).

#### Smaller Changes

- **On iOS, `PersonalNumber`, `OptionalData1` and `OptionalData2` are `""` instead of `null`** when absent, matching Android. Check them with `string.IsNullOrEmpty`.
- **`Start` no longer changes `TemplateFile`.** Through 3.4.x it could replace the property's value with the template's contents.
- **A `TemplateFile` name that isn't an app asset no longer throws.** Through 3.4.x, `Start` threw `FileNotFoundException`; now the value is passed to native as a file path, and a failure comes back as an `Exception` result.
- **`Start` reports failures to open the scanner** as an `Exception` result instead of never completing: on Android when there's no activity, and on iOS when another page is still animating. See [Starting the Scanner from a Modal Page](index.md#starting-the-scanner-from-a-modal-page).
- **A second `Start` during a scan returns the running scan's task** instead of opening another scanner.

### Adopt the New APIs

These are additive, so adopting them is optional:

- [`GetFieldValidationStatus`](../api-reference/mrz-data.md#getfieldvalidationstatus) — per-field check-digit status.
- [`EnumErrorCode`](../api-reference/error-code.md) — the bundle's own error codes, both about camera access.
- [`IsCameraPermissionPromptEnabled`](../api-reference/mrz-scanner-config.md#iscamerapermissionpromptenabled) — turns off the scanner's permission prompt.
- [`MRZScannerConfig()`](../api-reference/mrz-scanner-config.md#constructors) — a parameterless constructor; set `License` afterwards.

The scanner UI also gains a progress spinner and a flip prompt for two-sided ID cards, with no code needed. See [The Scanner Screen](index.md#the-scanner-screen).
