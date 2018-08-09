---
title: Integrieren von Sicherheitslösungen in Azure Security Center | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Partner in Azure Security Center integriert werden, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2018
ms.author: terrylan
ms.openlocfilehash: b0e674eb161af41a848f0456a033d615293a9947
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622788"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integrieren von Sicherheitslösungen in Azure Security Center
Dieses Dokument unterstützt Sie bei der Verwaltung von bereits mit Azure Security Center verbundenen Sicherheitslösungen sowie beim Hinzufügen neuer Lösungen.

## <a name="integrated-azure-security-solutions"></a>Integrierte Azure-Sicherheitslösungen
Mit Security Center können Sie ganz einfach integrierte Sicherheitslösungen in Azure aktivieren. Dies hat unter anderem folgende Vorteile:

- **Vereinfachte Bereitstellung:** Security Center bietet eine optimierte Bereitstellung integrierter Partnerlösungen. Für Lösungen wie Antischadsoftware und Sicherheitsrisikobewertungen kann Security Center den erforderlichen Agent auf Ihren virtuellen Computern bereitstellen, und für Firewallgeräte kann sich Security Center um den Großteil der erforderlichen Netzwerkkonfiguration kümmern.
- **Integrierte Erkennungen:** Sicherheitsereignisse von Partnerlösungen werden automatisch gesammelt, aggregiert und im Rahmen der Security Center-Warnungen und Vorfälle angezeigt. Diese Ereignisse werden mit Erkennungen anderer Quellen zusammengeführt, um erweiterte Bedrohungserkennungsfunktionen bereitzustellen.
- **Vereinheitlichte Systemüberwachung und -verwaltung:** Dank integrierter Integritätsereignisse können Kunden alle Partnerlösungen auf einen Blick überwachen. Es ist eine grundlegende Verwaltung verfügbar – mit einfachem Zugriff auf die erweiterte Konfiguration über die Partnerlösung.

Integrierte Sicherheitslösungen umfassen derzeit Folgendes:

- Endpoint Protection ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), [Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://www.microsoft.com/windows/comprehensive-security) und [System Center Endpoint Protection](https://docs.microsoft.com/sccm/protect/deploy-use/endpoint-protection))
- Web Application Firewall ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/products.html) und [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Firewall der nächsten Generation ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2), [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html) und [Palo Alto Networks](https://www.paloaltonetworks.com/products))
- Sicherheitsrisikobewertung ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) und [Rapid7](https://www.rapid7.com/products/insightvm/))

Die Endpoint Protection-Integrationsumgebung kann je nach Lösung variieren. Die folgende Tabelle enthält weitere Details zu den Umgebungen der einzelnen Lösungen:

| Endpoint Protection               | Plattformen                             | Security Center-Installation | Security Center-Ermittlung |
|-----------------------------------|---------------------------------------|------------------------------|---------------------------|
| Windows Defender (Microsoft Antimalware)                  | Windows Server 2016                   | Nein, in Betriebssystem integriert           | JA                       |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 | Per Erweiterung                | JA                       |
| Trend Micro – alle Versionen         | Windows Server-Familie                 | Nein                            | JA                       |
| Symantec v12.1.1100+              | Windows Server-Familie                 | Nein                            | JA                       |
| McAfee v10+                       | Windows Server-Familie                 | Nein                            | JA                       |
| Kaspersky                         | Windows Server-Familie                 | Nein                            | Nein                         |
| Sophos                            | Windows Server-Familie                 | Nein                            | Nein                         |



## <a name="how-security-solutions-are-integrated"></a>Informationen zur Integration von Sicherheitslösungen
Über Security Center bereitgestellte Azure-Sicherheitslösungen werden automatisch verbunden. Sie können auch eine Verbindung mit anderen Sicherheitsdatenquellen herstellen. Hierzu zählen beispielsweise folgende:

- Azure AD Identity Protection
- Lokal oder in anderen Clouds ausgeführte Computer
- Sicherheitslösung mit CEF-Unterstützung (Common Event Format)
- Microsoft Advanced Threat Analytics

![Integration von Partnerlösungen](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Verwalten integrierter Azure-Sicherheitslösungen und anderer Datenquellen

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.

2. Wählen Sie im **Microsoft Azure-Menü** die Option **Security Center**. **Security Center – Übersicht** wird geöffnet.

  ![Übersicht über Security Center](./media/security-center-partner-integration/overview.png)

3. Wählen Sie unter **Übersicht** die Option **Sicherheitslösungen**.

Unter **Sicherheitslösungen** können Sie Informationen zur Integrität integrierter Azure-Sicherheitslösungen anzeigen und grundlegende Verwaltungsaufgaben durchführen. Sie können auch eine Verbindung mit anderen Arten von Sicherheitsdatenquellen herstellen – beispielsweise mit Azure Active Directory Identity Protection-Warnungen und Firewallprotokollen im CEF-Format (Common Event Format).

### <a name="connected-solutions"></a>Verbundene Lösungen

Der Abschnitt **Verbunde Lösungen** enthält Sicherheitslösungen, die derzeit mit Security Center verbunden sind, sowie Informationen zum Integritätsstatus der einzelnen Lösungen.  

![Verbundene Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Weitere Informationen finden Sie unter [Verwalten von verbundenen Partnerlösungen in Azure Security Center](security-center-partner-solutions.md).

### <a name="discovered-solutions"></a>Ermittelte Lösungen

Security Center erkennt Sicherheitslösungen automatisch, die in Azure ausgeführt werden, aber nicht mit Security Center verbunden sind, und zeigt die Lösungen im Abschnitt **Ermittelte Lösungen** an. Dazu gehören sowohl Azure-Lösungen wie [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) als auch Partnerlösungen.

> [!NOTE]
> Für das ermittelte Lösungsfeature ist der Standardtarif von Security Center ist auf Abonnementebene erforderlich. Weitere Informationen zu den Tarifen von Security finden Sie unter [Preise](security-center-pricing.md).
>
>

Wählen Sie unterhalb einer Lösung die Option **VERBINDEN**, um die Integration in Security Center durchzuführen und über Sicherheitswarnungen benachrichtigt zu werden.

![Ermittelte Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Security Center erkennt auch im Abonnement bereitgestellte Lösungen, die die Weiterleitung von CEF-Protokollen (Common Event Format) ermöglichen. Informieren Sie sich über das [Verbinden einer Sicherheitslösung](quick-security-solutions.md) mit Security Center, für die CEF-Protokolle verwendet werden.

### <a name="add-data-sources"></a>Hinzufügen von Datenquellen

Der Abschnitt **Datenquellen hinzufügen** enthält weitere verfügbare Datenquellen, mit denen eine Verbindung hergestellt werden kann. Klicken Sie **HINZUFÜGEN**, um eine Anleitung zum Hinzufügen von Daten aus einer dieser Quellen anzuzeigen.

![Datenquellen](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Partnerlösungen in Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Connecting Microsoft Advanced Threat Analytics to Azure Security Center (Verbinden von Microsoft Advanced Threat Analytics mit Azure Security Center)](security-center-ata-integration.md)
* [Connecting Azure Active Directory Identity Protection to Azure Security Center (Verbinden von Azure Active Directory Identity Protection mit Azure Security Center)](security-center-aadip-integration.md)
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung von Security Center.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
