---
title: Übersicht über die Self-Service-Kennwortzurücksetzung in Azure AD | Microsoft-Dokumentation
description: Wie kann Ihre Organisation von der Self-Service-Kennwortzurücksetzung in Azure AD profitieren?
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 810c585e104f6b7bc63222594375527c45c6082d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Self-Service-Kennwortzurücksetzung in Azure AD für IT-Experten

Mit der Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) in Azure Active Directory (Azure AD) können Benutzer ihre Kennwörter selbst zurückzusetzen, wann und wo sie es für nötig halten. Gleichzeitig können Administratoren steuern, wie das Kennwort eines Benutzers zurückgesetzt wird. Benutzer müssen keinen Helpdesk mehr anrufen, um ihre Kennwörter zurückzusetzen. Azure AD-SSPR umfasst:

* **Self-Service-Kennwortänderung**: Der Benutzer kennt sein Kennwort, möchte es aber ändern.
* **Self-Service-Kennwortzurücksetzung**: Der Benutzer kann sich nicht anmelden und möchte sein Kennwort zurücksetzen, indem er eine oder mehrere der folgenden Authentifizierungsmethoden nutzt:
   * Senden einer SMS an ein überprüftes Mobiltelefon
   * Vornehmen eines Telefonanrufs an ein überprüftes Mobil- oder Bürotelefon
   * Senden einer E-Mail an ein überprüftes sekundäres E-Mail-Konto
   * Beantworten seiner Sicherheitsfragen
* **Self-Service-Kontoentsperrung**: Der Benutzer kann sich nicht mit seinem Kennwort anmelden und wurde gesperrt. Der Benutzer möchte sein Kennwort ohne Eingreifen des Administrators entsperren, indem er seine Authentifizierungsmethoden verwendet.

## <a name="why-choose-azure-ad-sspr"></a>Was spricht für Azure AD SSPR?

Azure AD SSPR ermöglicht Ihnen Folgendes:

* **Senken Sie die Kosten**, da die von einem Helpdesk unterstützte Kennwortzurücksetzung in der Regel 20 % der Supportanfragen einer IT-Abteilung ausmachen. 
* **Verbessern Sie die Endbenutzererfahrungen**, und **verringern Sie den Helpdeskbedarf**, weil Endbenutzer die Möglichkeit erhalten, ihre eigenen Kennwortprobleme zu beheben. Es gibt keine Notwendigkeit, einen Helpdesk anzurufen oder eine Supportanfrage zu stellen.
* **Fördern Sie die Mobilität**, da Benutzer ihre Kennwörter unabhängig von ihrem Standort zurücksetzen können.
* **Behalten Sie die Kontrolle** über die Sicherheitsrichtlinie. Administratoren können weiterhin ihre derzeitigen Richtlinien durchsetzen.

Wenn Sie bereit sind, können Sie mithilfe unserer [Schnellstartanleitung](../active-directory-passwords-getting-started.md) mit der Nutzung von Azure AD SSPR beginnen. Sie können Ihre Benutzer schnell in die Lage versetzen, ihre eigenen Kennwörter zurückzusetzen.

## <a name="azure-ad-sspr-availability"></a>Verfügbarkeit von Azure AD SSPR

Die Azure AD SSPR ist abhängig von Ihrem Abonnement in drei Tarifen verfügbar:

* **Azure AD Free**: Cloudadministratoren können ihre eigenen Kennwörter zurücksetzen.
* **Azure AD Basic** oder ein beliebiges **kostenpflichtiges Office 365-Abonnement**: Cloudbenutzer können ihre eigenen Kennwörter zurücksetzen.
* **Azure AD Premium**: Alle Benutzer und alle Administratoren können ihre eigenen Kennwörter zurücksetzen, dazu gehören Cloudbenutzer, Verbundbenutzer und Benutzer mit Kennwortsynchronisierung. Für lokale Kennwörter muss das Kennwortrückschreiben aktiviert sein.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD-Preise, SLA, Updates und Roadmap

Weitere Details zu Lizenzierung, Preisen und Plänen für die Zukunft finden Sie auf den folgenden Websites:

* [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/)
* [Office 365 – Preise](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Azure – Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/)
* [Vereinbarung zum Servicelevel für Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Azure-Updates](https://azure.microsoft.com/updates/)
* [Azure-Roadmap](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Nächste Schritte

* Sind Sie bereit für die ersten Schritte mit SSPR? [Einrichten von Self-Service-Kennwortzurücksetzung in Azure AD](../active-directory-passwords-getting-started.md).
* Planen Sie eine erfolgreiche SSPR-Bereitstellung für Ihre Benutzer, indem Sie die Anleitungen unter [Erfolgreicher Rollout der Self-Service-Kennwortzurücksetzung](howto-sspr-deployment.md) verwenden.
* [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](../active-directory-passwords-update-your-own-password.md)
* [Registrieren Sie sich für die Self-Service-Kennwortzurücksetzung](../active-directory-passwords-reset-register.md).
