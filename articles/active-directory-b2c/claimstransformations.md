---
title: ClaimsTransformations - Azure Active Directory B2C | Microsoft Docs
description: Definición del elemento ClaimsTransformations en el esquema del marco de experiencia de identidad de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 65b64563bf00bb519a65b6d2e0418b4f755dea2d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64710817"
---
# <a name="claimstransformations"></a>ClaimsTransformations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

El elemento **ClaimsTransformations** contiene una lista de funciones de transformación de notificaciones que puede usarse en los recorridos del usuario como parte de una [directiva personalizada](active-directory-b2c-overview-custom.md). Una transformación de notificaciones convierte una notificación determinada en otra. En la transformación de notificaciones, debe especificar el método de transformación, por ejemplo, agregando un elemento a una colección de cadenas o cambiando las mayúsculas y minúsculas de una cadena.

Para incluir la lista de funciones de transformación de notificaciones que se puede usar en los recorridos del usuario, se debe declarar un elemento XML ClaimsTransformations en la sección BuildingBlocks de la directiva.

```xml
<ClaimsTransformations>
  <ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
    ...
  </ClaimsTransformation>
</ClaimsTransformations>
```

El **ClaimsTransformation** elemento contiene los siguientes atributos:

| Atributo |Obligatorio | DESCRIPCIÓN |
| --------- |-------- | ----------- |
| Id |Sí | Un identificador que se usa para identificar de forma única la transformación de la notificación. Se hace referencia al identificador desde otros elementos XML de la directiva. |
| TransformationMethod | Sí | El método de transformación que se va a usar en la transformación de las reclamaciones. Cada transformación de notificación tiene sus propios valores. Consulte la [referencia de la transformación de notificaciones](#claims-transformations-reference) para obtener una lista completa de los valores disponibles. |

## <a name="claimstransformation"></a>ClaimsTransformation

El elemento **ClaimsTransformation** contiene los elementos siguientes:

```xml
<ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
  <InputClaims>
    ...
  </InputClaims>
  <InputParameters>
    ...
  </InputParameters>                
  <OutputClaims>
    ...
  </OutputClaims>
</ClaimsTransformation>
```


| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | -------- | ----------- |
| InputClaims | 0:1 | Una lista de elementos **InputClaim** que especifican los tipos de notificación que se toman como entrada en la transformación de notificaciones. Cada uno de esto elementos contiene una referencia a un ClaimType ya definido en la sección ClaimsSchema de la directiva. |
| InputParameters | 0:1 | Una lista de elementos **InputParameter** que se proporcionan como entrada para la transformación de notificaciones.  
| OutputClaims | 0:1 | Una lista de elementos **OutputClaim** que especifican los tipos de notificación que se producen después de que se invoque ClaimsTransformation. Cada uno de esto elementos contiene una referencia a un ClaimType ya definido en la sección ClaimsSchema. |

### <a name="inputclaims"></a>InputClaims

El elemento **InputClaims** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| InputClaim | 1:n | Un tipo de notificación de entrada esperado. |

#### <a name="inputclaim"></a>InputClaim

El elemento **InputClaim** contiene los atributos siguientes:

| Atributo |Obligatorio | DESCRIPCIÓN |
| --------- | ----------- | ----------- |
| ClaimTypeReferenceId |Sí | Una referencia a un ClaimType ya definido en la sección ClaimsSchema de la directiva. |
| TransformationClaimType |Sí | Un identificador para hacer referencia a un tipo de notificación de transformación. Cada transformación de notificación tiene sus propios valores. Consulte la [referencia de la transformación de notificaciones](#claims-transformations-reference) para obtener una lista completa de los valores disponibles. |

### <a name="inputparameters"></a>InputParameters

El elemento **InputParameters** contiene el siguiente elemento:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| InputParameter | 1:n | Un parámetro de entrada esperado. |

#### <a name="inputparameter"></a>InputParameter

| Atributo | Obligatorio |DESCRIPCIÓN |
| --------- | ----------- |----------- |
| Id | Sí | Un identificador que es una referencia a un parámetro del método de transformación de notificaciones. Cada método de transformación de notificaciones tiene sus propios valores. Consulte la tabla de la transformación de notificaciones para obtener una lista completa de los valores disponibles. |
| DataType | Sí | El tipo de datos del parámetro, como String, Boolean, Int o DateTime, según la enumeración DataType en el esquema XML de la directiva personalizada. Este tipo se usa para realizar operaciones aritméticas correctamente. Cada transformación de notificación tiene sus propios valores. Consulte la [referencia de la transformación de notificaciones](#claims-transformations-reference) para obtener una lista completa de los valores disponibles. |
| Valor | Sí | Un valor que se pasa de forma literal a la transformación. Algunos de los valores son arbitrarios, algunos de ellos se seleccionan desde el método de transformación de notificaciones. |

### <a name="outputclaims"></a>OutputClaims

El elemento **OutputClaims** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| OutputClaim | 0:n | Un tipo de notificación de salida esperado. |

#### <a name="outputclaim"></a>OutputClaim 

El elemento **OutputClaim** contiene los atributos siguientes:

| Atributo |Obligatorio | DESCRIPCIÓN |
| --------- | ----------- |----------- |
| ClaimTypeReferenceId | Sí | Una referencia a un ClaimType ya definido en la sección ClaimsSchema de la directiva.
| TransformationClaimType | Sí | Un identificador para hacer referencia a un tipo de notificación de transformación. Cada transformación de notificación tiene sus propios valores. Consulte la [referencia de la transformación de notificaciones](#claims-transformations-reference) para obtener una lista completa de los valores disponibles. |
 
Si la notificación de entrada y la notificación de salida son del mismo tipo (cadena o booleano), puede usar la misma notificación de entrada como de salida. En este caso, la transformación de notificaciones cambia la notificación de entrada por el valor de salida.

## <a name="example"></a>Ejemplo

Por ejemplo, puede almacenar la última versión de sus términos del servicio que el usuario aceptó. Al actualizar los términos de servicio, puede pedir al usuario que acepte la nueva versión. En el ejemplo siguiente, la transformación de notificaciones **HasTOSVersionChanged** compara el valor de la notificación **TOSVersion** con el valor de la notificación **LastTOSAcceptedVersion** y después devuelve la notificación booleana **TOSVersionChanged**.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="TOSVersionChanged">
      <DisplayName>Indicates if the TOS version accepted by the end user is equal to the current version</DisplayName>
      <DataType>boolean</DataType>
    </ClaimType>
    <ClaimType Id="TOSVersion">
      <DisplayName>TOS version</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    <ClaimType Id="LastTOSAcceptedVersion">
      <DisplayName>TOS version accepted by the end user</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <ClaimsTransformation Id="HasTOSVersionChanged" TransformationMethod="CompareClaims">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="TOSVersion" TransformationClaimType="inputClaim1" />
        <InputClaim ClaimTypeReferenceId="LastTOSAcceptedVersion" TransformationClaimType="inputClaim2" />
      </InputClaims>
      <InputParameters>
        <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
      </InputParameters>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="TOSVersionChanged" TransformationClaimType="outputClaim" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

## <a name="claims-transformations-reference"></a>Referencia a las transformaciones de notificaciones

Para obtener ejemplos de transformaciones de notificaciones, consulte las páginas de referencia siguientes:

- [Boolean](boolean-transformations.md)
- [Fecha](date-transformations.md)
- [Entero](integer-transformations.md)
- [JSON](json-transformations.md)
- [General](general-transformations.md)
- [Cuenta social](social-transformations.md)
- [String](string-transformations.md)
- [StringCollection](stringcollection-transformations.md)

