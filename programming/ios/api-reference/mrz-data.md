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

*Assembly:* DynamsoftMRZScannerBundle.xcframework

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
| [`issuingStateRaw`](#issuingstateraw) | String | The raw ICAO issuing state code as it appears in the MRZ. |
| [`nationality`](#nationality) | String | The nationality of the user of the MRZ document. |
| [`nationalityRaw`](#nationalityraw) | String | The raw ICAO nationality code as it appears in the MRZ. |
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

The raw ICAO issuing state code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

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

The raw ICAO nationality code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

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

## Methods

| Method | Description |
| ------ | ----------- |
| [`getFieldValidationStatus`](#getfieldvalidationstatus) | Returns the validation status of a single parsed MRZ field. |

### getFieldValidationStatus

Returns the validation status of a single parsed MRZ field. Most MRZ fields are protected by a check digit, and this method reports whether the value read from the document matched it.

A status of `failed` means the value does not agree with its check digit — the document may be misread, invalid, or altered. The value is still returned by the corresponding property, so you can choose whether to accept it, prompt for a re-scan, or ask for manual correction.

<div class="sample-code-prefix"></div>
>- Objective-C
>- Swift
>
>1. 
```objc
- (DSValidationStatus)getFieldValidationStatus:(NSString *)fieldName;
```
2. 
```swift
func getFieldValidationStatus(_ fieldName: String) -> ValidationStatus
```

**Parameters**

`fieldName`: The name of the field to query, using the same names as the properties on this class. The accepted values are:

| `fieldName` | Corresponding property |
| ----------- | ---------------------- |
| `firstName` | [`firstName`](#firstname) |
| `lastName` | [`lastName`](#lastname) |
| `sex` | [`sex`](#sex) |
| `issuingState` | [`issuingState`](#issuingstate) |
| `nationality` | [`nationality`](#nationality) |
| `dateOfBirth` | [`dateOfBirth`](#dateofbirth) |
| `dateOfExpire` | [`dateOfExpire`](#dateofexpire) |
| `documentNumber` | [`documentNumber`](#documentnumber) |
| `mrzText` | [`mrzText`](#mrztext) |
| `optionalData1` | [`optionalData1`](#optionaldata1) |
| `optionalData2` | [`optionalData2`](#optionaldata2) |
| `personalNumber` | [`personalNumber`](#personalnumber) |

**Return Value**

A `ValidationStatus` value:

| Member | Value | Description |
| ------ | ----- | ----------- |
| `DSValidationStatusNone` / `none` | 0 | The field has no validation specified. |
| `DSValidationStatusSucceeded` / `succeeded` | 1 | The validation for the field succeeded. |
| `DSValidationStatusFailed` / `failed` | 2 | The validation for the field failed. |

`ValidationStatus` belongs to Dynamsoft Code Parser rather than to this SDK, and is imported from `DynamsoftCaptureVisionBundle`. See the [ValidationStatus reference](https://www.dynamsoft.com/code-parser/docs/mobile/programming/ios/api-reference/enum/validation-status.html?lang=objc,swift){:target="_blank"} for its full definition.

> [!NOTE]
> `dateOfBirth`, `dateOfExpire` and `mrzText` are composites of several underlying MRZ fields. Each returns the worst status among its components: `failed` if any component failed, otherwise `succeeded` if any component passed, otherwise `none`.
>
> Because `mrzText` aggregates whole MRZ lines, it can report `failed` when no individual field does. This happens when the corruption falls in a field that carries no check digit of its own, such as name, nationality, or sex.

An unrecognized `fieldName` returns `none` rather than raising an error, as does any field for which no validation information was captured.
