---
title: Lizenzieren der Self-Service-Kennwortzurücksetzung – Azure Active Directory
description: Lizenzanforderungen für die Self-Service-Kennwortzurücksetzung in Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: b1cfb76c7e196441ebad3bcfcd1d788bd544ab74
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36292558"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung

In *Ihrer Organisation muss mindestens eine Lizenz für diesen Benutzer zugewiesen sein*, damit die Kennwortzurücksetzung in Azure Active Directory (Azure AD) funktioniert. Wenn ein Benutzer direkt oder indirekt von einem durch diese Lizenz abgedeckten Feature profitiert, ist eine vollständige Lizenz erforderlich.

* **Nur Cloudbenutzer**: beliebige kostenpflichtige SKU für Office 365 oder Azure AD Basic
* **Cloudbenutzer** oder **lokale Benutzer**: Azure AD Premium P1 oder P2, Enterprise Mobility + Security (EMS) oder Microsoft 365

## <a name="licensing-requirements-for-password-writeback"></a>Lizenzierungsanforderungen für das Kennwortrückschreiben

**Self-Service-Kennwortzurücksetzung/-änderung/-entsperrung mit lokalem Rückschreiben ist eine Premium-Funktion von Azure AD**. Weitere Informationen zur Lizenzierung finden Sie auf der [Preiswebsite für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

Damit Kennwortrückschreiben verwendet werden kann, muss in Ihrem Mandanten eine der folgenden Lizenzen zugewiesen sein:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 oder A3
* Enterprise Mobility + Security E5 oder A5
* Microsoft 365 E3 oder A3
* Microsoft 365 E5 oder A5
* Microsoft 365 F1

> [!WARNING]
> Eigenständige Office 365-Lizenzierungspläne *bieten keine Unterstützung für das Kennwortrückschreiben* und erfordern, dass Sie einen der obigen Pläne haben, damit diese Funktion verwendbar ist.
>

Weitere Informationen zur Lizenzierung, einschließlich der Kosten, finden Sie auf den folgenden Seiten:

* [Website mit Preisen für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-Features und -Funktionen](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Aktivieren der gruppen- oder benutzerbasierten Lizenzierung

Azure AD unterstützt jetzt gruppenbasierte Lizenzierung. Administratoren können einer Gruppe von Benutzern Lizenzen in einem Massenvorgang zuweisen, anstatt sie einzeln zuzuweisen. Weitere Informationen hierzu finden Sie unter [Zuweisen, Prüfen und Beheben von Problemen mit Lizenzen](../active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses).

Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft **Nutzungsspeicherort** für den Benutzer angeben. Die Zuweisung von Lizenzen erfolgt im Azure-Portal im Abschnitt **Benutzer** > **Profil** > **Einstellungen**. *Wenn Sie eine Gruppenlizenzzuweisung verwenden, erben alle Benutzer ohne Nutzungsspeicherort den Speicherort des Verzeichnisses.*

## <a name="next-steps"></a>Nächste Schritte

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](howto-sspr-deployment.md)
* [Zurücksetzen oder Ändern des Kennworts](../active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](../active-directory-passwords-reset-register.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](howto-sspr-authenticationdata.md)
* [Authentifizierungsmethoden](concept-sspr-howitworks.md#authentication-methods)
* [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](concept-sspr-policy.md)
* [Übersicht über die Kennwortrückschreibung](howto-sspr-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](howto-sspr-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](concept-sspr-howitworks.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)
