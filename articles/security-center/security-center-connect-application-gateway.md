---
title: Verbinden von Microsoft Azure Application Gateway mit Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Application Gateway und Azure Security Center integrieren, um die Sicherheit Ihrer Ressourcen insgesamt zu verbessern.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: 5638b71147592ae71c741ca86da68ddfec668af5
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299065"
---
# <a name="connecting-microsoft-azure-application-gateway-to-azure-security-center"></a>Verbinden von Microsoft Azure Application Gateway mit Azure Security Center
Dieses Dokument unterstützt Sie dabei, die Integration in Application Gateway Web Application Firewall (WAF) und Security Center zu konfigurieren.

## <a name="why-connect-application-gateway"></a>Gute Gründe für die Verbindung mit Application Gateway
WAF in Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Befehlen, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen. Security Center wird in Application Gateway integriert, um Bedrohungen für nicht geschützte Webanwendungen in Ihrer Umgebung zu verhindern und zu erkennen.

## <a name="how-do-i-configure-this-integration"></a>Vorgehensweise bei der Konfiguration der Integration
Security Center ermittelt zuvor bereitgestellte WAF-Instanzen im Abonnement. Verbinden Sie diese Lösungen mit Security Center, um die Integration von Warnungen zuzulassen.

> [!NOTE]
> Die Application Gateway WAF kann über die **Empfehlungen** des Security Centers bereitgestellt werden, wie unter [Hinzufügen einer Web Application Firewall](security-center-add-web-application-firewall.md) beschrieben.
>
>

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.

2. Wählen Sie im **Microsoft Azure-Menü** die Option **Security Center**.

3. Wählen Sie unter **RESSOURCENSICHERHEIT** die Option **Sicherheitslösungen** aus.

  ![Übersicht über Security Center](./media/security-center-connect-application-gateway/overview.png)

4. Suchen Sie unter **Ermittelte Lösungen** den Eintrag „Microsoft SaaS-basierte Web Application Firewall“, und wählen Sie **VERBINDEN**.

  ![Ermittelte Lösungen](./media/security-center-connect-application-gateway/connect.png)

5. **WAF-Lösung verknüpfen** wird geöffnet.  Wählen Sie **Verbinden**, um WAF und Security Center zu integrieren.

  ![WAF-Lösung verknüpfen](./media/security-center-connect-application-gateway/waf-solution.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Application Gateway WAF in Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Integrieren von Sicherheitslösungen in Azure Security Center](security-center-partner-integration.md)
* [Verbinden von Microsoft Advanced Threat Analytics mit Azure Security Center](security-center-ata-integration.md)
* [Verbinden von Azure Active Directory Identity Protection mit Azure Security Center](security-center-aadip-integration.md)
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md):
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md):
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md):
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/):
