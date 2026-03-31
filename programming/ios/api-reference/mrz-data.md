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
| [`firstName`](#firstname) | String | The first name of the user of the MRZ document. |
| [`lastName`](#lastname) | String | The last name of the user of the MRZ document. |
| [`sex`](#sex) | String | The sex of the user of the MRZ document. |
| [`issuingState`](#issuingstate) | String | The issuing state of the MRZ document. |
| [`issuingStateRaw`](#issuingstateraw) | String | The raw issuing state string as it appears in the MRZ. |
| [`nationality`](#nationality) | String | The nationality of the user of the MRZ document. |
| [`nationalityRaw`](#nationalityraw) | String | The raw nationality string as it appears in the MRZ. |
| [`dateOfBirth`](#dateofbirth) | String | The date of birth of the user of the MRZ document. |
| [`dateOfExpire`](#dateofexpire) | String | The expiry date of the MRZ document. |
| [`documentType`](#documenttype) | String | The type of MRZ document. |
| [`documentNumber`](#documentnumber) | String | The MRZ document number. |
| [`personalNumber`](#personalnumber) | String? | The personal number from the MRZ document, if available. |
| [`optionalData1`](#optionaldata1) | String? | The first optional data field from the MRZ, if available. |
| [`optionalData2`](#optionaldata2) | String? | The second optional data field from the MRZ, if available. |
| [`age`](#age) | Int | The age of the user of the MRZ document. |
| [`mrzText`](#mrztext) | String | The raw text of the MRZ. |

### firstName

The first name of the user of the MRZ document.

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

The last name of the user of the MRZ document.

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

The sex of the user of the MRZ document.

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

The issuing state of the MRZ document.

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

### issuingStateRaw

The raw issuing state string as it appears in the MRZ, before any decoding or normalization.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, readonly) NSString* issuingStateRaw;
```
2. 
```swift
let issuingStateRaw: String
```

### nationality

The nationality of the user of the MRZ document.

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

### nationalityRaw

The raw nationality string as it appears in the MRZ, before any decoding or normalization.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, readonly) NSString* nationalityRaw;
```
2. 
```swift
let nationalityRaw: String
```

### dateOfBirth

The date of birth of the user of the MRZ document.

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

The expiry date of the MRZ document.

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

The type of MRZ document.

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

The MRZ document number.

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

### personalNumber

The personal number from the MRZ document. Returns `nil` if not present in the document.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, nullable, readonly) NSString* personalNumber;
```
2. 
```swift
let personalNumber: String?
```

### optionalData1

The first optional data field from the MRZ. Returns `nil` if not present in the document.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, nullable, readonly) NSString* optionalData1;
```
2. 
```swift
let optionalData1: String?
```

### optionalData2

The second optional data field from the MRZ. Returns `nil` if not present in the document.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
@property (nonatomic, nullable, readonly) NSString* optionalData2;
```
2. 
```swift
let optionalData2: String?
```

### age

The age of the user of the MRZ document.

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

The raw text of the MRZ.

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
