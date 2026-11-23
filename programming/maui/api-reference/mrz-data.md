---
layout: default-layout
title: MRZData Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZData of DynamsoftMRZScanner MAUI is a result class that contains the parsed MRZ information.
keywords: MRZ, scanner, scan result, validation
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZData
---

# MRZData

`MRZData` is a result class that contains the parsed MRZ information.

## Definition

*Assembly:* Dynamsoft.MRZScannerBundle.Maui

*Namespace:* Dynamsoft.MRZScannerBundle.Maui

```csharp
public class MRZData
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`FirstName`](#firstname) | *string* | The first name of the document holder. |
| [`LastName`](#lastname) | *string* | The last name of the document holder. |
| [`Sex`](#sex) | *string* | The sex of the document holder. |
| [`IssuingState`](#issuingstate) | *string* | The issuing state of the document. |
| [`IssuingStateRaw`](#issuingstateraw) | *string* | The raw ICAO issuing state code as it appears in the MRZ. |
| [`Nationality`](#nationality) | *string* | The nationality of the document holder. |
| [`NationalityRaw`](#nationalityraw) | *string* | The raw ICAO nationality code as it appears in the MRZ. |
| [`DateOfBirth`](#dateofbirth) | *string* | The date of birth of the document holder. |
| [`DateOfExpire`](#dateofexpire) | *string* | The expiry date of the document. |
| [`DocumentType`](#documenttype) | *string* | The type of MRZ document. |
| [`DocumentNumber`](#documentnumber) | *string* | The document number. |
| [`PersonalNumber`](#personalnumber) | *string* | The personal number, or an empty string if absent. |
| [`OptionalData1`](#optionaldata1) | *string* | The first optional data field, or an empty string if absent. |
| [`OptionalData2`](#optionaldata2) | *string* | The second optional data field, or an empty string if absent. |
| [`Age`](#age) | *int* | The age of the document holder. |
| [`MrzText`](#mrztext) | *string* | The raw text of the MRZ. |

### FirstName

The first name of the document holder.

```csharp
string FirstName { get; }
```

### LastName

The last name of the document holder.

```csharp
string LastName { get; }
```

### Sex

The sex of the document holder. Can be empty when the MRZ leaves it unspecified.

```csharp
string Sex { get; }
```

### IssuingState

The issuing state of the document.

```csharp
string IssuingState { get; }
```

### IssuingStateRaw

The raw ICAO issuing state code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

```csharp
string IssuingStateRaw { get; }
```

### Nationality

The nationality of the document holder.

```csharp
string Nationality { get; }
```

### NationalityRaw

The raw ICAO nationality code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

```csharp
string NationalityRaw { get; }
```

### DateOfBirth

The date of birth of the document holder.

```csharp
string DateOfBirth { get; }
```

### DateOfExpire

The expiry date of the document.

```csharp
string DateOfExpire { get; }
```

### DocumentType

The type of MRZ document.

```csharp
string DocumentType { get; }
```

### DocumentNumber

The document number.

```csharp
string DocumentNumber { get; }
```

### PersonalNumber

The personal number, typically found on TD3 (passport) documents. An empty string if the document has none.

```csharp
string PersonalNumber { get; }
```

### OptionalData1

The first optional data field. Its content depends on the document type and issuing authority. An empty string if the document has none.

```csharp
string OptionalData1 { get; }
```

### OptionalData2

The second optional data field. Its content depends on the document type and issuing authority. An empty string if the document has none.

```csharp
string OptionalData2 { get; }
```

### Age

The age of the document holder.

```csharp
int Age { get; }
```

### MrzText

The raw text of the MRZ.

```csharp
string MrzText { get; }
```

## Methods

| Method | Description |
| ------ | ----------- |
| [`GetFieldValidationStatus`](#getfieldvalidationstatus) | Returns the check-digit validation status of a parsed field. |

### GetFieldValidationStatus

Returns whether a parsed field matched its check digit. A field that fails is still returned by its property, so you can accept it, re-scan, or ask for a correction.

```csharp
public EnumValidationStatus GetFieldValidationStatus(string fieldName);
```

**Parameter(s)**

`fieldName`: The field name in camelCase, as the native libraries use it:

| `fieldName` | Corresponding property |
| ----------- | ---------------------- |
| `firstName` | [`FirstName`](#firstname) |
| `lastName` | [`LastName`](#lastname) |
| `sex` | [`Sex`](#sex) |
| `issuingState` | [`IssuingState`](#issuingstate) |
| `nationality` | [`Nationality`](#nationality) |
| `dateOfBirth` | [`DateOfBirth`](#dateofbirth) |
| `dateOfExpire` | [`DateOfExpire`](#dateofexpire) |
| `documentNumber` | [`DocumentNumber`](#documentnumber) |
| `mrzText` | [`MrzText`](#mrztext) |
| `optionalData1` | [`OptionalData1`](#optionaldata1) |
| `optionalData2` | [`OptionalData2`](#optionaldata2) |
| `personalNumber` | [`PersonalNumber`](#personalnumber) |

**Return Value**

`VS_NONE` (0) if the field has no validation, `VS_SUCCEEDED` (1) or `VS_FAILED` (2).

**Remarks**

- `EnumValidationStatus` is in the `Dynamsoft.CodeParser.Maui` namespace, so add `using Dynamsoft.CodeParser.Maui;` to compare the result. See the [EnumValidationStatus reference](https://www.dynamsoft.com/code-parser/docs/mobile/programming/maui/api-reference/enum/validation-status.html){:target="_blank"}.
- `dateOfBirth`, `dateOfExpire` and `mrzText` combine several MRZ fields and return the worst status among them.
