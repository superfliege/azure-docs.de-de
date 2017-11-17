---
title: "Azure AD Connect: Passthrough-Authentifizierung – Aktuelle Einschränkungen | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen zu den aktuellen Einschränkungen der Azure AD-Passthrough-Authentifizierung (Azure Active Directory)."
services: active-directory
keywords: "Azure AD Connect-Passthrough-Authentifizierung, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 4a33df43ca218545d6c684103a64f2cd1460913b
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory-Passthrough-Authentifizierung: aktuelle Einschränkungen

>[!IMPORTANT]
>Es handelt sich bei der Azure AD-Passthrough-Authentifizierung um eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen. Die Passthrough-Authentifizierung steht nur in der weltweiten Instanz von Azure AD zur Verfügung, nicht für [Microsoft Cloud Deutschland](http://www.microsoft.de/cloud-deutschland) oder [Microsoft Azure Government-Cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Folgende Szenarios werden vollständig unterstützt:

- Benutzeranmeldungen bei allen webbrowserbasierten Anwendungen
- Benutzeranmeldungen bei Office 365-Clientanwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen – Office 2016 und Office 2013 _mit_ moderner Authentifizierung.
- Azure AD Join für Windows 10-Geräte
- Unterstützung für Exchange ActiveSync.

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Folgende Szenarios werden _nicht_ unterstützt:

- Benutzeranmeldungen bei älteren Office-Clientanwendungen (Office 2010 und Office 2013 _ohne_ moderne Authentifizierung). Organisationen wird empfohlen, möglichst zur modernen Authentifizierung zu wechseln. Die moderne Authentifizierung ermöglicht nicht nur die Unterstützung der Passthrough-Authentifizierung, sondern trägt mit Features für [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) wie etwa Multi-Factor Authentication (MFA) auch zum Schutz Ihrer Benutzerkonten bei.
- Benutzeranmeldungen bei Skype for Business-Clientanwendungen inklusive Skype for Business 2016.
- Benutzeranmeldungen bei PowerShell v1.0. Es empfiehlt sich, stattdessen PowerShell 2.0 zu verwenden.
- Azure AD Domain Services
- App-Kennwörter für MFA.
- Erkennen von Benutzern mit [kompromittierten Anmeldeinformationen](../active-directory-reporting-risk-events.md#leaked-credentials).

>[!IMPORTANT]
>Zur _ausschließlichen_ Umgehung nicht unterstützter Szenarien können Sie im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) die Kennworthashsynchronisierung aktivieren. Durch das Aktivieren der Kennworthashsynchronisierung erhalten Sie außerdem die Möglichkeit einer Failoverauthentifizierung, wenn Ihre lokale Infrastruktur vollständig unterbrochen wird. Dieser Failover von Passthrough-Authentifizierung zu Kennworthashsynchronisierung erfolgt nicht automatisch, muss aber mithilfe des Microsoft-Supports durchgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
- [**Schnellstart**](active-directory-aadconnect-pass-through-authentication-quick-start.md): Einrichten und Ausführen der Passthrough-Authentifizierung mit Azure AD
- [**Smart Lockout**](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): Konfigurieren der Smart Lockout-Funktion für Ihren Mandanten zum Schutz der Benutzerkonten.
- [**Technische Einzelheiten**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) – Funktionsweise dieses Features verstehen
- [**Häufig gestellte Fragen**](active-directory-aadconnect-pass-through-authentication-faq.md) – Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**Ausführliche Informationen zur Sicherheit**](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): zusätzliche ausführliche technische Informationen zum Feature.
- [**Nahtlose SSO mit Azure AD**](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen
