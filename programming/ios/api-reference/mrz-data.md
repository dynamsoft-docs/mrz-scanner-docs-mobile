---
layout: default-layout
title: MRZData Class - Dynamsoft MRZ Scanner iOS Edition
description: MRZData of DynamsoftMRZScanner iOS is a result class that contains the parsed MRZ information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZData
---

# MRZData

`MRZData` is a result class that contains the parsed MRZ information.

## Definition

*Assembly:* DynamsoftMRZScanner.xcframework

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@interface DSMRZData : NSObject
```
2. 
```swift
class MRZData : NSObject
```

## Properties

| Property | Type | Description |
| -------- | ------ | ----------- |
| [`firstName`](#firstname) | String | The first name. |
| [`lastName`](#lastname) | String | The last name. |
| [`sex`](#sex) | String | The sex. |
| [`issuingState`](#issuingstate) | String | The issuing state. |
| [`nationality`](#nationality) | String | The nationality. |
| [`dateOfBirth`](#dateofbirth) | String | The date of birth. |
| [`dateOfExpire`](#dateofexpire) | String | The date of expire. |
| [`documentType`](#documenttype) | String | The type of document. |
| [`documentNumber`](#documentnumber) | String | The document number. |
| [`age`](#age) | Int | The age. |
| [`mrzText`](#mrztext) | String | The raw text of MRZ. |

### firstName

The first name.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, readonly) NSString* firstName;
```
2. 
```swift
let firstName: String
```

### lastName

The last name.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* lastName;
``` 
2. 
```swift
let lastName: String
```

### sex

The sex.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* sex;
``` 
2. 
```swift
let sex: String
```

### issuingState

The issuing state.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* issuingState;
``` 
2. 
```swift
let issuingState: String
```

### nationality

The nationality.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* nationality;
``` 
2. 
```swift
let nationality: String
``` 

### dateOfBirth

The date of birth.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* dateOfBirth;
``` 
2. 
```swift
let dateOfBirth: String
```

### dateOfExpire

The date of expire.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* dateOfExpire;
``` 
2. 
```swift
let dateOfExpire: String
```

### documentType

The type of document.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* documentType;
``` 
2. 
```swift
let documentType: String
```

### documentNumber

The document number.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* documentNumber;
``` 
2. 
```swift
let documentNumber: String
```

### age

The age.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc 
@property (nonatomic, readonly) int age;
```
2. 
```swift
let age: Int
```

### mrzText

The raw text of MRZ.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1.
```objc
@property (nonatomic, readonly) NSString* mrzText;
``` 
2. 
```swift
let mrzText: String
```
