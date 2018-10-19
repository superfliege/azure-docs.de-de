---
title: Grundlagen der Azure-Identitäts- und Zugriffsverwaltung – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie mehr über die erweiterten Schutzfunktionen und zusätzlichen Tools, die die Azure Active Directory Premium-Editionen bereitstellen.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2018
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: f7baa29c77ae4af9813bfc755a39cc07288a3ad2
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734674"
---
# <a name="what-are-the-fundamentals-of-azure-identity-and-access-management"></a>Grundlagen der Azure-Identitäts- und Zugriffsverwaltung
Azure AD Premium ist eine cloudbasierte Identitäts- und Zugriffsverwaltungslösung mit erweiterten Schutzfunktionen. Diese erweiterten Funktionen bieten eine sichere Identität für alle Ihre Apps, Identitätsschutz (optimiert durch [Microsoft 	Intelligent Security Graph](https://www.microsoft.com/security/intelligence)) und [Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md). Azure AD unterstützt Sie dabei, die Identitäten Ihrer Benutzer in Echtzeit zu schützen sowie risikobasierte und anpassungsfähige Zugriffsrichtlinien für Ihre Organisationsdaten zu erstellen.

In diesem kurzen Video erhalten Sie einen schnellen Überblick über Identitätsverwaltung und Schutz mit Azure AD:
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Azure AD stellt auch eine Reihe von Tools bereit, mit denen Sie Ihre Umgebung sichern, automatisieren und verwalten können. Dies umfasst das Zurücksetzen von Kennwörtern, Benutzer-und Gruppenverwaltung und App-Anforderungen. Außerdem unterstützt Sie Azure AD bei der Verwaltung benutzereigener Geräte sowie beim Zugriff auf und bei der Steuerung von SaaS-Apps (Software-as-a-Service).

Weitere Informationen zu den Kosten für die Azure Active Directory Premium-Editionen und die entsprechenden Tools finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Verbinden einer lokalen Active Directory-Instanz mit Azure AD und Office 365
Erweitern Sie Ihre lokale Active Directory-Implementierung in die Cloud, indem Sie Ihre lokalen Verzeichnisse mit Azure AD über die [Hybrididentitätsverwaltung](https://aka.ms/aadframework) integrieren. [Azure AD Connect](../connect/active-directory-aadconnect.md) stellt diese Integration bereit und gibt Ihren Benutzern eine einzige Identität und SSO-Zugriff (Single Sign-On, einmaliges Anmelden) auf Ihre lokalen Ressourcen und Ihre Clouddienste, z.B. Office 365.

Azure AD Connect ersetzt ältere Versionen von Identitätsintegrationstools wie DirSync und AAD Sync, um Ihre Anforderungen an Identitätssynchronisierung zwischen lokalen Ressourcen und Azure AD zu erfüllen. Die Azure AD Connect-Synchronisierung wird durch die folgenden Komponenten ermöglicht:

- **Synchronisierung:** Diese Komponente ist verantwortlich für das Erstellen von Benutzern, Gruppen und anderen Objekten. Außerdem stellt sie sicher, dass Identitätsinformationen für Ihre lokalen Benutzer denen in Azure AD entsprechen. Wenn Sie das Kennwortrückschreiben aktivieren, werden Ihre lokalen Verzeichnisse synchronisiert, falls Benutzer Kennwörter in Azure AD aktualisieren.

- Authentifizierung: Es ist wichtig, beim Einrichten Ihrer Azure AD-Hybrididentitätslösung die richtige Authentifizierungsmethode auszuwählen. Die verfügbaren Optionen für Ihre Organisation sind Cloudauthentifizierung (Kennworthashsynchronisierung/Passthrough-Authentifizierung) und Verbundauthentifizierung (AD FS). Weitere Informationen zu den verfügbaren Optionen finden Sie unter [Wählen der richtigen Authentifizierungsmethode für Ihre Azure Active Directory-Hybrididentitätslösung](https://aka.ms/auth-options).

- **Systemüberwachung:** Azure AD Connect Health bietet Überwachung und einen zentralen Speicherort im Azure-Portal, um diese Aktivität anzuzeigen. Ausführlichere Informationen finden Sie unter [Überwachen Ihrer lokalen Identitätsinfrastruktur und Synchronisierung von Diensten in der Cloud](../connect-health/active-directory-aadconnect-health.md).

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Mehr Produktivität und weniger Helpdeskkosten durch Self-Service und Single Sign-On
Benutzer sparen Zeit, wenn sie nur einen einzigen Benutzernamen und ein einziges Kennwort verwenden – sowie eine einheitliche Oberfläche auf jedem Gerät. Außerdem gewinnen sie Zeit, indem sie Aufgaben selbst ausführen, z.B. [vergessene Kennwörter zurücksetzen](../user-help/active-directory-passwords-update-your-own-password.md) oder den Zugriff auf eine Anwendung anfordern, ohne auf Unterstützung durch den Helpdesk zu warten.

Azure AD unterstützt SSO und eine konsistente Umgebung durch die [Erweiterung Ihrer lokalen Active Directory-Instanz](../connect/active-directory-aadconnect.md) in die Cloud. So können Ihre Benutzer ihr primäres Organisationskonto sowohl für die mit ihrer Domäne verbundenen Geräte und Unternehmensressourcen als auch für sämtliche Web- und SaaS-Anwendungen verwenden, die sie für ihre Arbeit benötigen. 

Darüber hinaus kann der Anwendungszugriff automatisch bereitgestellt (bzw. die Bereitstellung aufgehoben) werden, basierend auf Gruppenmitgliedschaften und dem Mitarbeiterstatus eines Benutzers, wodurch Sie den Zugriff auf Katalog-Apps oder Ihre eigenen lokalen Apps steuern können, die Sie über den [Azure AD-Anwendungsproxy](../manage-apps/application-proxy.md) entwickelt und veröffentlicht haben.

## <a name="manage-and-control-access-to-your-organizational-resources"></a>Verwalten und Steuern des Zugriffs auf Organisationsressourcen
Lösungen zur Identitäts- und Zugriffsverwaltung von Microsoft unterstützen Sie dabei, den Zugriff auf Apps und Ressourcen in Ihrem Organisationsrechenzentrum und in der Cloud zu schützen. Diese Zugriffsverwaltung ermöglicht die Bereitstellung zusätzlicher Überprüfungsebenen, z.B. [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) und [Richtlinien für bedingten Zugriff](../conditional-access/overview.md). Das Überwachen verdächtiger Aktivitäten über erweiterte Sicherheitsberichterstellung, Überwachung und Warnungen trägt dazu bei, potenzielle Sicherheitsprobleme zu verringern.

Richtlinien für bedingten Zugriff in Azure AD Premium bieten Ihnen die Möglichkeit zum Erstellen von richtlinienbasierten Zugriffsregeln für jede mit Azure AD verbundene App, z.B. für SaaS-Apps, in der Cloud ausgeführte benutzerdefinierte Apps, lokale oder Web-Apps. Azure AD wertet Ihre Regeln in Echtzeit aus und setzt sie durch, wenn ein Benutzer versucht, auf eine App zuzugreifen. Mit den Azure-Identitätsschutzrichtlinien können Sie beim Entdecken verdächtiger Aktivitäten automatisch Maßnahmen ergreifen, z.B. den Zugriff sperren, Multi-Factor Authentication erzwingen oder Benutzerkennwörter zurücksetzen.

## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management
[Privileged Identity Management (PIM)](../privileged-identity-management/pim-getting-started.md) ist in der Azure Active Directory Premium P2-Edition enthalten und ermöglicht Ihnen das Ermitteln, Einschränken und Überwachen von Administratorkonten und deren Zugriff auf Ressourcen in Ihrer Azure Active Directory-Instanz und anderen Microsoft-Onlinediensten. Mit PIM können Sie auch den bedarfsabhängigen Administratorzugriff für einen bestimmten Zeitraum verwalten. Das heißt, Sie können Administratoren erlauben, eine mehrstufig authentifizierte, temporäre Erweiterung ihrer Rechte für einen vorkonfigurierten Zeitraum anzufordern, bevor ihre Konten wieder den normalen Benutzerstatus erlangen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure AD-Architektur finden Sie unter [Was ist die Azure AD-Architektur?](active-directory-architecture.md).
