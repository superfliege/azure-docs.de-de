---
title: Verwalten des Benutzerzugriffs in Azure Active Directory B2C | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Minderjährige identifizieren, das Geburtsdatum und das Land erfassen und in Ihrer Anwendung per Azure AD B2C die Zustimmung zu den Nutzungsbedingungen einholen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 05/04/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 52cf34d56f87d2543fb272ce99dbe011bc0ea037
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34711126"
---
# <a name="manage-user-access-in-azure-ad-b2c"></a>Verwalten des Benutzerzugriffs in Azure AD B2C

Dieser Artikel enthält Informationen dazu, wie Sie den Benutzerzugriff auf Ihre Anwendungen mit Azure Active Directory (AD) B2C verwalten können. Die Zugriffsverwaltung in Ihrer Anwendung umfasst Folgendes:

- Identifizieren von Minderjährigen und Steuern des Zugriffs auf die Nutzung Ihrer Anwendung
- Erzwingen der Zustimmung durch die Eltern, wenn Minderjährige Ihre Anwendungen nutzen möchten
- Erfassen des Geburtsdatums und Lands von Benutzern
- Einholen der Zustimmung zu den Nutzungsbedingungen und Durchführen der Alterskontrolle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

>[!Note] 
>Dieser Artikel enthält Informationen, die Sie verwenden können, um Verpflichtungen aufgrund der Datenschutz-Grundverordnung zu erfüllen. Allgemeine Informationen zur Datenschutz-Grundverordnung finden Sie im [Service Trust Portal im Abschnitt zur DSGVO](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="control-minor-access"></a>Steuern des Zugriffs durch Minderjährige

Für Anwendungen und Organisationen kann die Entscheidung getroffen werden, für Minderjährige die Nutzung von Anwendungen und Diensten, die nicht für diese Zielgruppe bestimmt sind, zu sperren. Alternativ hierzu kann für Anwendungen und Organisationen auch entschieden werden, Minderjährige zuzulassen und dann die Erteilung der Zustimmung durch die Eltern zu verwalten und zulässige Benutzeroberflächen bereitzustellen, die gemäß den Geschäftsregeln und den Regulierungsbestimmungen zulässig sind. 

Wenn ein Benutzer als minderjährig identifiziert wird, kann für den Benutzerflow in Azure AD B2C eine von drei Optionen gewählt werden:

- **Send a signed JWT id_token back to the application** (Signiertes JWT-ID-Token an die Anwendung zurücksenden): Der Benutzer wird im Verzeichnis registriert, und ein Token wird an die Anwendung zurückgegeben. Die Anwendung wird dann basierend auf Geschäftsregeln fortgesetzt. Beispielsweise kann die Anwendung mit einem Prozess fortfahren, bei dem die Zustimmung der Eltern eingeholt wird. Hierzu geben Sie an, dass Sie die Ansprüche **ageGroup** und **consentProvidedForMinor** von der Anwendung erhalten möchten.
- **Send an unsigned JSON token to the application** (Nicht signiertes JSON-Token an die Anwendung senden): Azure AD B2C benachrichtigt die Anwendung darüber, dass der Benutzer minderjährig ist, und stellt den Zustimmungsstatus der Eltern für den Benutzer bereit. Die Anwendung wird dann basierend auf Geschäftsregeln fortgesetzt. Ein JSON-Token führt keine erfolgreiche Authentifizierung bei der Anwendung durch. Die Anwendung muss den nicht authentifizierten Benutzer gemäß den Ansprüchen verarbeiten, die im JSON-Token enthalten sind, z.B. **name**, **email**, **ageGroup** und **consentProvidedForMinor**.
- **Block the user** (Benutzer blockieren): Dies trifft zu, wenn der Benutzer minderjährig ist und die Zustimmung der Eltern nicht erteilt wurde.  Azure AD B2C kann dem Benutzer dann einen Hinweis anzeigen, dass der Zugriff blockiert ist.  Es wird kein Token ausgestellt, der Zugriff wird blockiert, und das Benutzerkonto wird während des Registrierungsvorgangs nicht erstellt. Zur Implementierung stellen Sie eine geeignete HTML/CSS-Inhaltsseite bereit, um den Benutzer zu informieren und die entsprechenden Optionen anzuzeigen. Für die Anwendung sind bei neuen Registrierungen keine weiteren Aktionen erforderlich.

## <a name="get-parental-consent"></a>Einholen der Zustimmung der Eltern

Je nach Regulierung der Anwendung muss unter Umständen die Zustimmung durch einen Benutzer erteilt werden, der als erwachsene Person verifiziert wurde.  Azure AD B2C enthält keine Benutzeroberfläche zur Verifizierung des Alters einer Person und zur Erteilung der Zustimmung durch eine verifizierte erwachsene Person für einen minderjährigen Benutzer.  Diese Benutzeroberfläche muss von der Anwendung oder einem anderen Dienstanbieter bereitgestellt werden.

Hier ist ein Beispiel für einen Benutzerflow zum Einholen der elterlichen Zustimmung angegeben:

1. Bei einem Vorgang der [Azure Active Directory-Graph-API](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog) wird der Benutzer als minderjährig identifiziert, und die Benutzerdaten werden in Form eines nicht signierten JSON-Tokens an die Anwendung zurückgegeben.
2. Die Anwendung verarbeitet das JSON-Token und zeigt einen Hinweis für die minderjährige Person an, dass die Zustimmung der Eltern erforderlich ist. Diese Zustimmung wird dann online angefordert. 
3. In Azure AD B2C wird ein Anmeldeprozess angezeigt, über den sich der Benutzer auf normale Weise anmelden kann, und es wird ein Token für die Anwendung ausgegeben, das **legalAgeGroupClassification = “minorWithParentalConsent”** enthält. Die Anwendung erfasst die E-Mail-Adresse eines Elternteils und verifiziert, dass es sich bei dieser Person um einen Erwachsenen handelt, indem eine vertrauenswürdige Quelle genutzt wird, z.B. Einwohnermeldeamt-Daten, Lizenzverifizierung oder Kreditkartennachweis. Wenn dieser Vorgang erfolgreich ist, wird die minderjährige Person von der Anwendung aufgefordert, sich über den Azure AD B2C-Benutzerflow anzumelden. Falls die Zustimmung verweigert wurde (z.B. **legalAgeGroupClassification = “minorWithoutParentalConsent”**), gibt Azure AD B2C ein JSON-Token (keine Anmeldedaten) an die Anwendung zurück, um den Zustimmungsprozess neu zu starten. Der Benutzerflow kann optional angepasst werden, damit eine minderjährige oder erwachsene Person Zugriff auf das Konto eines Minderjährigen erlangen kann. Hierzu wird ein Registrierungscode an die hinterlegte E-Mail-Adresse des Minderjährigen oder des Erwachsenen gesendet.
4. Die Anwendung zeigt für die minderjährige Person eine Option an, mit der die Zustimmung widerrufen werden kann.
5. Wenn die minderjährige oder die erwachsene Person die Zustimmung widerruft, kann die Azure AD-Graph-API verwendet werden, um **consetProvidedForMinor** in **denied** zu ändern. Alternativ hierzu kann für die Anwendung auch die Entscheidung getroffen werden, eine minderjährige Person zu löschen, für die die Zustimmung widerrufen wurde. Optional ist es möglich, den Benutzerflow so anzupassen, dass die authentifizierte minderjährige Person (oder ein Elternteil, der das Konto der minderjährigen Person verwendet) die Zustimmung widerrufen kann. Azure AD B2C zeichnet **consentProvidedForMinor** als **denied** auf.

Weitere Informationen zu **legalAgeGroupClassification**, **consentProvidedForMinor** und **ageGroup** finden Sie unter [User Resource Type](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/user) (Benutzerressourcentyp). Weitere Informationen zu benutzerdefinierten Attributen finden Sie unter [Verwenden benutzerdefinierter Attribute zum Erfassen von Informationen über Ihre Kunden](active-directory-b2c-reference-custom-attr.md). Bei der Adressierung von erweiterten Attributen per Azure AD-Graph-API muss die lange Version des Attributs verwendet werden, z.B. extension_18b70cf9bb834edd8f38521c2583cd86_dateOfBirth": "2011-01-01T00:00:00Z".

## <a name="gather-date-of-birth-and-country-data"></a>Erfassen des Geburtsdatums und des Lands

Anwendungen können Azure AD B2C nutzen, um während der Registrierung das Geburtsdatum und das Land für alle Benutzer zu erfassen. Falls das Geburtsdatum und die Daten zum Land nicht bereits vorhanden sind, können sie vom Benutzer während des nächsten Authentifizierungsprozesses (Anmeldung) abgefragt werden. Benutzer können den Vorgang nicht fortsetzen, ohne das Geburtsdatum und das Land anzugeben. Anhand des angegebenen Lands und Geburtsdatums ermittelt Azure AD B2C, ob die Person gemäß den Bestimmungen des jeweiligen Lands als minderjährig gilt. 

Mit einem angepassten Benutzerflow können das Geburtsdatum und das Land erfasst werden, und die Transformation von Azure AD B2C-Ansprüchen kann genutzt werden, um die Altersgruppe (**ageGroup**) zu ermitteln und das Ergebnis im Verzeichnis zu speichern (oder das Land und Geburtsdatum direkt zu speichern).

In den folgenden Schritten wird die Logik veranschaulicht, mit der aus dem Geburtsdatum die Altersgruppe (**ageGroup**) berechnet wird:

1. Es wird versucht, das Land anhand des Landescodes in der Liste zu ermitteln. Wenn dieser Vorgang nicht erfolgreich ist, wird **Default** verwendet.
2. Bei Vorhandensein des Knotens **MinorConsent** im Landeselement:  <br>a. Es wird das Mindestdatum berechnet, an dem der Benutzer geboren sein muss, um als erwachsene Person zu gelten. Beispiel: Das Geburtsdatum ist der 14.03.2015, und für **MinorConsent** gilt der Wert 18. Das Mindestdatum für die Geburt ist der 14.03.2000.
    <br>b. Das Mindestdatum für die Geburt wird mit dem tatsächlichen Geburtsdatum verglichen. Falls das Mindestdatum für die Geburt vor dem Geburtstag des Benutzers liegt, wird für die Berechnung der Altersgruppe das Ergebnis **Minor** zurückgegeben.
3. Falls der Knoten **MinorNoConsentRequired** im Landeselement enthalten ist, können Sie die Schritte 2a und 2b wiederholen, indem Sie den Wert aus **MinorNoConsentRequired** verwenden. Die Ausgabe von 2b gibt **MinorNoConsentRequired** zurück, falls das Mindestdatum für die Geburt vor dem Geburtsdatum des Benutzers liegt. 
4. Wenn für keine Berechnung „true“ zurückgegeben wurde, wird für die Berechnung **Adult** zurückgegeben.

Falls für eine Anwendung mit anderen Methoden auf zuverlässige Weise Daten zum Geburtsdatum oder Land erfasst wurden, kann die Anwendung die GRAPH-API nutzen, um den Benutzerdatensatz mit diesen Informationen zu aktualisieren. Beispiel: 

- Wenn für einen Benutzer bekannt ist, dass es sich um eine erwachsene Person handelt, wird das Verzeichnisattribut **ageGroup** auf den Wert **Adult** aktualisiert.
- Falls ein Benutzer sicher als minderjährig eingestuft wird, wird das Verzeichnisattribut **ageGroup** auf den Wert **Minor** aktualisiert und **consentProvidedForMinor** entsprechend festgelegt.

Weitere Informationen zum Erfassen des Geburtsdatums finden Sie unter [Verwenden einer Altersbeschränkung in Active Directory B2C](basic-age-gating.md).

## <a name="capture-terms-of-use-agreement"></a>Erfassen der Zustimmung zu den Nutzungsbedingungen

Beim Entwickeln Ihrer Anwendung erfassen Sie die Benutzerzustimmung zu den Nutzungsbedingungen normalerweise innerhalb der Anwendung, ohne dass das Benutzerverzeichnis in größerem Maße in diesen Vorgang einbezogen wird.  Es kann aber ein Azure AD B2C-Benutzerflow verwendet werden, um die Zustimmung zu den Nutzungsbedingungen einzuholen, den Zugriff bei nicht erteilter Zustimmung zu beschränken und die Zustimmung zu zukünftigen Änderungen der Nutzungsbedingungen basierend auf dem Datum der letzten Zustimmung und dem Datum der letzten Version der Nutzungsbedingungen durchzusetzen.

Die **Nutzungsbedingungen** können auch die „Zustimmung zur Weitergabe von Daten an Dritte“ umfassen.  Die Erteilung der Zustimmung zu diesen Bedingungen durch einen Benutzer kann technisch zur gleichen Zeit eingeholt werden, oder der Benutzer hat die Möglichkeit, nur einer Bedingung zuzustimmen. Dies richtet sich jeweils nach den Bestimmungen und Geschäftsregeln vor Ort.

In den folgenden Schritten werden die Funktionen zum Verwalten der Nutzungsbedingungen beschrieben:

1. Aufzeichnen der Zustimmung zu den Nutzungsbedingungen und des Zustimmungsdatums über die Graph-API und erweiterte Attribute. Dies kann sowohl über integrierte als auch über benutzerdefinierte Benutzerflows erfolgen. Es wird empfohlen, die Attribute **extension_termsOfUseConsentDateTime** und **extension_termsOfUseConsentVersion** zu erstellen und zu verwenden.
2. Erstellen Sie ein erforderliches Kontrollkästchen mit dem Titel „Nutzungsbedingungen akzeptieren“, und zeichnen Sie das Ergebnis während der Registrierung auf. Dies kann sowohl über integrierte als auch über benutzerdefinierte Benutzerflows erfolgen.
3. In Azure AD B2C werden die Nutzungsbedingungen und die Erteilung der Zustimmung gespeichert. Die Graph-API kann verwendet werden, um den Status eines Benutzers abzufragen, indem das Erweiterungsattribut gelesen wird, das zum Aufzeichnen der Antwort verwendet wurde, z.B. **termsOfUseTestUpdateDateTime**. Dies kann sowohl über integrierte als auch über benutzerdefinierte Benutzerflows erfolgen.
4. Erzwingen der Zustimmung zu aktualisierten Nutzungsbedingungen, indem das Datum der Zustimmung mit dem Datum der letzten Version der Nutzungsbedingungen verglichen wird. Dies kann nur über einen benutzerdefinierten Benutzerflow erfolgen. Verwenden Sie das erweiterte Attribut **extension_termsOfUseConsentDateTime**, und vergleichen Sie den Wert mit dem Anspruch von **termsOfUseTextUpdateDateTime**. Erzwingen Sie einen neuen Zustimmungsbildschirm zur Selbstbestätigung, falls die Zustimmung veraltet ist, und blockieren Sie den Zugriff andernfalls per Richtlinienlogik.
5. Erzwingen der Zustimmung zu aktualisierten Nutzungsbedingungen, indem die Versionsnummer der Zustimmung mit der zuletzt akzeptierten Versionsnummer verglichen wird. Dies kann nur über einen benutzerdefinierten Benutzerflow erfolgen. Verwenden Sie das erweiterte Attribut **extension_termsOfUseConsentDateTime**, und vergleichen Sie den Wert mit dem Anspruch von **extension_termsOfUseConsentVersion**. Erzwingen Sie einen neuen Zustimmungsbildschirm zur Selbstbestätigung, falls die Zustimmung veraltet ist, und blockieren Sie den Zugriff andernfalls per Richtlinienlogik.

Die Erfassung der Zustimmung zu den Nutzungsbedingungen kann dem Benutzer in den folgenden Szenarien präsentiert werden:

- Ein neuer Benutzer registriert sich. Die Nutzungsbedingungen werden angezeigt, und das Ergebnis der Zustimmung wird gespeichert.
- Ein Benutzer meldet sich an und hat bereits seine Zustimmung zu den neuesten bzw. aktiven Nutzungsbedingungen gegeben. Die Nutzungsbedingungen werden nicht angezeigt.
- Ein Benutzer meldet sich an und hat noch nicht seine Zustimmung zu den neuesten bzw. aktiven Nutzungsbedingungen gegeben. Die Nutzungsbedingungen werden angezeigt, und das Ergebnis der Zustimmung wird gespeichert.
- Ein Benutzer meldet sich an und hat bereits seine Zustimmung zu älteren Nutzungsbedingungen gegeben, die jetzt auf eine neuere Version aktualisiert werden. Die Nutzungsbedingungen werden angezeigt, und das Ergebnis der Zustimmung wird gespeichert.

In der folgenden Abbildung ist der empfohlene Benutzerflow dargestellt:

![Benutzerflow für Zustimmung](./media/manage-user-access/user-flow.png) 

Hier ist ein Beispiel für die Zustimmung zu DateTime-basierten Nutzungsbedingungen in einem Anspruch angegeben:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentDateTime" TransformationMethod="GetCurrentDateTime">
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="currentDateTime" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequired" TransformationMethod="IsTermsOfUseConsentRequired">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="termsOfUseConsentDateTime" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="termsOfUseTextUpdateDateTime" DataType="dateTime" Value="2098-01-30T23:03:45" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="result" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Hier ist ein Beispiel für die Zustimmung zu versionsbasierten Nutzungsbedingungen in einem Anspruch angegeben:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetEmptyTermsOfUseConsentVersionForNewUser" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value=""/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentVersion" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value="V1"/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="compareTo" DataType="string" Value="V1" />
      <InputParameter Id="operator" DataType="string" Value="not equal" />
      <InputParameter Id="ignoreCase" DataType="string" Value="true" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations> 
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich unter [Verwalten von Benutzerdaten in Azure AD B2C](manage-user-data.md) über das Löschen und Exportieren von Benutzerdaten.
