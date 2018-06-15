---
title: 'Azure AD Connect: Passthrough-Authentifizierung – Aktuelle Einschränkungen | Microsoft-Dokumentation'
description: Dieser Artikel enthält Informationen zu den aktuellen Einschränkungen der Azure Active Directory-Passthrough-Authentifizierung (Azure AD).
services: active-directory
keywords: Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ebfbb972d567963b6c302b7e6151f73165c4a87b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34590673"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory-Passthrough-Authentifizierung: aktuelle Einschränkungen

>[!IMPORTANT]
>Es handelt sich bei der Azure Active Directory-Passthrough-Authentifizierung (Azure AD) um eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen. Die Passthrough-Authentifizierung steht nur in der weltweiten Instanz von Azure AD zur Verfügung, nicht für die Cloud [Microsoft Azure Deutschland](http://www.microsoft.de/cloud-deutschland) oder die Cloud [Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Folgende Szenarios werden vollständig unterstützt:

- Benutzeranmeldungen bei allen webbrowserbasierten Anwendungen
- Benutzeranmeldungen bei Office-Anwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen: Office 2016 und Office 2013 _mit_ moderner Authentifizierung
- Benutzeranmeldungen bei Outlook-Clients über ältere Protokolle wie Exchange ActiveSync, SMTP, POP und IMAP.
- Benutzeranmeldungen bei Skype for Business, die moderne Authentifizierung unterstützen, einschließlich Online- und Hybridtopologien. Weitere Informationen zu unterstützten Topologien finden Sie [hier](https://technet.microsoft.com/library/mt803262.aspx).
- Azure AD-Domänenbeitritte für Windows 10-Geräte
- App-Kennwörter für Multi-Factor Authentication

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Folgende Szenarios werden _nicht_ unterstützt:

- Benutzeranmeldungen bei älteren Office-Clientanwendungen außer Outlook (siehe oben **Unterstützte Szenarien**): Office 2010 und Office 2013 _ohne_ moderne Authentifizierung Organisationen wird empfohlen, möglichst zur modernen Authentifizierung zu wechseln. Die moderne Authentifizierung ermöglicht die Unterstützung der Passthrough-Authentifizierung. Sie trägt zudem mit Features für [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) wie etwa Multi-Factor Authentication (MFA) auch zum Schutz Ihrer Benutzerkonten bei.
- Zugriff auf Kalenderfreigabe und Frei-/Gebucht-Informationen in Exchange-Hybridumgebungen nur in Office 2010.
- Benutzeranmeldungen bei Skype for Business-Clientanwendungen _ohne_ moderne Authentifizierung.
- Benutzeranmeldungen bei PowerShell Version 1.0. Es empfiehlt sich, PowerShell 2.0 zu verwenden.
- Erkennen von Benutzern mit [kompromittierten Anmeldeinformationen](../active-directory-reporting-risk-events.md#leaked-credentials).
- Für Azure AD Domain Services muss Kennworthashsynchronisierung auf dem Mandanten aktiviert sein. Aus diesem Grund funktionieren Mandanten, die Pass-Through-Authentifizierung verwenden, _nur_ in Szenarien nicht, die Azure AD Domain Services benötigen.
- Pass-Through Authentifizierung ist nicht in [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md) integriert.
- Das Apple Device Enrollment Program (Apple DEP) mit dem iOS-Setup-Assistenten unterstützt keine moderne Authentifizierung. Bei Apple DEP-Geräten tritt bei der Registrierung bei Intune für verwaltete Domänen, die Pass-Through-Authentifizierung verwenden, ein Fehler auf. Sie können auch die [Unternehmensportal-App](https://blogs.technet.microsoft.com/intunesupport/2018/02/08/support-for-multi-token-dep-and-authentication-with-company-portal/) als Alternative verwenden.

>[!IMPORTANT]
>Zur _ausschließlichen_ Umgehung nicht unterstützter Szenarien können Sie im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) die Kennworthashsynchronisierung aktivieren. Wenn Benutzer sich bei Anwendungen anmelden, die im Abschnitt „nicht unterstützte Szenarien“ aufgeführt sind, werden diese spezifischen Anmeldeanforderungen _nicht_ von Passthrough-Authentifizierungs-Agents verarbeitet und daher nicht in [ Passthrough-Authentifizierungs-Protokollen](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs) aufgezeichnet.

>[!NOTE]
Durch das Aktivieren der Kennworthashsynchronisierung erhalten Sie die Möglichkeit einer Failoverauthentifizierung, wenn Ihre lokale Infrastruktur unterbrochen wird. Dieses Failover von Passthrough-Authentifizierung zu Kennworthashsynchronisierung erfolgt nicht automatisch. Sie müssen die Anmeldemethode manuell mit Azure AD Connect wechseln. Wenn der Server, auf dem Azure AD Connect ausgeführt wird, ausfällt, benötigen Sie die Hilfe des Microsoft-Support, um die Pass-Through-Authentifizierung zu deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
- [Schnellstart:](active-directory-aadconnect-pass-through-authentication-quick-start.md) Aktivieren und Ausführen der Passthrough-Authentifizierung von Azure AD
- [Smart Lockout:](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) Konfigurieren der Smart Lockout-Funktion für Ihren Mandanten, um Benutzerkonten zu schützen
- [Technische Einzelheiten:](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informationen zur Funktionsweise des Features für die Passthrough-Authentifizierung
- [Häufig gestellte Fragen:](active-directory-aadconnect-pass-through-authentication-faq.md) Antworten auf häufig gestellte Fragen zur Passthrough-Authentifizierung
- [Problembehandlung:](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) Informationen zum Beheben von allgemeinen Problemen, die bei der Passthrough-Authentifizierung auftreten können
- [Ausführliche Informationen zur Sicherheit:](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) Technische Informationen zur Passthrough-Authentifizierung
- [Nahtloses SSO mit Azure AD](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [UserVoice:](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Anfordern neuer Features über das Azure Active Directory-Forum
