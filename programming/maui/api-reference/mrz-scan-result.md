---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZScanResult of DynamsoftMRZScanner MAUI is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is a result class that contains the parsed MRZ information and the corresponding additional information.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
class MRZScanResult
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`Data`](#data) | [*MRZData?*](mrz-data.md) | Represents the parsed MRZ data. |
| [`ResultStatus`](#resultstatus) | [*EnumResultStatus*](result-status.md) | Represents the result status, which can be finished, canceled or exception. |
| [`ErrorCode`](#errorcode) | *int* | Represents the error code should something go wrong during the MRZ scanning process. |
| [`ErrorString`](#errorstring) | *string* | Represents the error message associated with the error code should something go wrong during the MRZ scanning process. |

### Data

Represents the parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```csharp
MRZData? Data {get; set;};
```

**See also**

- [`MRZData`](mrz-data.md)

### ResultStatus

Represents the status of the result, which can be finished, canceled or exception.

```csharp
EnumResultStatus ResultStatus {get; set;};
```

- `Finished`: The MRZ scanning is finished.
- `Canceled`: The MRZ scanning activity is closed before the process is finished.
- `Exception`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### ErrorCode

Represents the error code should something go wrong during the MRZ scanning process.

```csharp
int ErrorCode {get; set;};
```

### ErrorString

Represents the error message associated with the error code should something go wrong during the MRZ scanning process.

```csharp
string? ErrorString {get; set;};
```
