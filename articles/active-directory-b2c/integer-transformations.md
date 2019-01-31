---
title: Beispiele für die Transformation von Integer-Ansprüchen für das Schema des Frameworks für die Identitätsfunktion von Azure Active Directory B2C | Microsoft-Dokumentation
description: Hier finden Sie Beispiele für die Transformation von Integer-Ansprüchen für das Schema des Frameworks für die Identitätsfunktion von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 358ee07b8fd32edded084d406e490cae9f557fdd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159505"
---
# <a name="integer-claims-transformations"></a>Transformationen von Integer-Ansprüchen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dieser Artikel enthält Beispiele für die Verwendung von Integer-Anspruchstransformationen des Schemas des Frameworks für die Identitätsfunktion von Azure Active Directory (Azure AD) B2C. Weitere Informationen finden Sie unter [Anspruchstransformationen](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim 

Konvertiert einen long-Datentyp in einen string-Datentyp.

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | lang | Der Anspruchstyp, der in eine Zeichenfolge konvertiert werden soll. |
| OutputClaim | outputClaim | Zeichenfolge | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

In diesem Beispiel wird der Anspruch `numericUserId` mit dem Werttyp long in einen `UserId`-Anspruch mit dem Werttyp string konvertiert.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim:** 12334 (long)
- Ausgabeansprüche: 
    - **outputClaim:** 12334 (string)

