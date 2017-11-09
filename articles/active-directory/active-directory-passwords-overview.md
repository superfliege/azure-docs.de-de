---
title: "Übersicht über die Self-Service-Kennwortzurücksetzung in Azure AD | Microsoft-Dokumentation"
description: "Wie kann Ihre Organisation von der Self-Service-Kennwortzurücksetzung in Azure AD profitieren?"
services: active-directory
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
ms.openlocfilehash: f577ee95c34f9bc3065c026bc943d30622b9d658
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Self-Service-Kennwortzurücksetzung in Azure AD für IT-Experten

Mit der Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) in Azure Active Directory (Azure AD) können Benutzer ihre Kennwörter selbst zurückzusetzen, wann und wo es notwendig ist, wobei Administratoren die Kontrolle über die Art und Weise der Kennwortzurücksetzung behalten. Benutzer müssen nicht mehr den Helpdesk anrufen, um ihre Kennwörter zurückzusetzen.

* **Self-Service-Kennwortänderung**: Der Benutzer kennt sein Kennwort, möchte es aber ändern.
* **Self-Service-Kennwortzurücksetzung**: Der Benutzer kann sich nicht anmelden und möchte sein Kennwort mit einer oder mehreren der folgenden Authentifizierungsmethoden zurücksetzen.
   * SMS an ein überprüftes Mobiltelefon
   * Telefonanruf an ein überprüftes Mobil- oder Bürotelefon
   * E-Mail an ein überprüftes sekundäres E-Mail-Konto
   * Antworten auf Sicherheitsfragen
* **Self-Service-Kontoentsperrung**: Der Benutzer kann sich nicht mit seinem Kennwort anmelden, wurde ausgesperrt und möchte sein Kennwort ohne Eingreifen des Administrators mit seinen Authentifizierungsmethoden entsperren.

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Gründe für die Self-Service-Kennwortzurücksetzung in Azure AD

* **Senken Sie die Kosten**, da die vom Helpdesk unterstützte Kennwortzurücksetzung in der Regel 20 % der Supportanfragen einer IT-Abteilung ausmachen. 
* **Verbessern Sie die Endbenutzererfahrungen**, und **reduzieren Sie den Helpdesk**, indem Sie Endbenutzern die Möglichkeit geben, eigene Probleme mit Kennwörtern sofort zu beheben, ohne den Helpdesk anzurufen oder eine Supportanfrage zu öffnen.
* **Fördern Sie die Mobilität**, da Benutzer ihre Kennwörter unabhängig von ihrem Standort zurücksetzen können.
* **Behalten Sie die Kontrolle** über die Sicherheitsrichtlinie. Administratoren können weiterhin ihre derzeitigen Richtlinien durchsetzen.

Wenn Sie bereit sind, können Sie mithilfe unserer [Schnellstartanleitung](active-directory-passwords-getting-started.md) mit der Nutzung von Azure AD SSPR beginnen und schnell Ihre Benutzer in die Lage versetzen, ihre eigenen Kennwörter zurückzusetzen.

## <a name="azure-ad-self-service-password-reset-availability"></a>Verfügbarkeit der Self-Service-Kennwortzurücksetzung in Azure AD

Die Self-Service-Kennwortzurücksetzung in Azure AD ist in drei Tarifen abhängig von Ihrem Abonnement verfügbar.

* **Azure AD Free**: Cloudadministratoren können ihre eigenen Kennwörter zurücksetzen.
* **Azure AD Basic** oder ein beliebiges **kostenpflichtiges Office 365-Abonnement**: Cloudbenutzer können ihre eigenen Kennwörter zurücksetzen.
* **Azure AD Premium**: Alle Benutzer und alle Administratoren können ihre eigenen Kennwörter zurücksetzen, dazu gehören Cloudbenutzer, Verbundbenutzer und Benutzer mit Kennwortsynchronisierung. Für lokale Kennwörter muss das Kennwortrückschreiben aktiviert sein.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD-Preise, SLA, Updates und Roadmap

Weitere Details zu Lizenzierung, Preisen und Plänen für die Zukunft finden Sie auf den folgenden Websites.

* [**Azure Active Directory – Preise**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Office 365 – Preise**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Azure – Vereinbarungen zum Servicelevel (SLAs)**](https://azure.microsoft.com/support/legal/sla/)
* [**Vereinbarung zum Servicelevel für Microsoft Online Services**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure-Updates**](https://azure.microsoft.com/updates/)
* [**Roadmap für Azure**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Nächste Schritte

* Sind Sie bereit für die ersten Schritte mit SSPR? [Richten Sie die Self-Service-Kennwortzurücksetzung in Azure AD ein](active-directory-passwords-getting-started.md).
* Planen Sie eine erfolgreiche SSPR-Bereitstellung für Ihre Benutzer mithilfe der Anleitungen unter [ **Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung**](active-directory-passwords-best-practices.md).
* [Setzen Sie Ihr Kennwort zurück bzw. ändern Sie es](active-directory-passwords-update-your-own-password.md).
* [Registrieren Sie sich für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md).