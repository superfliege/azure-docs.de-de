---
title: Identitätsdatenspeicher für europäische Kunden – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wo identitätsbezogene Daten für europäische Kunden durch Azure Active Directory gespeichert werden.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/17/2018
ms.custom: it-pro, seodec18
ms.openlocfilehash: 371c7b2eb2f2e0e34dc80cf8fe6771620fa76765
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53099935"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Identitätsdatenspeicher für europäische Kunden in Azure Active Directory
Mit Azure Active Directory (Azure AD) können Sie Benutzeridentitäten verwalten und auf Intelligence basierende Zugriffsrichtlinien erstellen, um die Ressourcen Ihrer Organisation zu schützen. Identitätsdaten werden an einem Ort gespeichert, der auf der Adresse Ihrer Organisation basiert, die Sie beim Abonnieren des Diensts angegeben haben. Dies kann beispielsweise der Vorgang zum Abonnieren von Office 365 oder Azure sein. Spezifische Informationen zur Speicherung Ihrer Identitätsdaten können Sie im Microsoft Trust Center im Abschnitt [Wo sich Ihre Daten befinden](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) einsehen.

Die meisten Azure AD-bezogenen europäischen Identitätsdaten bleiben zwar in Datencentern in Europa, aber es gibt fünf benutzerbezogene Attribute, die normalerweise in Datencentern in den USA gespeichert werden. Dies sind die Attribute „GivenName“, „Surname“, „userPrincipalName“, „Domain“ und „PasswordHash“. Das Attribut „PasswordHash“ kann eine Ausnahme sein und nicht in den USA gespeichert werden, falls ein lokales Verfahren für die Verbundauthentifizierung verwendet wird, durch das für den PasswordHash-Wert die Synchronisierung mit Azure AD verhindert wird. Zusätzlich sind einige dienstspezifische Betriebsdaten vorhanden, die für den normalen Azure AD-Betrieb benötigt werden. Diese Daten werden in den USA gespeichert und enthalten keine persönlichen Daten.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Außerhalb von europäischen Datencentern gespeicherte Daten für europäische Kunden

Die meisten Azure AD-bezogenen europäischen Identitätsdaten für Organisationen mit Adressen in Europa bleiben in europäischen Datencentern. Folgende Azure AD-Daten werden in europäischen Datencentern gespeichert und auch in US-Datencentern repliziert:

- **Identitätsbezogene Attribute**

    Die folgenden identitätsbezogenen Attribute werden in den USA repliziert:

    - GivenName
    - Surname
    - userPrincipalName
    - Domäne
    - PasswordHash
    - SourceAnchor
    - AccountEnabled
    - PasswordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Microsoft Azure Multi-Factor Authentication (MFA) und Azure AD-Self-Service-Kennwortzurücksetzung (SSPR)**
    
    Für MFA werden alle ruhenden Benutzerdaten in europäischen Datencentern gespeichert. Einige für den MFA-Dienst spezifische Daten werden aber in den USA gespeichert, z.B.:
    
    - Die Daten für die zweistufige Authentifizierung und die dazugehörigen persönlichen Daten werden unter Umständen in den USA gespeichert, wenn Sie MFA oder SSPR nutzen.
        - Die zweistufige Authentifizierung per Telefonanruf oder SMS wird unter Umständen von US-Betreibern durchgeführt.
        - Für Pushbenachrichtigungen mit der Microsoft Authenticator-App sind Benachrichtigungen über den Benachrichtigungsdienst des Herstellers (Apple oder Google) erforderlich, der sich ggf. außerhalb von Europa befindet.
        - OATH-Codes werden immer in den USA überprüft. 
    - Einige MFA- und SSPR-Protokolle werden unabhängig vom Authentifizierungstyp 30 Tage lang in den USA gespeichert.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Für Azure AD B2C werden alle ruhenden Benutzerdaten in europäischen Datencentern gespeichert. Die Betriebsprotokolle verbleiben (nach Entfernung der persönlichen Daten) an dem Standort, von dem aus die Person auf die Dienste zugreift. Wenn ein B2C-Benutzer beispielsweise in den USA auf den Dienst zugreift, verbleiben die Betriebsprotokolle in den USA. Außerdem werden alle Richtlinienkonfigurationsdaten, die keine persönlichen Daten enthalten, nur in den USA gespeichert. Weitere Informationen zu Richtlinienkonfigurationen finden Sie im Artikel [Azure Active Directory B2C: Integrierte Richtlinien](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies).

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    Für Azure AD B2B werden alle ruhenden Benutzerdaten in europäischen Datencentern gespeichert. Allerdings werden für B2B Metadaten, die keine persönlichen Daten enthalten, in Tabellen in Datencentern in den USA gespeichert. Diese Tabelle enthält Felder wie „redeemUrl“, „invitationTicket“, „resource tenant Id“, „InviteRedirectUrl“ und „InviterAppId“.

- **Microsoft Azure Active Directory Domain Services (Azure AD DS)**

    Für Azure AD DS werden die Benutzerdaten an dem Ort gespeichert, der vom Kunden für das virtuelle Azure-Netzwerk ausgewählt wurde. Wenn das Netzwerk also außerhalb von Europa liegt, werden die Daten außerhalb Europas repliziert und gespeichert.

- **In Azure AD integrierte Dienste und Apps**

    Alle Dienste und Apps, die in Azure AD integriert werden, haben Zugriff auf Identitätsdaten. Evaluieren Sie jeden Dienst und jede App, um zu ermitteln, wie Identitätsdaten vom jeweiligen Dienst bzw. der App verarbeitet werden und ob sie die Datenspeicheranforderungen Ihres Unternehmens erfüllen.

    Weitere Informationen zur Datenaufbewahrung finden Sie im Microsoft Trust Center im Abschnitt [Wo sich Ihre Daten befinden](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den oben beschriebenen Features und Funktionen finden Sie in diesen Artikeln:
- [Was ist Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
- [Self-Service-Kennwortzurücksetzung in Azure AD für IT-Experten](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)
- [Was ist Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
