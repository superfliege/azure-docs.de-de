---
title: 'Azure AD Connect: Passthrough-Authentifizierung – Aktuelle Einschränkungen | Microsoft-Dokumentation'
description: Dieser Artikel enthält Informationen zu den aktuellen Einschränkungen der Azure Active Directory-Passthrough-Authentifizierung (Azure AD).
services: active-directory
keywords: Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ff023812acd5e30bfec34254379431b3e620dac9
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781841"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory-Passthrough-Authentifizierung: aktuelle Einschränkungen

>[!IMPORTANT]
>Es handelt sich bei der Azure Active Directory-Passthrough-Authentifizierung (Azure AD) um eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen. Die Passthrough-Authentifizierung steht nur in der weltweiten Instanz von Azure AD zur Verfügung, nicht für die Cloud [Microsoft Azure Deutschland](http://www.microsoft.de/cloud-deutschland) oder die Cloud [Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Folgende Szenarios werden unterstützt:

- Benutzeranmeldungen bei webbrowserbasierten Anwendungen.
- Benutzeranmeldungen bei Outlook-Clients über ältere Protokolle wie Exchange ActiveSync, EAS, SMTP, POP und IMAP.
- Benutzeranmeldungen bei älteren Office-Clientanwendungen und Office-Anwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen: Versionen Office 2010, 2013 und 2016.
- Benutzeranmeldungen bei älteren Protokollanwendungen, z.B. PowerShell Version 1.0 und andere.
- Azure AD-Joins für Windows 10-Geräte.
- App-Kennwörter für Multi-Factor Authentication

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Folgende Szenarios werden _nicht_ unterstützt:

- Erkennen von Benutzern mit [kompromittierten Anmeldeinformationen](../reports-monitoring/concept-risk-events.md#leaked-credentials).
- Für Azure AD Domain Services muss Kennworthashsynchronisierung auf dem Mandanten aktiviert sein. Aus diesem Grund funktionieren Mandanten, die Pass-Through-Authentifizierung verwenden, _nur_ in Szenarien nicht, die Azure AD Domain Services benötigen.
- Pass-Through Authentifizierung ist nicht in [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md) integriert.

>[!IMPORTANT]
>Zur _ausschließlichen_ Umgehung nicht unterstützter Szenarien (Azure AD Connect Health-Integration ausgenommen) können Sie im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) die Kennworthashsynchronisierung aktivieren.

>[!NOTE]
Durch das Aktivieren der Kennworthashsynchronisierung erhalten Sie die Möglichkeit einer Failoverauthentifizierung, wenn Ihre lokale Infrastruktur unterbrochen wird. Dieses Failover von Passthrough-Authentifizierung zu Kennworthashsynchronisierung erfolgt nicht automatisch. Sie müssen die Anmeldemethode manuell mit Azure AD Connect wechseln. Wenn der Server, auf dem Azure AD Connect ausgeführt wird, ausfällt, benötigen Sie die Hilfe des Microsoft-Support, um die Pass-Through-Authentifizierung zu deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
- [Schnellstart:](active-directory-aadconnect-pass-through-authentication-quick-start.md) Aktivieren und Ausführen der Passthrough-Authentifizierung von Azure AD
- [Migrieren von AD FS zur Passthrough-Authentifizierung](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx): Ein detaillierter Leitfaden zur Migration von AD FS (oder anderen Verbundtechnologien) zur Passthrough-Authentifizierung
- [Smart Lockout:](../authentication/howto-password-smart-lockout.md) Konfigurieren der Smart Lockout-Funktion für Ihren Mandanten, um Benutzerkonten zu schützen
- [Technische Einzelheiten:](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informationen zur Funktionsweise des Features für die Passthrough-Authentifizierung
- [Häufig gestellte Fragen:](active-directory-aadconnect-pass-through-authentication-faq.md) Antworten auf häufig gestellte Fragen zur Passthrough-Authentifizierung
- [Problembehandlung:](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) Informationen zum Beheben von allgemeinen Problemen, die bei der Passthrough-Authentifizierung auftreten können
- [Ausführliche Informationen zur Sicherheit:](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) Technische Informationen zur Passthrough-Authentifizierung
- [Nahtloses SSO mit Azure AD](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [UserVoice:](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Anfordern neuer Features über das Azure Active Directory-Forum
