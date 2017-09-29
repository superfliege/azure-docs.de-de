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
ms.date: 09/18/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 6e193efe391c614a17d2861d4ba7d7776a7d441c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory-Passthrough-Authentifizierung: aktuelle Einschränkungen

>[!IMPORTANT]
>Es handelt sich bei der Azure AD-Passthrough-Authentifizierung um eine kostenlose Funktion, sodass Sie für deren Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen. Die Passthrough-Authentifizierung steht nur in der weltweiten Instanz von Azure AD zur Verfügung, nicht für [Microsoft Cloud Deutschland](http://www.microsoft.de/cloud-deutschland) oder [Microsoft Azure Government-Cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Folgende Szenarios werden vollständig unterstützt:

- Benutzeranmeldungen bei allen webbrowserbasierten Anwendungen
- Benutzeranmeldungen bei Office 365-Clientanwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen
- Azure AD Join für Windows 10-Geräte
- Unterstützung für Exchange ActiveSync.

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Folgende Szenarios werden _nicht_ unterstützt:

- Benutzeranmeldungen bei älteren Office-Clientanwendungen (Office 2013 oder früher). Organisationen wird empfohlen, möglichst zur modernen Authentifizierung zu wechseln. Die moderne Authentifizierung ermöglicht nicht nur die Unterstützung der Passthrough-Authentifizierung, sondern trägt mit Features für [bedingten Zugriff](../active-directory-conditional-access.md) wie etwa Multi-Factor Authentication (MFA) auch zum Schutz Ihrer Benutzerkonten bei.
- Benutzeranmeldungen bei Skype for Business-Clientanwendungen inklusive Skype for Business 2016.
- Benutzeranmeldungen bei PowerShell v1.0. Es empfiehlt sich, stattdessen PowerShell 2.0 zu verwenden.
- App-Kennwörter für MFA.
- Erkennen von Benutzern mit [kompromittierten Anmeldeinformationen](../active-directory-reporting-risk-events.md#leaked-credentials).

>[!IMPORTANT]
>Zur Umgehung nicht unterstützter Szenarien können Sie im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) die Kennworthashsynchronisierung aktivieren. Die Kennworthashsynchronisierung ist _nur eine Ausweichlösung für vorherigen Szenarien_ (also _keine_ allgemeine Ausweichlösung für die Passthrough-Authentifizierung). Durch das Aktivieren der Kennworthashsynchronisierung erhalten Sie außerdem die Möglichkeit einer Failoverauthentifizierung (über den Microsoft-Support), wenn Ihre lokale Infrastruktur unterbrochen wird.

## <a name="next-steps"></a>Nächste Schritte
- [**Schnellstartanleitung:**](active-directory-aadconnect-pass-through-authentication-quick-start.md) Richten Sie die Azure AD-Passthrough-Authentifizierung ein.
- [**Ausführliche technische Betrachtung:**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informieren Sie sich über die Funktionsweise dieses Features.
- [**Häufig gestellte Fragen**](active-directory-aadconnect-pass-through-authentication-faq.md) – Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**Nahtlose SSO mit Azure AD**](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen

