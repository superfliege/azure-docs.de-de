---
title: Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen in Azure AD | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Ansprüche im SAML-Token für Unternehmensanwendungen in Azure AD anpassen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2018
ms.author: celested
ms.reviewer: luleon, jeedes
ms.custom: aaddev
ms.openlocfilehash: affdad71c9c97f13c015b35f1c67ee79f2473442
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075209"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Gewusst wie: Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen

Derzeit unterstützt Azure Active Directory (Azure AD) einmaliges Anmelden für die meisten Unternehmensanwendungen, einschließlich bereits im Azure AD-App-Katalog integrierter Anwendungen sowie benutzerdefinierter Anwendungen. Wenn sich ein Benutzer mithilfe des SAML 2.0-Protokolls über Azure AD bei einer Anwendung authentifiziert, sendet Azure AD ein Token an die Anwendung (über eine HTTP POST-Anfrage). Die Anwendung überprüft und verwendet dann das Token, um den Benutzer anzumelden, anstatt den Benutzernamen und das Kennwort anzufordern. Diese SAML-Token enthalten Informationen über den Benutzer, die als „Ansprüche“ bezeichnet werden.

Ein *Anspruch* (Claim) bezeichnet Informationen, die ein Identitätsanbieter über einen Benutzer in dem für diesen Benutzer ausgestellten Token angibt. Im [SAML-Token](https://en.wikipedia.org/wiki/SAML_2.0) sind diese Daten in der Regel in der SAML-Attributanweisung enthalten. Die eindeutige ID des Benutzers wird normalerweise im SAML-Betreff dargestellt und auch als Namensbezeichner bezeichnet.

Standardmäßig stellt Azure AD der Anwendung ein SAML-Token aus, das einen NameIdentifier-Anspruch enthält, dessen Wert dem Benutzernamen (also dem Benutzerprinzipalnamen) in Azure AD entspricht. Über diesen Wert kann der Benutzer eindeutig identifiziert werden. Das SAML-Token enthält auch zusätzliche Ansprüche, die E-Mail-Adresse des Benutzers, Vorname und Nachname enthält.

Um die im SAML-Token für die Anwendung ausgestellten Ansprüche anzuzeigen oder zu bearbeiten, öffnen Sie die Anwendung im Azure-Portal. Aktivieren Sie anschließend im Abschnitt **Benutzerattribute** der Anwendung das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**.

![Abschnitt „Benutzerattribute“][1]

Es gibt zwei Gründe dafür, dass Sie die im SAML-Token ausgegebenen Ansprüche möglicherweise bearbeiten müssen:
* Die Anwendung wurde so geschrieben, dass sie einen anderen Satz an Anspruchs-URIs oder Anspruchswerten erfordert.
* Die Anwendung wurde auf eine Weise bereitgestellt, die erfordert, dass es sich beim NameIdentifier-Anspruch nicht um den in Azure AD gespeicherten Benutzernamen (also Benutzerprinzipalnamen) handelt.

Sie können beliebige Standardwerte eines Anspruchs bearbeiten. Wählen Sie die Anspruchszeile in der Tabelle mit den SAML-Tokenattributen. Daraufhin wird der Abschnitt **Attribut bearbeiten** geöffnet, und Sie können den Anspruchsnamen und -wert sowie den dem Anspruch zugeordneten Namespace bearbeiten.

![Benutzerattribut bearbeiten][2]

Sie können Ansprüche (mit Ausnahme des NameIdentifier-Anspruchs) über das Kontextmenü auch entfernen. Klicken Sie zum Öffnen dieses Menüs auf das Symbol **...**. Mithilfe der Schaltfläche **Attribut hinzufügen** können Sie darüber hinaus neue Ansprüche hinzufügen.

![Benutzerattribut bearbeiten][3]

## <a name="editing-the-nameidentifier-claim"></a>Bearbeiten des NameIdentifier-Anspruchs

Falls die Anwendung mit einem anderen Benutzernamen bereitgestellt wurde, klicken Sie im Abschnitt **Benutzerattribute** auf die Dropdownliste **Benutzer-ID**. Dadurch wird ein Dialogfeld mit mehreren Optionen angezeigt:

![Benutzerattribut bearbeiten][4]

### <a name="attributes"></a>Attribute

Wählen Sie die gewünschte Quelle für den Anspruch `NameIdentifier` (oder NameID) aus. Sie können aus folgenden Optionen auswählen:

| NAME | BESCHREIBUNG |
|------|-------------|
| E-Mail | Die E-Mail-Adresse des Benutzers. |
| userprincipalName | Der Benutzerprinzipalname (UPN) des Benutzers. |
| onpremisessamaccount | Der SAM-Kontoname der aus der lokalen Azure AD-Instanz synchronisiert wurde. |
| objectID | Die objectID des Benutzers in Azure AD. |
| EmployeeID | Die EmployeeID des Benutzers. |
| Verzeichniserweiterungen | Verzeichniserweiterungen, die [über die Azure AD Connect-Synchronisierung aus dem lokalen Active Directory synchronisiert wurden](../hybrid/how-to-connect-sync-feature-directory-extensions.md). |
| Erweiterungsattribute 1–15 | Die lokalen Erweiterungsattribute, die zur Erweiterung des Azure AD-Schemas verwendet werden. |

### <a name="transformations"></a>Transformationen

Sie können auch die speziellen Funktionen für Anspruchstransformationen verwenden.

| Funktion | BESCHREIBUNG |
|----------|-------------|
| **ExtractMailPrefix()** | Entfernt das Domänensuffix aus der E-Mail-Adresse, dem SAM-Kontonamen oder dem Benutzerprinzipalnamen. Dadurch wird nur der erste Teil des Benutzernamens übergeben (z.B. „joe_smith“ anstelle von joe_smith@contoso.com). |
| **join()** | Verknüpft ein Attribut mit einer überprüften Domäne. Wenn der ausgewählte Wert für die Benutzer-ID über eine Domäne verfügt, wird der Benutzername extrahiert, an den ausgewählte überprüfte Domäne angefügt werden soll. Wenn Sie z.B. die E-Mail-Adresse (joe_smith@contoso.com) als Wert für die Benutzer-ID und „contoso.onmicrosoft.com“ als überprüfte Domäne verwenden, erhalten Sie joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Konvertiert die Zeichen des ausgewählten Attributs in Kleinbuchstaben. |
| **ToUpper()** | Konvertiert die Zeichen des ausgewählten Attributs in Großbuchstaben. |

## <a name="adding-claims"></a>Hinzufügen von Ansprüchen

Wenn Sie einen Anspruch hinzufügen, können Sie den Attributnamen angeben (der nicht unbedingt wie bei der SAML-Spezifikation einem URI-Muster entsprechen muss). Legen Sie den Wert auf ein beliebiges Benutzerattribut fest, das im Verzeichnis gespeichert ist, oder verwenden Sie einen konstanten Wert als statischen Einstiegspunkt für alle Benutzer in Ihrer Organisation.

![Benutzerattribut hinzufügen][7]

Sie können beispielsweise die Abteilung des Benutzers in seiner Organisation als Anspruch übermitteln (z.B. Sales). Geben Sie den Anspruchsnamen ein, wie er von der Anwendung erwartet wird, und wählen Sie als Wert **user.department** aus.

> [!NOTE]
> Wenn für einen bestimmten Benutzer kein Wert für ein ausgewähltes Attribut gespeichert ist, wird dieser Anspruch nicht in dem Token ausgestellt.

> [!TIP]
> Die Optionen **user.onpremisesecurityidentifier** und **user.onpremisesamaccountname** werden nur beim Synchronisieren von Benutzerdaten aus der lokalen Active Directory-Instanz mithilfe des Tools [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) unterstützt.

## <a name="restricted-claims"></a>Eingeschränkte Ansprüche

In SAML gibt es einige eingeschränkte Ansprüche. Wenn Sie diese Ansprüche hinzufügen, werden sie von Azure AD nicht gesendet. Nachfolgend sind die eingeschränkten SAML-Anspruchssätze aufgeführt:

    | Anspruchstyp (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expired |
    | http://schemas.microsoft.com/identity/claims/accesstoken |
    | http://schemas.microsoft.com/identity/claims/openid2_id |
    | http://schemas.microsoft.com/identity/claims/identityprovider |
    | http://schemas.microsoft.com/identity/claims/objectidentifier |
    | http://schemas.microsoft.com/identity/claims/puid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier[MR1] |
    | http://schemas.microsoft.com/identity/claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groups |
    | http://schemas.microsoft.com/claims/groups.link |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/claims/scope |

## <a name="next-steps"></a>Nächste Schritte

* [Anwendungsverwaltung in Azure AD](../manage-apps/what-is-application-management.md)
* [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure AD-Anwendungskatalog enthalten sind](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Problembehandlung bei SAML-basiertem einmaligem Anmelden](howto-v1-debug-saml-sso-issues.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png
