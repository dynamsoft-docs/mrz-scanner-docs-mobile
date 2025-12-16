---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner React Native Edition
description: MRZScanResult of DynamsoftMRZScanner React Native is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is the most basic class to represent the full MRZ result object. It comes with a result status and the parsed MRZ info as a MRZData object.

## Definition

*Assembly:* dynamsoft-capture-vision-react-native

```ts
class MRZScanResult
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`data`](#mrzdata) | [*MRZData*](mrz-data.md) | Represents the parsed MRZ data. |
| [`resultStatus`](#resultstatus) | [*EnumResultStatus*](result-status.md) | Represents the status of the result, which can be finished, canceled or exception. |
| [`errorCode`](#errorcode) | *number?* | Represents the error code should something go wrong during the MRZ scanning process. |
| [`errorString`](#errorstring) | *string?* | Represents the error message associated with the error code should something go wrong during the MRZ scanning process. |

### mrzData

Represents the parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```ts
data?: MRZData
```

### resultStatus

Represents the status of the result, which can be finished, canceled or exception.

```ts
resultStatus: EnumResultStatus
```

**Remarks**

The result status can be one of three things:

- `RS_FINISHED`: The MRZ scanning is finished.
- `RS_CANCELED`: The MRZ scanning activity is closed before the process is finished.
- `RS_EXCEPTION`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### errorCode

Returns the error code when an exception occurs. This value is only valid when resultStatus is `RS_EXCEPTION`.

```ts
errorCode?: number
```

### errorString

Returns the error message associated with the error code when an exception occurs. This value is only valid when resultStatus is `RS_EXCEPTION`.

```ts
errorString?: string
```
