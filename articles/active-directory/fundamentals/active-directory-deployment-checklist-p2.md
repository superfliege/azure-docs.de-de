---
title: Premium P2-Lizenzierungsfeature-Prüfliste – Azure Active Directory | Microsoft-Dokumentation
description: Bereitstellungsprüfliste für Bereitstellung des Azure Active Directory-Premium P2-Features für 30 Tage, 90 Tage und länger.
services: active-directory
ms.service: active-directory
ms.component: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.custom: it-pro, seodec18
ms.openlocfilehash: 61cefe762b639dcc58c2e10fc845dd9729cb2cb2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53078232"
---
# <a name="azure-active-directory-premium-p2-licensing-feature-checklist"></a>Azure Active Directory Premium P2-Lizenzierungsfeature-Prüfliste

Manche Organisationen haben möglicherweise Bedenken, ob sie die Bereitstellung und Gewährleistung der Sicherheit von Azure Active Directory (Azure AD) bewältigen können. In diesem Artikel werden einige allgemeine Aufgaben behandelt, die für Kunden hilfreich sind. Kunden führen diese Aufgaben in der Regel innerhalb von 30 oder 90 Tagen (oder darüber hinaus) aus, um ihren Sicherheitsstatus zu verbessern. Selbst Organisationen, die Azure AD bereits bereitgestellt haben, können anhand dieser Prüfliste sicherstellen, dass sie ihre Investitionen optimal nutzen.

Eine gut geplante und ausgeführte Identitätsinfrastruktur ebnet den Weg zum sichereren Zugriff ausschließlich authentifizierter Benutzer und Geräte auf Ihre Produktivitätsworkloads und Daten.

## <a name="prerequisites"></a>Voraussetzungen

Dieser Leitfaden setzt voraus, dass Sie über Azure AD Premium P2-Lizenzen, Enterprise Mobility + Security E5, Microsoft 365 E5 oder ein ähnliches Lizenzpaket verfügen.

[Azure AD-Lizenzierung](https://azure.microsoft.com/pricing/details/active-directory/)

[Microsoft 365 Enterprise](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)

[Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)

## <a name="plan-and-deploy-day-1-30"></a>Planen und Bereitstellen: Tag 1-30

- Festlegen mehrerer globaler Administratoren (Notfallzugriff-Konto)
   - [Verwalten von Administratorkonten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md)
- Aktivieren von Azure AD Privileged Identity Management (PIM) zum Anzeigen von Berichten
   - [Einstieg in die Verwendung von PIM](../privileged-identity-management/pim-getting-started.md)
- Verwenden Sie nach Möglichkeit nicht globale Administratorrollen.
   - [Zuweisen von Administratorrollen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)
- Authentifizierung
   - [Erfolgreicher Rollout der Self-Service-Kennwortzurücksetzung](../authentication/howto-sspr-deployment.md)
   - Bereitstellen des Kennwortschutzes für Azure AD (Vorschauversion)
      - [Beseitigen falscher Kennwörter in Ihrer Organisation](../authentication/concept-password-ban-bad.md)
      - [Erzwingen des Azure AD-Kennwortschutzes für Windows Server Active Directory](../authentication/concept-password-ban-bad-on-premises.md)
   - Konfigurieren von Kontosperrungsrichtlinien
      - [Azure Active Directory Smart Lockout](../authentication/howto-password-smart-lockout.md)
      - [AD FS Extranet Lockout und Extranet Smart Lockout](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection)
   - [Bereitstellen von Azure AD Multi-Factor Authentication mit Richtlinien für bedingten Zugriff](../authentication/howto-mfa-getstarted.md)
   - [Aktivieren der zusammengeführten Registrierung für Self-Service-Kennwortzurücksetzung und Azure AD Multi-Factor Authentication (Vorschauversion)](../authentication/concept-registration-mfa-sspr-converged.md)
   - [Aktivieren der Azure Active Directory Identity Protection](../identity-protection/enable.md)
      - [Verwenden von Risikoereignissen zum Auslösen von Multi-Factor Authentication und Kennwortänderungen](../authentication/tutorial-risk-based-sspr-mfa.md)
   - [Kennwort-Leitfaden](https://www.microsoft.com/research/publication/password-guidance/)
      - Verwalten Sie eine Mindestlänge von acht Zeichen, mehr ist nicht notwendigerweise besser.
      - Vermeiden Sie Anforderungen an die Zeichenzusammensetzung.
      - [Vermeiden Sie obligatorisches regelmäßiges Zurücksetzen von Kennwörtern für Benutzerkonten.](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire)
- Synchronisieren von Benutzern aus dem lokalen Active Directory
   - [Installieren von Azure AD Connect](../connect/active-directory-aadconnect-select-installation.md)
   - [Implementieren der Kennworthashsynchronisierung](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)
   - [Implementieren der Kennwortrückschreibung](../authentication/howto-sspr-writeback.md)
   - [Implementieren von Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md)
- [Zuweisen von Lizenzen zu Benutzer nach Gruppenmitgliedschaft in Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)

## <a name="plan-and-deploy-day-31-90"></a>Planen und Bereitstellen: Tag 31-90

- [Planen des Zugriffs durch Gastbenutzer](../b2b/what-is-b2b.md)
   - [Add Azure Active Directory B2B collaboration users in the Azure portal](../b2b/add-users-administrator.md) (Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal)
   - [Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen](../b2b/allow-deny-list.md)
   - [Gewähren des Zugriffs auf lokale Anwendungen für B2B-Benutzer in Azure AD](../b2b/hybrid-cloud-to-on-premises.md)
- Treffen von Entscheidungen zur Strategie zur Verwaltung des Benutzerlebenszyklus
- [Entscheidungen zur Geräteverwaltungsstrategie](../devices/overview.md)
   - [Verwendungsszenarios und Bereitstellungsaspekte für Azure AD Join](../devices/azureadjoin-plan.md)
- [Verwalten von Windows Hello for Business in Ihrer Organisation](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization)

## <a name="plan-and-deploy-day-90-and-beyond"></a>Planen und Bereitstellen: Tag 90 und später

- [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md)
   - [Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM](../privileged-identity-management/pim-how-to-change-default-settings.md)
   - [Zuweisen von Azure AD-Verzeichnisrollen in PIM](../privileged-identity-management/pim-how-to-add-role-to-user.md)
- [Abschließen einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM](../privileged-identity-management/pim-how-to-start-security-review.md)
- Ganzheitliches Verwalten des Benutzerlebenszyklus
   - Azure AD bietet einen Ansatz für die Verwaltung des Identitätslebenszyklus
   - Entfernen Sie manuelle Schritte aus dem Kontolebenszyklus Ihrer Mitarbeiter, um nicht autorisierte Zugriffe zu verhindern:
      - Synchronisieren Sie Identitäten über Ihre zuverlässige Datenquelle (HR-System) mit Azure AD. Verknüpfung mit unterstützten HR-Systemen
      - [Weisen Sie mit dynamischen Gruppen automatisch Benutzer basierend auf deren Attributen von HR (oder Ihrer zuverlässigen Datenquelle) wie z.B. Abteilung, Titel, Region und anderen Attributen Gruppen zu.](../users-groups-roles/groups-dynamic-membership.md)
      - [Verwenden Sie die gruppenbasierte Bereitstellung der Zugriffsverwaltung zum automatischen Bereitstellen von Benutzern für SaaS-Anwendungen.](../manage-apps/what-is-access-management.md)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurationen für den Identitäts- und Gerätezugriff](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Allgemeine empfohlene Identitäts- und Gerätezugriffsrichtlinien](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
