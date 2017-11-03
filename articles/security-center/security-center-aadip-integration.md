---
title: Verbinden von Azure Active Directory Identity Protection mit Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Security Center in Azure Active Directory Identity Protection integriert wird.
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: 0d4b77c2-dba4-4e46-8f55-ab04ddd92496
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 7562dd5e1c303a6cb97d25bda5aa080bb5643583
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="connecting-azure-active-directory-identity-protection-to-azure-security-center"></a>Verbinden von Azure Active Directory Identity Protection mit Azure Security Center
Dieses Dokument soll Sie bei der Konfiguration der Integration zwischen Azure Active Directory (AD) Identity Protection und Azure Security Center unterstützen.

## <a name="why-connect-azure-ad-identity-protection"></a>Vorteile der Verbindung mit Azure AD Identity Protection
[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) hilft Ihnen dabei, potenzielle Sicherheitsrisiken für die Identitäten Ihrer Organisation zu ermitteln. Wenn Sie verbunden sind, können Sie Warnungen von Azure AD Identity Protection im Security Center sehen. Durch diese Integration können Sie alle Sicherheitswarnungen, die im Zusammenhang mit Ihren hybriden Cloudworkloads im Security Center stehen, ansehen, korrelieren lassen und untersuchen. 

## <a name="how-do-i-configure-this-integration"></a>Vorgehensweise bei der Konfiguration der Integration
Wenn Ihre Organisation bereits Azure AD Identity Protection verwendet, achten Sie auf folgende Schritte bei der Konfiguration der Integration:

1. Öffnen Sie das Dashboard **Security Center**.
2. Klicken Sie im linken Bereich auf **Sicherheitslösungen**. Security Center erkennt automatisch, ob Azure AD Identity Protection für Ihre Organisation aktiviert ist.

    ![AADIP](./media/security-center-aadip-integration/security-center-aadip-integration-fig1.png)

3. Klicken Sie auf **VERBINDEN**.
4. Scrollen Sie auf der Seite **Integrieren von Azure AD Identity Protection** nach unten, und wählen Sie den geeigneten Arbeitsbereich aus:

    ![Arbeitsbereich](./media/security-center-aadip-integration/security-center-aadip-integration-fig2.png)

5. Klicken Sie auf **Verbinden**.

Wenn Sie diese Konfiguration beenden, erscheint die Azure AD Identity Protection-Lösung auf der Seite **Sicherheitslösungen** unter **Verbundene Lösungen**. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument erfahren Sie, wie Sie Azure AD Identity Protection mit Security Center verbinden. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Connecting Microsoft Advanced Threat Analytics to Azure Security Center (Verbinden von Microsoft Advanced Threat Analytics mit Azure Security Center)](security-center-ata-integration.md)
* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md) : Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Azure Security Center-Datensicherheit:](security-center-data-security.md) Hier erfahren Sie, wie Daten in Security Center verwaltet und geschützt werden.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.


