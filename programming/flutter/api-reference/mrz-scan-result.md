---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner Flutter Edition
description: MRZScanResult of DynamsoftMRZScanner Flutter is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is the most basic class to represent the full MRZ result object. It comes with a result status and the parsed MRZ info as a MRZData object.

## Definition

*Assembly:* dynamsoft_mrz_scanner_bundle_flutter

```dart
class MRZScanResult
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`mrzData`](#mrzdata) | [*MRZData?*](mrz-data.md) | Represents the parsed MRZ data. |
| [`status`](#status) | [*EnumResultStatus*](result-status.md) | Represents the status of the result, which can be finished, canceled or exception. |
| [`errorCode`](#errorcode) | *int?* | Represents the error code should something go wrong during the MRZ scanning process. |
| [`errorString`](#errorstring) | *String?* | Represents the error message associated with the error code should something go wrong during the MRZ scanning process. |

### mrzData

Represents the parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```dart
MRZData? mrzData;
```

### status

Represents the status of the result, which can be finished, canceled or exception. The status comes in the form of a [`EnumResultStatus`](result-status.md).

```dart
EnumResultStatus status;
```

**Remarks**

The result status can be one of three things:

- `finished`: The MRZ scanning is finished.
- `canceled`: The MRZ scanning activity is closed before the process is finished.
- `exception`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### errorCode

Returns the error code when an exception occurs. This value is only valid when resultStatus is `exception`.

```dart
int? errorCode;
```

### errorString

Returns the error message associated with the error code when an exception occurs. This value is only valid when resultStatus is `exception`.

```dart
String? errorMessage;
```
