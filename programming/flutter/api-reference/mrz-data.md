---
layout: default-layout
title: MRZData Class - Dynamsoft MRZ Scanner Flutter Edition
description: MRZData of DynamsoftMRZScanner Flutter is a result class that contains the parsed MRZ information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZData
---

# MRZData

`MRZData` is a result class that is used to represent the parsed MRZ information.

## Definition

*Assembly:* dynamsoft_mrz_scanner_bundle_flutter

```dart
class MRZData
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`firstName`](#firstname) | *String* | The first name of the MRZ document holder. |
| [`lastName`](#lastname) | *String* | The last name of the MRZ document holder. |
| [`sex`](#sex) | *String* | The sex of the MRZ document holder. |
| [`issuingState`](#issuingstate) | *String* | The issuing state (represented as the full name of the country/region) of the MRZ document. |
| [`nationality`](#nationality) | *String* | The nationality (represented as the full name of the country/region) of the MRZ document holder. |
| [`dateOfBirth`](#dateofbirth) | *String* | The date of birth of the MRZ document holder. |
| [`dateOfExpire`](#dateofexpire) | *String* | The expiry date of the MRZ document. |
| [`documentType`](#documenttype) | *String* | The type of MRTD that the MRZ document is. |
| [`documentNumber`](#documentnumber) | *String* | The MRZ document number. |
| [`age`](#age) | *int* | The age of the MRZ document holder. |
| [`mrzText`](#mrztext) | *String* | The raw unparsed text of the MRZ. |

### firstName

Represents the first name of the MRZ document holder.

```dart
String firstName;
```

### lastName

Represents the last name of the MRZ document holder.

```dart
String lastName;
```

### sex

Represents the sex of the MRZ document holder.

```dart
String sex;
```

### issuingState

Represents the issuing state of the MRZ document.

```dart
String issuingState;
```

### nationality

Represents the nationality of the MRZ document holder.

```dart
String nationality;
```

### dateOfBirth

Represents the date of birth of the MRZ document holder.

```dart
String dateOfBirth;
```

### dateOfExpire

Represents the expiry date of the MRZ document.

```dart
String dateOfExpire;
```

### documentType

Represents the type of MRZ document.

```dart
String documentType;
```

### documentNumber

Represents the MRZ document number.

```dart
String documentNumber;
```

### age

Represents the age of the MRZ document holder.

```dart
int age;
```

### mrzText

Represents the raw text of the MRZ.

```dart
String mrzText;
```
