---
layout: default-layout
title: MRZScanResult Class - Dynamsoft MRZ Scanner Android Edition
description: MRZScanResult of DynamsoftMRZScanner Android is a result class that contains the parsed MRZ information from one scan and the additional information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZScanResult
---

# MRZScanResult

`MRZScanResult` is a result class that contains the parsed MRZ information and the corresponding additional information.

## Definition

*Assembly:* DynamsoftMRZScanner.aar

*Namespace:* com.dynamsoft.mrzscanner.ui

```java
class MRZScanResult
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`getData`](#getdata) | Returns the parsed MRZ data. |
| [`getResultStatus`](#getresultstatus) | Returns the result status, which can be finished, canceled or exception. |
| [`getErrorCode`](#geterrorcode) | Returns the error code should something go wrong during the MRZ scanning process. |
| [`getErrorString`](#geterrorstring) | Returns the error message associated with the error code should something go wrong during the MRZ scanning process. |

### getData

Returns the parsed MRZ information as a [`MRZData`](mrz-data.md) object.

```java
MRZData getData();
```

**Return Value**

A [`MRZData`](mrz-data.md) object that contains the parsed MRZ data.

### getResultStatus

Returns the status of the result, which can be finished, canceled or exception.

```java
@EnumResultStatus
int getResultStatus();
```

**Return Value**

The status of the result, which can be finished, canceled or exception.

- `RS_FINISHED`: The MRZ scanning is finished.
- `RS_CANCELED`: The MRZ scanning activity is closed before the process is finished.
- `RS_EXCEPTION`: Failed to start MRZ scanning or an error occurs when scanning the MRZ.

### getErrorCode

Returns the error code should something go wrong during the MRZ scanning process.

```java
int getErrorCode();
```

**Return Value**

An integer representing a `EnumErrorCode`.

### getErrorString

Returns the error message associated with the error code should something go wrong during the MRZ scanning process.

```java
String getErrorString();
```

**Return Value**

A string representing the message of a `EnumErrorCode`.
