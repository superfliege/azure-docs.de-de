---
title: Azure MFA-Server oder Dienst, lokal oder in der Cloud?
description: Welche Version von MFA soll ich als Azure AD-Administrator bereitstellen?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 783564f8c924cc0b7f94d94748ead35300250ded
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075566"
---
# <a name="which-version-of-azure-mfa-is-right-for-my-organization"></a>Welche Version von Azure MFA ist für meine Organisation die richtige?

Bevor Sie entscheiden, wo und wie Sie Multi-Factor Authentication (MFA) bereitstellen, müssen Sie drei grundlegende Fragen beantworten.

* [Was möchte ich sichern?](#what-am-i-trying-to-secure)
* [Wo befinden sich die Benutzer?](#where-are-the-users-located)
* [Welche Features benötige ich?](#what-features-do-i-need)

Jeder der folgenden Abschnitte enthält Details, die Ihnen dabei helfen, diese Fragen zu beantworten.

## <a name="what-am-i-trying-to-secure"></a>Was möchte ich sichern?

Um die richtige zweistufige Überprüfungslösung zu ermitteln, müssen Sie zunächst die Frage beantworten, was Sie mithilfe des zusätzlichen Authentifizierungsschritts sichern möchten. Handelt es sich um eine Anwendung in Azure? Oder ist es ein RAS-System? Die Antwort auf die Frage, wo Multi-Factor Authentication aktiviert werden muss, richtet sich danach, was Sie sichern möchten.

| Was möchte ich sichern? | MFA in der Cloud | MFA-Server |
| --- |:---:|:---:|
| Microsoft-Apps |● |● |
| SaaS-Apps im Appkatalog |● |  |
| Über den Azure AD-App-Proxy veröffentlichte Webanwendungen |● |  |
| Nicht über den Azure AD-App-Proxy veröffentlichte IIS-Anwendungen | |● |
| Remotezugriff, z. B. VPN, RDG | ● | ● |

## <a name="where-are-the-users-located"></a>Wo befinden sich die Benutzer?

Außerdem hängt die Entscheidung für die richtige Lösung – in der Cloud oder lokal über den MFA-Server – davon ab, wo sich die Benutzer Ihrer Organisation befinden.

| Benutzerstandort | MFA in der Cloud | MFA-Server |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD und lokales AD über einen Verbund mit AD FS |● |● |
| Azure AD und lokales AD mit Azure AD Connect ohne Kennworthashsynchronisierung oder Passthrough-Authentifizierung |● |● |
| Azure AD und lokales AD mit Azure AD Connect mit Kennworthashsynchronisierung oder Passthrough-Authentifizierung |● | |
| Lokales Active Directory | |● |

## <a name="what-features-do-i-need"></a>Welche Features benötige ich?

Die folgende Tabelle enthält eine Gegenüberstellung der Features von Multi-Factor Authentication in der Cloud und dem Multi-Factor Authentication-Server.

| Feature | MFA in der Cloud | MFA-Server |
| --- |:---:|:---:|
| Benachrichtigung in der mobilen App als zweite Stufe | ● | ● |
| Bestätigungscode in der mobilen App als zweite Stufe | ● | ● |
| Telefonanruf als zweite Stufe | ● | ● |
| Unidirektionale SMS als zweite Stufe | ● | ● |
| Hardwaretoken als zweite Stufe | ● (Öffentliche Vorschau) | ● |
| App-Kennwörter für Office 365-Clients, die MFA nicht unterstützen | ● | |
| Administrative Kontrolle über Authentifizierungsmethoden | ● | ● |
| PIN-Modus | | ● |
| Betrugswarnung | ● | ● |
| MFA-Berichte | ● | ● |
| Einmalumgehung | | ● |
| Benutzerdefinierte Begrüßungen für Telefonanrufe | ● | ● |
| Benutzerdefinierte Anrufer-ID für Telefonanrufe | ● | ● |
| Vertrauenswürdige IP-Adressen | ● | ● |
| Speichern der MFA für vertrauenswürdige Geräte | ● | |
| Bedingter Zugriff | ● | ● |
| Cache |  | ● |

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun den Unterschied zwischen Azure Multi-Factor Authentication in der Cloud und dem lokalen MFA-Server kennen, können Sie Azure Multi-Factor Authentication einrichten. **Wählen Sie das Symbol aus, das Ihrem Szenario entspricht.**

<center>

[![MFA in der Cloud](./media/concept-mfa-whichversion/cloud2.png)](howto-mfa-getstarted.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [![MFA-Server](./media/concept-mfa-whichversion/server2.png)](howto-mfaserver-deploy.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
