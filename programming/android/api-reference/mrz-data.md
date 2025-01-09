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
| [`getFirstName`](#getfirstname) | Returns the first name. |
| [`getLastName`](#getlastname) | Returns the last name. |
| [`getSex`](#getsex) | Returns the sex. |
| [`getIssuingState`](#getissuingstate) | Returns the issuing state. |
| [`getNationality`](#getnationality) | Returns the nationality. |
| [`getDateOfBirth`](#getdateofbirth) | Returns the date of birth. |
| [`getDateOfExpire`](#getdateofexpire) | Returns the date of expire. |
| [`getDocumentType`](#getdocumenttype) | Returns the type of document. |
| [`getDocumentNumber`](#getdocumentnumber) | Returns the document number. |
| [`getAge`](#getage) | Returns the age. |
| [`getMrzText`](#getmrztext) | Returns the raw text of MRZ. |

### getFirstName

Returns the first name.

```java
String getFirstName();
```

**Return Value**

The first name.

### getLastName

Returns the last name.

```java
String getLastName();
```

**Return Value**

The last name.

### getSex

Returns the sex.

```java
String getSex();
```

**Return Value**

The sex.

### getIssuingState

Returns the issuing state.

```java
String getIssuingState();
```

**Return Value**

The issuing state.

### getNationality

Returns the nationality.

```java
String getNationality();
```

**Return Value**

The nationality.

### getDateOfBirth

Returns the date of birth.

```java
String getDateOfBirth();
```

**Return Value**

The date of birth.

### getDateOfExpire

Returns the date of expire.

```java
String getDateOfExpire();
```

**Return Value**

The date of expire.

### getDocumentType

Returns the type of document.

```java
String getDocumentType();
```

**Return Value**

The type of document, such as ID cards or passports.

### getDocumentNumber

Returns the document number.

```java
String getDocumentNumber();
```

**Return Value**

The document number.

### getAge

Returns the age.

```java
int getAge();
```

**Return Value**

The age.

### getMrzText

Returns the raw text of MRZ.

```java
String getMrzText();
```

**Return Value**

The raw text of MRZ.
