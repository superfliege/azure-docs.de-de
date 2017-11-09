---
title: 'Lizenzierung: Azure AD SSPR | Microsoft-Dokumentation'
description: "Lizenzanforderungen für die Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
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
ms.openlocfilehash: db849cd1e9da634064f79fbc041098542580ad02
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung

**In Ihrer Organisation muss mindestens eine Lizenz zugewiesen sein**, damit die Kennwortzurücksetzung in Azure AD funktioniert. Auf der Benutzeroberfläche für das Zurücksetzen von Kennwörtern wird keine Einzelbenutzerlizenzierung erzwungen. Um Konformität mit Ihrer Microsoft-Lizenzvereinbarung zu gewährleisten, müssen Sie allen Benutzern Lizenzen zuweisen, die Premiumfeatures verwenden.

* **Nur Cloudbenutzer**: beliebige kostenpflichtige SKU für Office 365 (O365) oder Azure AD Basic
* **Cloudbenutzer** und/oder **lokale Benutzer**: Azure AD Premium P1 oder P2, Enterprise Mobility + Security (EMS) oder Secure Productive Enterprise (SPE)

## <a name="licenses-required-for-password-writeback"></a>Erforderliche Lizenzen für das Kennwortrückschreiben

Zur Verwendung des Kennwortrückschreibens muss in Ihrem Mandanten eine der folgenden Lizenzen zugewiesen sein.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 E3
* Microsoft 365 E5

> [!WARNING]
> Eigenständige Office 365-Lizenzierungspläne **bieten keine Unterstützung für das Kennwortrückschreiben**. Sie benötigen einen der obigen Pläne, damit diese Funktion verwendet werden kann.

Weitere Informationen zur Lizenzierung, einschließlich der Kosten, finden Sie auf den folgenden Seiten.

* [Website mit Preisen für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-Features und -Funktionen](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Aktivieren der gruppen- oder benutzerbasierten Lizenzierung

Azure AD unterstützt jetzt die gruppenbasierte Lizenzierung, mit der Administratoren in einem Massenvorgang einer Gruppe von Benutzern Lizenzen zuweisen können, anstatt sie einzeln zuzuweisen. [Zuweisen, Prüfen und Beheben von Problemen mit Lizenzen](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft „Verwendungsstandort“ für den Benutzer angeben. Die Zuweisung von Lizenzen erfolgt im Azure-Portal unter „Benutzer“ > „Profil“ > „Einstellungen“. **Bei Verwendung der Gruppenlizenzzuweisung erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses.**

## <a name="next-steps"></a>Nächste Schritte

* [Wie führe ich ein erfolgreiches Rollout der SSPR durch?](active-directory-passwords-best-practices.md)
* [Setzen Sie Ihr Kennwort zurück bzw. ändern Sie es](active-directory-passwords-update-your-own-password.md).
* [Registrieren Sie sich für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md).
* [Welche Daten werden von SSPR verwendet, und welche Daten sollten Sie für Ihre Benutzer angeben?](active-directory-passwords-data.md)
* [Welche Authentifizierungsmethoden sind für Benutzer verfügbar?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Welche Richtlinienoptionen stehen mit SSPR zur Verfügung?](active-directory-passwords-policy.md)
* [Worum handelt es sich beim Rückschreiben von Kennwörtern, und warum sollte ich mir Gedanken darüber machen?](active-directory-passwords-writeback.md)
* [Wie melde ich eine Aktivität bei SSPR?](active-directory-passwords-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)
