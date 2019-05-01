---
title: 'Azure Active Directory B2C: ClaimsProviders | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie das ClaimsProvider-Element einer benutzerdefinierten Richtlinie in Azure Active Directory B2C angeben.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: ababd7e9f1de33eb8679e583c2db18d2992cfb1c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64699623"
---
# <a name="claimsproviders"></a>ClaimsProviders

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ein Anspruchsanbieter enthält eine Gruppe von [technischen Profilen](technicalprofiles.md). Jeder Anspruchsanbieter muss über mindestens ein technisches Profil verfügen, mit dem die Endpunkte und die Protokolle bestimmt werden, die für die Kommunikation mit diesem Anspruchsanbieter erforderlich sind. Ein Anspruchsanbieter kann über mehrere technische Profile verfügen. Beispielsweise können mehrere technische Profile definiert werden, weil der Anspruchsanbieter mehrere Protokolle bzw. verschiedene Endpunkte mit unterschiedlichen Funktionen unterstützt oder unterschiedliche Ansprüche mit verschiedenen Vertrauensgraden freigibt. Es kann akzeptabel sein, in einer User Journey sensible Ansprüche freizugeben, während dies für eine andere nicht der Fall ist.

```XML
<ClaimsProviders>
  <ClaimsProvider>
    <Domain>Domain name</Domain>
    <DisplayName>Display name</DisplayName>
    <TechnicalProfiles>
      </TechnicalProfile>
        ...
      </TechnicalProfile>
        ...
    </TechnicalProfiles>
  </ClaimsProvider>
  ...
</ClaimsProviders>
```

Das **ClaimsProviders**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| ClaimsProvider | 1:n | Eine anerkannter Anspruchsanbieter, der in verschiedenen User Journeys genutzt werden kann. |

## <a name="claimsprovider"></a>ClaimsProvider

Das **ClaimsProvider**-Element enthält die folgenden untergeordneten Elemente:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ---------- | ----------- |
| Domäne | 0:1 | Eine Zeichenfolge, die den Domänennamen für den Anspruchsanbieter enthält. Wenn Ihr Anspruchsanbieter beispielsweise das technische Profil von Facebook enthält, lautet der Domänenname „Facebook.com“. Dieser Domänenname wird für alle technischen Profile verwendet, die im Anspruchsanbieter definiert sind, es sei denn, er wird vom technischen Profil außer Kraft gesetzt. Auf den Domänennamen kann auch in einem **domain_hint** verwiesen werden. Weitere Informationen finden Sie im Abschnitt **Umleiten der Anmeldung zu einem Anbieter sozialer Netzwerke** unter [Einrichten der direkten Anmeldung mit Azure Active Directory B2C](direct-signin.md). |
| DisplayName | 0:1 | Eine Zeichenfolge, die den Namen des Anspruchsanbieters enthält, die Benutzern angezeigt werden kann. |
| [TechnicalProfiles](technicalprofiles.md) | 0:1 | Eine Gruppe von technischen Profilen, die vom Anspruchsanbieter unterstützt werden. |

**ClaimsProvider** organisiert die Beziehung zwischen Ihren technischen Profilen und dem Anspruchsanbieter. Das folgende Beispiel zeigt den Azure Active Directory-Anspruchsanbieter mit den technischen Azure Active Directory-Profilen:

```XML
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AAD-Common">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      ...
    </TechnicalProfile>
      ...
    <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWritePhoneNumberUsingObjectId">
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

Das folgende Beispiel zeigt den Facebook-Anspruchsanbieter mit dem technischen Profil **Facebook-OAUTH**.

```XML
<ClaimsProvider>
  <Domain>facebook.com</Domain>
  <DisplayName>Facebook</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Facebook-OAUTH">
      <DisplayName>Facebook</DisplayName>
      <Protocol Name="OAuth2" />
        ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
