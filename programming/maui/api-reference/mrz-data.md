---
layout: default-layout
title: MRZData Class - Dynamsoft MRZ Scanner MAUI Edition
description: MRZData of DynamsoftMRZScanner MAUI is a result class that contains the parsed MRZ information.
keywords: MRZ, scanner, scan result
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
class MRZData
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`FirstName`](#firstname) | *string* | The first name of the user of the MRZ document. |
| [`LastName`](#lastname) | *string* | The last name of the user of the MRZ document. |
| [`Sex`](#sex) | *string* | The sex of the user of the MRZ document. |
| [`IssuingState`](#issuingstate) | *string* | The issuing state of the MRZ document. |
| [`IssuingStateRaw`](#issuingstateraw) | *string* | The raw ICAO issuing state code as it appears in the MRZ. |
| [`Nationality`](#nationality) | *string* | The nationality of the user of the MRZ document. |
| [`NationalityRaw`](#nationalityraw) | *string* | The raw ICAO nationality code as it appears in the MRZ. |
| [`DateOfBirth`](#dateofbirth) | *string* | The date of birth of the user of the MRZ document. |
| [`DateOfExpire`](#dateofexpire) | *string* | The expiry date of the MRZ document. |
| [`DocumentType`](#documenttype) | *string* | The type of MRZ document. |
| [`DocumentNumber`](#documentnumber) | *string* | The MRZ document number. |
| [`PersonalNumber`](#personalnumber) | *string?* | The personal number from the MRZ document, if available. |
| [`OptionalData1`](#optionaldata1) | *string?* | The first optional data field from the MRZ, if available. |
| [`OptionalData2`](#optionaldata2) | *string?* | The second optional data field from the MRZ, if available. |
| [`Age`](#age) | *int* | The age of the user of the MRZ document. |
| [`MrzText`](#mrztext) | *string* | The raw text of the MRZ. |

### FirstName

The first name of the user of the MRZ document.

```csharp
string FirstName { get; }
```

### LastName

The last name of the user of the MRZ document.

```csharp
string LastName { get; }
```

### Sex

The sex of the user of the MRZ document.

```csharp
string Sex { get; }
```

### IssuingState

The issuing state of the MRZ document.

```csharp
string IssuingState { get; }
```

### IssuingStateRaw

The raw ICAO issuing state code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

```csharp
string IssuingStateRaw { get; }
```

### Nationality

The nationality of the user of the MRZ document.

```csharp
string Nationality { get; }
```

### NationalityRaw

The raw ICAO nationality code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

```csharp
string NationalityRaw { get; }
```

### DateOfBirth

The date of birth of the user of the MRZ document.

```csharp
string DateOfBirth { get; }
```

### DateOfExpire

The expiry date of the MRZ document.

```csharp
string DateOfExpire { get; }
```

### DocumentType

The type of MRZ document.

```csharp
string DocumentType { get; }
```

### DocumentNumber

The MRZ document number.

```csharp
string DocumentNumber { get; }
```

### PersonalNumber

The personal number from the MRZ document. This field is typically found on TD3 (passport) documents. Returns `null` if not present.

```csharp
string? PersonalNumber { get; }
```

### OptionalData1

The first optional data field from the MRZ. The content depends on the document type and issuing authority. Returns `null` if not present.

```csharp
string? OptionalData1 { get; }
```

### OptionalData2

The second optional data field from the MRZ. The content depends on the document type and issuing authority. Returns `null` if not present.

```csharp
string? OptionalData2 { get; }
```

### Age

The age of the user of the MRZ document.

```csharp
int Age { get; }
```

### MrzText

The raw text of the MRZ.

```csharp
string MrzText { get; }
```
