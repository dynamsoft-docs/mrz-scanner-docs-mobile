---
layout: default-layout
title: MRZData Class - Dynamsoft MRZ Scanner React Native Edition
description: MRZData of DynamsoftMRZScanner React Native is a result class that contains the parsed MRZ information.
keywords: MRZ, scanner, scan result
needAutoGenerateSidebar: true
needGenerateH3Content: true
breadcrumbText: MRZData
---

# MRZData

`MRZData` is a result class that is used to represent the parsed MRZ information.

## Definition

*Assembly:* dynamsoft-mrz-scanner-bundle-react-native

```ts
interface MRZData
```

## Properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| [`firstName`](#firstname) | *string* | The first name of the MRZ document holder. |
| [`lastName`](#lastname) | *string* | The last name of the MRZ document holder. |
| [`sex`](#sex) | *string* | The sex of the MRZ document holder. |
| [`issuingState`](#issuingstate) | *string* | The issuing state (represented as the full name of the country/region) of the MRZ document. |
| [`issuingStateRaw`](#issuingstateraw) | *string* | The raw ICAO issuing state code of the MRZ document. |
| [`nationality`](#nationality) | *string* | The nationality (represented as the full name of the country/region) of the MRZ document holder. |
| [`nationalityRaw`](#nationalityraw) | *string* | The raw ICAO nationality code of the MRZ document holder. |
| [`dateOfBirth`](#dateofbirth) | *string* | The date of birth of the MRZ document holder. |
| [`dateOfExpire`](#dateofexpire) | *string* | The expiry date of the MRZ document. |
| [`documentType`](#documenttype) | *string* | The type of MRTD that the MRZ document is. |
| [`documentNumber`](#documentnumber) | *string* | The MRZ document number. |
| [`age`](#age) | *number* | The age of the MRZ document holder. |
| [`mrzText`](#mrztext) | *string* | The raw unparsed text of the MRZ. |
| [`personalNumber`](#personalnumber) | *string* | The personal number field on the MRZ document. |
| [`optionalData1`](#optionaldata1) | *string* | The first optional data field on the MRZ document. |
| [`optionalData2`](#optionaldata2) | *string* | The second optional data field on the MRZ document. |

### firstName

Represents the first name of the MRZ document holder.

```ts
firstName?: string
```

### lastName

Represents the last name of the MRZ document holder.

```ts
lastName?: string
```

### sex

Represents the sex of the MRZ document holder.

```ts
sex?: string
```

### issuingState

Represents the issuing state of the MRZ document, expressed as the full name of the country/region.

```ts
issuingState?: string
```

### issuingStateRaw

Represents the raw ICAO issuing state code as it appears in the MRZ (e.g. `CAN`, `USA`), before conversion to a full country name.

```ts
issuingStateRaw?: string
```

### nationality

Represents the nationality of the MRZ document holder, expressed as the full name of the country/region.

```ts
nationality?: string
```

### nationalityRaw

Represents the raw ICAO nationality code of the MRZ document holder (e.g. `CAN`, `USA`), before conversion to a full country name.

```ts
nationalityRaw?: string
```

### dateOfBirth

Represents the date of birth of the MRZ document holder.

```ts
dateOfBirth?: string
```

### dateOfExpire

Represents the expiry date of the MRZ document.

```ts
dateOfExpire?: string
```

### documentType

Represents the type of MRZ document.

```ts
documentType?: string
```

### documentNumber

Represents the MRZ document number.

```ts
documentNumber?: string
```

### age

Represents the age of the MRZ document holder.

```ts
age?: number
```

### mrzText

Represents the raw text of the MRZ.

```ts
mrzText?: string
```

### personalNumber

Represents the personal number field on the MRZ document (where present). This field is encoded on TD1 and TD3 documents and is empty when the document does not carry a personal number.

```ts
personalNumber?: string
```

### optionalData1

Represents the first optional data field on the MRZ document. Usage varies by issuing authority.

```ts
optionalData1?: string
```

### optionalData2

Represents the second optional data field on the MRZ document. Usage varies by issuing authority.

```ts
optionalData2?: string
```
