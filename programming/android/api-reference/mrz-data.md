---
layout: default-layout
title: MRZData Class - Dynamsoft MRZ Scanner Android Edition
description: MRZData of DynamsoftMRZScanner Android is a result class that contains the parsed MRZ information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZData
---

# MRZData

`MRZData` is a result class that contains the parsed MRZ information.

## Definition

*Assembly:* MRZScannerBundle.aar

*Namespace:* com.dynamsoft.mrzscannerbundle.ui

```java
class MRZData
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`getFirstName`](#getfirstname) | Returns the first name of the user of the MRZ document. |
| [`getLastName`](#getlastname) | Returns the last name of the user of the MRZ document. |
| [`getSex`](#getsex) | Returns the sex of the user of the MRZ document. |
| [`getIssuingState`](#getissuingstate) | Returns the issuing state of the MRZ document. |
| [`getNationality`](#getnationality) | Returns the nationality of the user of the MRZ document. |
| [`getDateOfBirth`](#getdateofbirth) | Returns the date of birth of the user of the MRZ document. |
| [`getDateOfExpire`](#getdateofexpire) | Returns the expiry date of the MRZ document. |
| [`getDocumentType`](#getdocumenttype) | Returns the type of MRZ document. |
| [`getDocumentNumber`](#getdocumentnumber) | Returns the MRZ document number. |
| [`getAge`](#getage) | Returns the age of the user of the MRZ document. |
| [`getMrzText`](#getmrztext) | Returns the raw text of the MRZ. |
| [`getIssuingStateRaw`](#getissuingstateraw) | Returns the raw ICAO issuing state code as it appears in the MRZ. |
| [`getNationalityRaw`](#getnationalityraw) | Returns the raw ICAO nationality code as it appears in the MRZ. |
| [`getOptionalData1`](#getoptionaldata1) | Returns the first optional data field from the MRZ. |
| [`getOptionalData2`](#getoptionaldata2) | Returns the second optional data field from the MRZ. |
| [`getPersonalNumber`](#getpersonalnumber) | Returns the personal number from the MRZ. |
| [`getFieldValidationStatus`](#getfieldvalidationstatus) | Returns the validation status of a single parsed MRZ field. |

### getFirstName

Returns the first name of the user of the MRZ document.

```java
String getFirstName();
```

**Return Value**

The first name.

### getLastName

Returns the last name of the user of the MRZ document.

```java
String getLastName();
```

**Return Value**

The last name.

### getSex

Returns the sex of the user of the MRZ document.

```java
String getSex();
```

**Return Value**

The sex.

### getIssuingState

Returns the issuing state of the MRZ document.

```java
String getIssuingState();
```

**Return Value**

The issuing state.

### getNationality

Returns the nationality of the user of the MRZ document.

```java
String getNationality();
```

**Return Value**

The nationality.

### getDateOfBirth

Returns the date of birth of the user of the MRZ document.

```java
String getDateOfBirth();
```

**Return Value**

The date of birth.

### getDateOfExpire

Returns the expiry date of the MRZ document.

```java
String getDateOfExpire();
```

**Return Value**

The date of expiry.

### getDocumentType

Returns the type of MRZ document.

```java
String getDocumentType();
```

**Return Value**

The type of document, such as ID cards or passports.

### getDocumentNumber

Returns the MRZ document number.

```java
String getDocumentNumber();
```

**Return Value**

The document number.

### getAge

Returns the age of the user of the MRZ document.

```java
int getAge();
```

**Return Value**

The age.

### getMrzText

Returns the raw text of the MRZ.

```java
String getMrzText();
```

**Return Value**

The raw text of the MRZ.

### getIssuingStateRaw

Returns the raw ICAO issuing state code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

```java
String getIssuingStateRaw();
```

**Return Value**

The raw ICAO issuing state code.

### getNationalityRaw

Returns the raw ICAO nationality code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

```java
String getNationalityRaw();
```

**Return Value**

The raw ICAO nationality code.

### getOptionalData1

Returns the first optional data field from the MRZ. The content depends on the document type and issuing authority.

```java
@Nullable
String getOptionalData1();
```

**Return Value**

The first optional data field, or `null` if not present.

### getOptionalData2

Returns the second optional data field from the MRZ. The content depends on the document type and issuing authority.

```java
@Nullable
String getOptionalData2();
```

**Return Value**

The second optional data field, or `null` if not present.

### getPersonalNumber

Returns the personal number from the MRZ. This field is typically found on TD3 (passport) documents.

```java
@Nullable
String getPersonalNumber();
```

**Return Value**

The personal number, or `null` if not present.

### getFieldValidationStatus

Returns the validation status of a single parsed MRZ field. Most MRZ fields are protected by a check digit, and this method reports whether the value read from the document matched it.

A status of `VS_FAILED` means the value does not agree with its check digit — the document may be misread, invalid, or altered. The value is still returned by the corresponding getter, so you can choose whether to accept it, prompt for a re-scan, or ask for manual correction.

```java
int getFieldValidationStatus(String fieldName);
```

**Parameters**

`fieldName`: The name of the field to query, using the same names as the getters on this class. The accepted values are:

| `fieldName` | Corresponding getter |
| ----------- | -------------------- |
| `firstName` | [`getFirstName`](#getfirstname) |
| `lastName` | [`getLastName`](#getlastname) |
| `sex` | [`getSex`](#getsex) |
| `issuingState` | [`getIssuingState`](#getissuingstate) |
| `nationality` | [`getNationality`](#getnationality) |
| `dateOfBirth` | [`getDateOfBirth`](#getdateofbirth) |
| `dateOfExpire` | [`getDateOfExpire`](#getdateofexpire) |
| `documentNumber` | [`getDocumentNumber`](#getdocumentnumber) |
| `mrzText` | [`getMrzText`](#getmrztext) |
| `optionalData1` | [`getOptionalData1`](#getoptionaldata1) |
| `optionalData2` | [`getOptionalData2`](#getoptionaldata2) |
| `personalNumber` | [`getPersonalNumber`](#getpersonalnumber) |

**Return Value**

An integer representing an `EnumValidationStatus` value:

| Member | Value | Description |
| ------ | ----- | ----------- |
| `VS_NONE` | 0 | The field has no validation specified. |
| `VS_SUCCEEDED` | 1 | The validation for the field succeeded. |
| `VS_FAILED` | 2 | The validation for the field failed. |

`EnumValidationStatus` belongs to Dynamsoft Code Parser rather than to this SDK, and is imported from `com.dynamsoft.dcp`. See the [EnumValidationStatus reference](https://www.dynamsoft.com/code-parser/docs/mobile/programming/android/api-reference/enum/validation-status.html?lang=android){:target="_blank"} for its full definition.

> [!NOTE]
> `dateOfBirth`, `dateOfExpire` and `mrzText` are composites of several underlying MRZ fields. Each returns the worst status among its components: `VS_FAILED` if any component failed, otherwise `VS_SUCCEEDED` if any component passed, otherwise `VS_NONE`.
>
> Because `mrzText` aggregates whole MRZ lines, it can report `VS_FAILED` when no individual field does. This happens when the corruption falls in a field that carries no check digit of its own, such as name, nationality, or sex.

An unrecognized `fieldName` returns `VS_NONE` rather than raising an error, as does any `MRZData` instance deserialized from a bundle older than 3.6.2000.
