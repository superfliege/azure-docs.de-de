---
title: 'Lizenzierung: Azure AD SSPR | Microsoft-Dokumentation'
description: "Lizenzanforderungen für die Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 45cecbf1334d4d708f1578d1512627bc0ead3a04
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung

In *Ihrer Organisation muss mindestens eine Lizenz zugewiesen sein*, damit die Kennwortzurücksetzung in Azure Active Directory (Azure AD) funktioniert. In der Benutzeroberfläche für das Zurücksetzen von Kennwörtern wird keine Einzelbenutzerlizenzierung erzwungen. Um Konformität mit Ihrer Microsoft-Lizenzvereinbarung zu gewährleisten, müssen Sie allen Benutzern Lizenzen zuweisen, die Premiumfeatures verwenden.

* **Nur Cloudbenutzer**: beliebige kostenpflichtige SKU für Office 365 oder Azure AD Basic
* **Cloudbenutzer** oder **lokale Benutzer**: Azure AD Premium P1 oder P2, Enterprise Mobility + Security (EMS) oder Secure Productive Enterprise (SPE)

## <a name="licenses-required-for-password-writeback"></a>Erforderliche Lizenzen für das Kennwortrückschreiben

Damit Kennwortrückschreiben verwendet werden kann, muss in Ihrem Mandanten eine der folgenden Lizenzen zugewiesen sein:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 (Plan E3)
* Microsoft 365 (Plan E5)

> [!WARNING]
> Eigenständige Office 365-Lizenzierungspläne *bieten keine Unterstützung für das Kennwortrückschreiben* und erfordern, dass Sie einen der obigen Pläne haben, damit diese Funktion verwendbar ist.

Weitere Informationen zur Lizenzierung, einschließlich der Kosten, finden Sie auf den folgenden Seiten:

* [Website mit Preisen für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-Features und -Funktionen](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Aktivieren der gruppen- oder benutzerbasierten Lizenzierung

Azure AD unterstützt jetzt gruppenbasierte Lizenzierung. Administratoren können einer Gruppe von Benutzern Lizenzen in einem Massenvorgang zuweisen, anstatt sie einzeln zuzuweisen. Weitere Informationen hierzu finden Sie unter [Zuweisen, Prüfen und Beheben von Problemen mit Lizenzen](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses).

Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft **Nutzungsspeicherort** für den Benutzer angeben. Die Zuweisung von Lizenzen erfolgt im Azure-Portal im Abschnitt **Benutzer** > **Profil** > **Einstellungen**. *Wenn Sie eine Gruppenlizenzzuweisung verwenden, erben alle Benutzer ohne Nutzungsspeicherort den Speicherort des Verzeichnisses.*

## <a name="next-steps"></a>Nächste Schritte

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](active-directory-passwords-best-practices.md)
* [Zurücksetzen oder Ändern des Kennworts](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](active-directory-passwords-data.md)
* [Authentifizierungsmethoden](active-directory-passwords-how-it-works.md#authentication-methods)
* [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](active-directory-passwords-policy.md)
* [Übersicht über die Kennwortrückschreibung](active-directory-passwords-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](active-directory-passwords-reporting.md)
* [Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

