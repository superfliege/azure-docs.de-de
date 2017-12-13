---
title: "Azure AD Connect: Passthrough-Authentifizierung – Aktuelle Einschränkungen | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen zu den aktuellen Einschränkungen der Azure Active Directory-Passthrough-Authentifizierung (Azure AD)."
services: active-directory
keywords: "Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: billmath
ms.openlocfilehash: a7edfd1939ad45dd3309fe5eaee2afa36086e9eb
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory-Passthrough-Authentifizierung: aktuelle Einschränkungen

>[!IMPORTANT]
>Es handelt sich bei der Azure Active Directory-Passthrough-Authentifizierung (Azure AD) um eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen. Die Passthrough-Authentifizierung steht nur in der weltweiten Instanz von Azure AD zur Verfügung, nicht für die Cloud [Microsoft Azure Deutschland](http://www.microsoft.de/cloud-deutschland) oder die Cloud [Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Folgende Szenarios werden vollständig unterstützt:

- Benutzeranmeldungen bei allen webbrowserbasierten Anwendungen
- Benutzeranmeldungen bei Office 365-Clientanwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen
- Office 2016 und Office 2013 _mit_ moderner Authentifizierung
- Azure AD-Domänenbeitritte für Windows 10-Geräte
- Unterstützung für Exchange ActiveSync

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Folgende Szenarios werden _nicht_ unterstützt:

- Benutzeranmeldungen bei älteren Office-Clientanwendungen Office 2010 und Office 2013 _ohne_ moderne Authentifizierung. Organisationen wird empfohlen, möglichst zur modernen Authentifizierung zu wechseln. Die moderne Authentifizierung ermöglicht die Unterstützung der Passthrough-Authentifizierung. Sie trägt zudem mit Features für [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) wie etwa Multi-Factor Authentication (MFA) auch zum Schutz Ihrer Benutzerkonten bei.
- Benutzeranmeldungen bei Skype for Business-Clientanwendungen, inklusive Skype for Business 2016
- Benutzeranmeldungen bei PowerShell Version 1.0. Es empfiehlt sich, PowerShell 2.0 zu verwenden.
- Azure Active Directory Domain Services
- App-Kennwörter für Multi-Factor Authentication
- Erkennen von Benutzern mit [kompromittierten Anmeldeinformationen](../active-directory-reporting-risk-events.md#leaked-credentials).

>[!IMPORTANT]
>Zur _ausschließlichen_ Umgehung nicht unterstützter Szenarien können Sie im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) die Kennworthashsynchronisierung aktivieren.

>[!NOTE]
Durch das Aktivieren der Kennworthashsynchronisierung erhalten Sie die Möglichkeit einer Failoverauthentifizierung, wenn Ihre lokale Infrastruktur unterbrochen wird. Dieses Failover von Passthrough-Authentifizierung zu Kennworthashsynchronisierung erfolgt nicht automatisch. Sie müssen die Anmeldemethode manuell mit Azure AD Connect wechseln. Wenn der Server, auf dem Azure AD Connect ausgeführt wird, ausfällt, benötigen Sie die Hilfe des Microsoft-Support, um die Pass-Through-Authentifizierung zu deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
- [Schnellstart:](active-directory-aadconnect-pass-through-authentication-quick-start.md) Aktivieren und Ausführen der Passthrough-Authentifizierung von Azure AD
- [Smart Lockout:](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) Konfigurieren der Smart Lockout-Funktion für Ihren Mandanten, um Benutzerkonten zu schützen
- [Technische Einzelheiten:](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informationen zur Funktionsweise der Passthrough-Authentifizierung
- [Häufig gestellte Fragen:](active-directory-aadconnect-pass-through-authentication-faq.md) Antworten auf häufig gestellte Fragen zur Passthrough-Authentifizierung
- [Problembehandlung:](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) Informationen zum Beheben von allgemeinen Problemen, die bei der Passthrough-Authentifizierung auftreten können
- [Ausführliche Informationen zur Sicherheit:](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) Technische Informationen zur Passthrough-Authentifizierung
- [Nahtloses SSO mit Azure AD](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [UserVoice:](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Anfordern neuer Features über das Azure Active Directory-Forum

