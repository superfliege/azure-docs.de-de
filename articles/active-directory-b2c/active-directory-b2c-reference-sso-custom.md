---
title: "SSO-Sitzungsverwaltung mit benutzerdefinierten Richtlinien – Azure AD B2C | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie SSO-Sitzungen mithilfe benutzerdefinierter Richtlinien in Azure AD B2C verwalten.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 809f6000-2e52-43e4-995d-089d85747e1f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: parja
ms.openlocfilehash: ff767993eaf0305168176d0ad6e15c068b8c85eb
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2017
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a>Azure AD B2C: Einmaliges Anmelden (Single Sign-On, SSO) für Sitzungsverwaltung

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Mithilfe von Azure AD B2C kann ein Administrator steuern, wie Azure AD B2C mit einem Benutzer interagiert, nachdem der Benutzer bereits authentifiziert wurde. Dies erfolgt über die SSO-Sitzungsverwaltung. Der Administrator kann z. B. steuern, ob die Auswahl von Identitätsanbietern angezeigt wird, oder ob lokale Kontodetails erneut eingegeben werden müssen. In diesem Artikel wird das Konfigurieren der Einstellungen für einmaliges Anmelden für Azure AD B2C beschrieben.

## <a name="overview"></a>Übersicht

Die SSO-Sitzungsverwaltung besteht aus zwei Teilen. Der erste Teil befasst sich mit den direkten Benutzerinteraktionen mit Azure AD B2C und der zweite Teil behandelt Benutzerinteraktionen mit externen Parteien, z. B. mit Facebook. Azure AD B2C setzt SSO-Sitzungen nicht außer Kraft oder umgeht diese, die von externen Parteien abgehalten werden. Stattdessen wird die Route über Azure AD B2C zur externen Partei „gespeichert“, wodurch vermieden wird, dass der Benutzer erneut aufgefordert werden muss, seinen Identitätsanbieter auszuwählen. Die ultimative SSO-Entscheidung verbleibt der externen Partei.

## <a name="how-does-it-work"></a>Funktionsweise

Die SSO-Sitzungsverwaltung verwendet dieselbe Semantik wie jedes andere technische Profil in benutzerdefinierten Richtlinien. Wenn ein Orchestrierungsschritt ausgeführt wird, wird das technische Profil, das dem Schritt zugeordnet ist, nach einem `UseTechnicalProfileForSessionManagement`-Verweis abgefragt. Falls vorhanden, wird der referenzierte SSO-Sitzungsanbieter überprüft, um festzustellen, ob der Benutzer ein Sitzungsteilnehmer ist. In diesem Fall wird der SSO-Sitzungsanbieter dazu verwendet, um die Sitzung wieder aufzufüllen. Wenn die Ausführung eines Orchestrierungsschritts abgeschlossen ist, wird entsprechend der Anbieter verwendet, um Informationen in der Sitzung zu speichern, wenn ein SSO-Sitzungsanbieter angegeben wurde.

Azure AD B2C hat eine Reihe von SSO-Sitzungsanbietern definiert, die verwendet werden können:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

SSO Verwaltungsklassen werden mithilfe des `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />`-Elements eines technischen Profils angegeben.

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Wie der Name besagt, ist dieser Anbieter untätig. Dieser Anbieter kann zum Unterdrücken des SSO-Verhaltens für ein bestimmtes technisches Profil verwendet werden.

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Dieser Anbieter kann zum Speichern von Ansprüchen in einer Sitzung verwendet werden. Auf diesen Anbieter wird in der Regel in einem technischen Profil verwiesen, über das lokale Konten verwaltet werden. 

```XML
<TechnicalProfile Id="SM-AAD">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="newUser" />
        <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
    </PersistedClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true" />
    </OutputClaims>
</TechnicalProfile>
```

Verwenden Sie das `<PersistedClaims>`-Element des technischen Profils, um Ansprüche in der Sitzung hinzuzufügen. Wenn der Anbieter dazu verwendet wird, um die Sitzung erneut aufzufüllen, werden die bestehen gebliebenen Ansprüche zum Anspruchsbehälter hinzugefügt. `<OutputClaims>` dient zum Abrufen von Ansprüchen aus der Sitzung.

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Dieser Anbieter wird dazu verwendet, um den Bildschirm „Identitätsanbieter auswählen“ zu unterdrücken. Er wird in der Regel in einem technischen Profil für einen externen Identitätsanbieter referenziert, z. B. Facebook. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Dieser Anbieter wird zum Verwalten der Azure AD B2C SAML-Sitzungen zwischen Apps sowie zwischen externen SAML-Identitätsanbietern verwendet.

```XML
<TechnicalProfile Id="SM-Reflector-SAML">
    <DisplayName>Session Management Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="IncludeSessionIndex">false</Item>
        <Item Key="RegisterServiceProviders">false</Item>
    </Metadata>
</TechnicalProfile>
```

Es gibt zwei Metadatenelemente im technischen Profil:

| Element | Standardwert | Mögliche Werte | Beschreibung
| --- | --- | --- | --- |
| IncludeSessionIndex | true | True/False | Gibt dem Anbieter an, dass der Sitzungsindex gespeichert werden soll. |
| RegisterServiceProviders | true | True/False | Gibt an, dass der Anbieter alle SAML-Dienstanbieter registrieren soll, die eine Assertion ausgestellt haben. |

Wenn der Anbieter zum Speichern einer SAML-Identitätsanbietersitzung verwendet wird, müssen die beiden vorhergehenden Elemente „false“ sein. Wenn der Anbieter zum Speichern der B2C SAML-Sitzungen verwendet wird, müssen die obigen Elemente „true“ sein oder ausgelassen werden, da die Standardwerte „true“ sind.

>[!NOTE]
> Für die Abmeldung von der SAML-Sitzung ist erforderlich, dass `SessionIndex` und `NameID` abgeschlossen sind.

## <a name="next-steps"></a>Nächste Schritte

Wir freuen uns über Feedback und Vorschläge! Wenn Sie Probleme mit diesem Thema haben, posten Sie mit dem Tag [„azure-ad-b2c“](https://stackoverflow.com/questions/tagged/azure-ad-b2c) einen Beitrag auf Stack Overflow. Für Featureanforderungen können Sie in unserem [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) abstimmen.

