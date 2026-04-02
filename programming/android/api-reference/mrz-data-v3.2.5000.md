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

*Assembly:* DynamsoftMRZScanner.aar

*Namespace:* com.dynamsoft.mrzscanner.ui

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
