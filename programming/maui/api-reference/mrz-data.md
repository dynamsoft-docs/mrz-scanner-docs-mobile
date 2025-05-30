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
| [`FirstName`](#firstname) | *string* | Represents the first name of the user of the MRZ document. |
| [`LastName`](#lastname) | *string* | Represents the last name of the user of the MRZ document. |
| [`Sex`](#sex) | *string* | Represents the sex of the user of the MRZ document. |
| [`IssuingState`](#issuingstate) | *string* | Represents the issuing state of the MRZ document. |
| [`Nationality`](#nationality) | *string* | Represents the nationality of the user of the MRZ document. |
| [`DateOfBirth`](#dateofbirth) | *string* | Represents the date of birth of the user of the MRZ document. |
| [`DateOfExpire`](#dateofexpire) | *string* | Represents the expiry date of the MRZ document. |
| [`DocumentType`](#documenttype) | *string* | Represents the type of MRZ document. |
| [`DocumentNumber`](#documentnumber) | *string* | Represents the MRZ document number. |
| [`Age`](#age) | *int* | Represents the age of the user of the MRZ document. |
| [`MrzText`](#mrztext) | *string* | Represents the raw text of the MRZ. |

### FirstName

Represents the first name of the user of the MRZ document.

```csharp
string FirstName { get; set; };
```

### LastName

Represents the last name of the user of the MRZ document.

```csharp
string LastName { get; set; };
```

### Sex

Represents the sex of the user of the MRZ document.

```csharp
string Sex { get; set; };
```

### IssuingState

Represents the issuing state of the MRZ document.

```csharp
string IssuingState { get; set; };
```

### Nationality

Represents the nationality of the user of the MRZ document.

```csharp
string Nationality { get; set; };
```

### DateOfBirth

Represents the date of birth of the user of the MRZ document.

```csharp
string DateOfBirth { get; set; };
```

### DateOfExpire

Represents the expiry date of the MRZ document.

```csharp
string DateOfExpire { get; set; };
```

### DocumentType

Represents the type of MRZ document.

```csharp
string DocumentType { get; set; };
```

### DocumentNumber

Represents the MRZ document number.

```csharp
string DocumentNumber { get; set; };
```

### Age

Represents the age of the user of the MRZ document.

```csharp
int Age { get; set; };
```

### MrzText

Represents the raw text of the MRZ.

```csharp
string MrzText { get; set; };
```
