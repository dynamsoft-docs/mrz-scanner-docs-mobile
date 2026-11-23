---
layout: default-layout
title: Building the ScanMRZ Demo App - Dynamsoft MRZ Scanner MAUI Edition
description: Build the complete ScanMRZ demo app for MAUI - its result page, document images, per-field validation display, and camera-permission recovery.
keywords: demo app, sample, ScanMRZ, result page, MAUI
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: ScanMRZ Demo App
noTitleIndex: true
---

# Building the ScanMRZ Demo App

The [MRZ Scanner User Guide](../user-guide/index.md) builds **ScanMRZBasic**, the smallest app that scans an MRZ and shows the parsed data on a single page. This page walks through **ScanMRZ**, the complete demo app built on the same SDK calls.

Everything on this page is presentation: `ScanMRZ` uses `MRZScannerConfig`, `MRZScanner.Start` and the `MRZScanResult` getters exactly as the user guide does, and adds a result page around them.

> [!NOTE]
> Full source on GitHub: [ScanMRZ](https://github.com/Dynamsoft/mrz-scanner-mobile-maui/tree/main/ScanMRZ){:target="_blank"}. It matches the native ScanMRZ samples, which the **MRZ Scanner Demo** in the app stores is built on.

## What ScanMRZ adds

| | ScanMRZBasic | ScanMRZ |
| --- | --- | --- |
| Pages | one page | `MainPage` plus a dedicated `ResultPage` |
| Result data | key fields, plain text | full field set, in Personal Info and Document Info sections |
| Images | none | portrait and both document sides, with Processed/Original tabs when originals are returned |
| Failed validation | field names listed | amber, underlined, with a warning icon and an explanation dialog |
| Permission denial | error string | error string plus an **Open Settings** button |
| Extras | — | long-press an image to save it to Photos or the gallery |

## Project structure

- **`MainPage`** is the home page. It runs the scanner, opens `ResultPage` for a finished scan, and shows anything else (cancel, errors) itself.
- **`ResultPage`** shows a finished scan. It's pushed onto the navigation stack, so **Re-scan** and **Return home** go back to `MainPage` with `PopToRootAsync`.

## MainPage

### Starting the scanner

Only the license is required, as in the user guide. `ScanMRZ` also serves as a catalog of the other settings: each one is commented out at the opposite of its default, so uncommenting a line shows what it changes.

```csharp
internal static async Task ScanAsync()
{
    var config = new MRZScannerConfig("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9");

    // Every setting below is commented out at the opposite of its default — uncomment to try.
    //config.DocumentType = EnumDocumentType.Passport;
    //config.IsGuideFrameVisible = false;
    //config.ReturnOriginalImage = true;
    //config.IsCameraPermissionPromptEnabled = false;
    // ...

    _pendingResult = await MRZScanner.Start(config);
    PendingResultChanged?.Invoke();
}
```

`ScanAsync` is static so that `ResultPage` can run it for **Re-scan**. [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) covers each setting.

### Keeping the result across page recreation

On Android, the system can rebuild `MainPage` while the scanner is open. `Start` still completes, so the result waits in a static field until a page that is still on screen shows it:

```csharp
private static MRZScanResult? _pendingResult;
private static event Action? PendingResultChanged;

private async void ShowPendingResult()
{
    // A page whose window went away with the old Android activity leaves the result to its replacement.
    if (Window is not { } window || Application.Current?.Windows.Contains(window) != true)
    {
        PendingResultChanged -= ShowPendingResult;
        return;
    }
    if (_pendingResult is not { } result) return;
    _pendingResult = null;

    switch (result.ResultStatus)
    {
        case EnumResultStatus.Finished when result.Data is not null:
            await Navigation.PushAsync(new ResultPage(result));
            break;
        case EnumResultStatus.Finished:
            ShowStatus("Scan returned no data");
            break;
        case EnumResultStatus.Canceled:
            ShowStatus("Scan canceled");
            break;
        default:
            ShowStatus(result.ErrorString ?? "");
            BtnOpenSettings.IsVisible = result.ErrorCode == (int)EnumErrorCode.CameraPermissionDenied;
            break;
    }
}
```

The page subscribes to `PendingResultChanged` in `OnAppearing` and calls `ShowPendingResult` there too, so a rebuilt page picks up a result that arrived while it was gone.

### Recovering from a permission denial

The scanner's own prompt offers **Open Settings**, but on iOS changing the permission there **terminates the app**, so the prompt is gone by the time the user returns. `MainPage` keeps its own route into Settings:

```csharp
private void OnOpenSettings(object sender, EventArgs e) => AppInfo.ShowSettingsUI();
```

The button appears only for `CameraPermissionDenied`. For [`CameraPermissionRestricted`](../api-reference/error-code.md), Settings has no camera toggle, so the error string alone explains the situation. On Android, the app keeps running after the user grants access, and the next scan uses it.

## ResultPage

### What it receives

`ResultPage` takes the whole `MRZScanResult` in its constructor and reads the data and images from it. It needs `using Dynamsoft.Core.Maui;` for `ImageData` and `using Dynamsoft.CodeParser.Maui;` for `EnumValidationStatus`.

### Loading the images

`ToImageSource()` copies and encodes each image on the calling thread, and the originals are full camera frames. So the page opens with the text at once, and each image appears as soon as it has been converted off the UI thread:

```csharp
private async void LoadImagesAsync(params (Image Target, ImageData? Data)[] images)
{
    foreach (var (target, data) in images)
    {
        if (data is null) continue;
        // An image that fails to convert is left out rather than failing the whole result.
        var source = await Task.Run(() =>
        {
            try { return data.ToImageSource(); }
            catch { return null; }
        });
        if (source is not null)
            target.Source = source;
        else if (ReferenceEquals(target, ImgPortrait))
            ShowPortraitPlaceholder();
    }
}
```

The layout is settled before this runs, from which images are `null`, so nothing shifts as the images arrive. A missing portrait shows a placeholder.

### The Processed / Original tabs

When both the cropped ("Processed") and full-frame ("Original") images came back, tabs switch between them. With default settings there are no originals, and a single tab would have nothing to switch, so a plain header is shown instead:

```csharp
private void UpdateImageSectionVisibility(bool hasProcessed, bool hasOriginal)
{
    var showsTabs = hasProcessed && hasOriginal;

    ImageTabs.IsVisible = showsTabs;
    LblImagesHeader.IsVisible = !showsTabs && (hasProcessed || hasOriginal);
    LblImagesHeader.Text = hasProcessed ? "Processed Image(s)" : "Original Image(s)";
    ProcessedImages.IsVisible = hasProcessed;
    OriginalImages.IsVisible = hasOriginal && !hasProcessed;
}
```

With no images at all, the section collapses.

### Showing a failed field

The user guide lists failed field names. `ScanMRZ` marks the value itself: amber, underlined, with a warning icon, and a tap opens an explanation.

```csharp
private void ApplyField(Label label, string value, EnumValidationStatus status = EnumValidationStatus.VS_NONE)
{
    label.Text = string.IsNullOrEmpty(value) ? "N/A" : value;
    if (status != EnumValidationStatus.VS_FAILED || label.Parent is not HorizontalStackLayout cell) return;

    label.TextColor = WarningAmber;
    label.TextDecorations = TextDecorations.Underline;
    // The icon has no text of its own and colour alone isn't a cue, so spell it out.
    SemanticProperties.SetDescription(label, $"{label.Text}, validation failed");

    var iconSize = Math.Round(label.FontSize * 1.2);
    cell.Add(new Image { Source = "warning_circle.png", WidthRequest = iconSize, HeightRequest = iconSize });
    cell.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(ShowValidationInfo) });
}
```

- The icon is sized from the label's font, so it scales with the text.
- The semantic description states the failure in words, since screen readers can't see the color or the icon.
- Only failed rows get a tap gesture, so rows that passed aren't interactive.

### Grouping the fields

The fields are split into a summary header, a **Personal Info** section, and a **Document Info** section:

- **The summary header isn't marked.** It joins several fields into one line, such as "gender, age", and marking it for one field would implicate both.
- **Doc. Type has no status.** It comes from the MRZ layout, not a field with a check digit, and is shown as "ID (TD1)", "ID (TD2)" or "Passport (TD3)".
- **The raw MRZ text is tappable too.** `mrzText` covers whole MRZ lines, so it can fail when no single field does.

### Re-scan and Return home

Both are navigation back to `MainPage`:

```csharp
private async void OnRescan(object? sender, EventArgs e)
{
    await Navigation.PopToRootAsync();
    await MainPage.ScanAsync();
}

private async void OnReturnHome(object? sender, EventArgs e) => await Navigation.PopToRootAsync();
```

**Re-scan** waits for the pop to finish before starting the scanner, since the scanner can't open while a page is still animating.

### Saving an image

A long press (500 ms) on the portrait or a document image offers to save it. The page captures the image with `CaptureAsync` and writes it to Photos on iOS, or to the gallery through `MediaStore` on Android.

- **iOS** needs `NSPhotoLibraryAddUsageDescription` and `NSPhotoLibraryUsageDescription` in `Info.plist`; without them, saving crashes the app.
- **Android** saving needs Android 10 (API 29) or later, where `MediaStore` needs no storage permission. The sample doesn't offer saving on older versions.

## Next steps

- [MRZ Scanner User Guide](../user-guide/index.md) — build `ScanMRZBasic` from an empty project.
- [Customizing the MRZ Scanner](../user-guide/customize-mrz-scanner.md) — document type, UI elements, feedback, and camera permission.
- [MAUI API Reference](../api-reference/index.md) — all classes and methods.
