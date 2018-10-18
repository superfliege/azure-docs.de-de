---
title: Beispiele für die Transformation allgemeiner Ansprüche für das Schema des Frameworks für die Identitätsfunktion von Azure Active Directory B2C | Microsoft-Dokumentation
description: Hier finden Sie Beispiele für die Transformation allgemeiner Ansprüche für das Schema des Frameworks für die Identitätsfunktion von Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8ff418c24e9171d452bca873c4b8f66ada2adb7c
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47431325"
---
# <a name="general-claims-transformations"></a>Transformationen allgemeiner Ansprüche

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel werden Beispiele für die Verwendung von Transformationen allgemeiner Ansprüche für das Schema des Frameworks für die Identitätsfunktion in Azure Active Directory B2C veranschaulicht. Weitere Informationen finden Sie unter [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Überprüft, ob **inputClaim** vorhanden ist oder nicht, und legt **outputClaim** auf TRUE bzw. FALSE fest.

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | InputClaim |Beliebig | Der Eingabeanspruch, dessen Vorhandensein überprüft werden muss. |
| OutputClaim | outputClaim | boolean | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

Mithilfe dieser Anspruchstransformation können Sie überprüfen, ob ein Anspruch vorhanden ist oder einen beliebigen Wert enthält. Der Rückgabewert ist ein boolescher Wert, der angibt, ob der Anspruch vorhanden ist. Im folgenden Beispiel wird geprüft, ob die E-Mail-Adresse vorhanden ist.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim**: someone@contoso.com
- Ausgabeansprüche: 
    - **outputClaim**: TRUE

## <a name="hash"></a>Hash

Hash des angegebenen Nur-Texts, der Salt und einen geheimen Schlüssel verwendet.

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | plaintext | Zeichenfolge | Der zu verschlüsselnde Eingabeanspruch |
| InputClaim | Salt | Zeichenfolge | Der Salt-Parameter. Sie können mit der `CreateRandomString`-Anspruchstransformation einen Zufallswert erstellen. |
| InputParameter | randomizerSecret | Zeichenfolge | Verweist auf einen vorhandenen **Richtlinienschlüssel** von Azure AD B2C. So erstellen Sie einen Richtlinienschlüssel: Wählen Sie in Ihrem Azure AD B2C-Mandanten **B2C-Einstellungen > Framework für die Identitätsfunktion** aus. Wählen Sie **Richtlinienschlüssel** aus, um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen. Wählen Sie **Hinzufügen**. Wählen Sie unter **Optionen** den Eintrag **Manuell** aus. Geben Sie einen Namen an (das Präfix „B2C_1A_“ wird möglicherweise automatisch hinzugefügt). Geben Sie in das Feld „Geheimer Schlüssel“ einen geheimen Schlüssel ein, der verwendet werden soll, wie z.B. 1234567890. Wählen Sie für „Schlüsselverwendung“ die Option **Geheimer Schlüssel** aus. Klicken Sie auf **Erstellen**. |
| OutputClaim | hash | Zeichenfolge | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. Der im Eingabeanspruch `plaintext` konfigurierte Anspruch. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **plaintext**: MyPass@word1
    - **salt**: 487624568
    - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Ausgabeansprüche: 
    - **outputClaim**: CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=



