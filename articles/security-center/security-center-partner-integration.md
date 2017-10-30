---
title: "Integrieren von Sicherheitslösungen in Azure Security Center | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Partner in Azure Security Center integriert werden, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/23/2017
ms.author: yurid
ms.openlocfilehash: 847a872661bea31b774814188c7707260a16e620
ms.sourcegitcommit: 4d90200f49cc60d63015bada2f3fc4445b34d4cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integrieren von Sicherheitslösungen in Azure Security Center
Dieses Dokument unterstützt Sie bei der Verwaltung von bereits mit Azure Security Center verbundenen Sicherheitslösungen sowie beim Hinzufügen neuer Lösungen.

## <a name="integrated-azure-security-solutions"></a>Integrierte Azure-Sicherheitslösungen
Mit Security Center können Sie ganz einfach integrierte Sicherheitslösungen in Azure aktivieren. Dies hat unter anderem folgende Vorteile:

- **Vereinfachte Bereitstellung:** Security Center bietet eine optimierte Bereitstellung integrierter Partnerlösungen. Für Lösungen wie Antischadsoftware und Sicherheitsrisikobewertungen kann Security Center den erforderlichen Agent auf Ihren virtuellen Computern bereitstellen, und für Firewallgeräte kann sich Security Center um den Großteil der erforderlichen Netzwerkkonfiguration kümmern.
- **Integrierte Erkennungen:** Sicherheitsereignisse von Partnerlösungen werden automatisch gesammelt, aggregiert und im Rahmen der Security Center-Warnungen und Vorfälle angezeigt. Diese Ereignisse werden mit Erkennungen anderer Quellen zusammengeführt, um erweiterte Bedrohungserkennungsfunktionen bereitzustellen.
- **Vereinheitlichte Systemüberwachung und -verwaltung:** Dank integrierter Integritätsereignisse können Kunden alle Partnerlösungen auf einen Blick überwachen. Es ist eine grundlegende Verwaltung verfügbar – mit einfachem Zugriff auf die erweiterte Konfiguration über die Partnerlösung.

Integrierte Sicherheitslösungen umfassen derzeit Folgendes:

- Endpoint Protection ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](https://docs.microsoft.com/azure/security/azure-security-antimalware), Windows Defender und System Center Endpoint Protection (SCEP))
- Web Application Firewall ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) und [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Firewall der nächsten Generation ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) und [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html))
- Sicherheitsrisikobewertung ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

## <a name="how-security-solutions-are-integrated"></a>Informationen zur Integration von Sicherheitslösungen
Über Security Center bereitgestellte Azure-Sicherheitslösungen werden automatisch verbunden. Sie können auch eine Verbindung mit anderen Sicherheitsdatenquellen herstellen. Hierzu zählen beispielsweise folgende:

- Azure AD Identity Protection
- Lokal oder in anderen Clouds ausgeführte Computer
- Sicherheitslösung mit CEF-Unterstützung (Common Event Format)
- Microsoft Advanced Threat Analytics

![Integration von Partnerlösungen](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Verwalten integrierter Azure-Sicherheitslösungen und anderer Datenquellen

Nach der Bereitstellung können Sie Informationen zur Integrität integrierter Azure-Sicherheitslösungen anzeigen und grundlegende Verwaltungsaufgaben ausführen. Sie können auch eine Verbindung mit anderen Arten von Sicherheitsdatenquellen herstellen – beispielsweise mit Azure Active Directory Identity Protection-Warnungen und Firewallprotokollen im CEF-Format (Common Event Format). Klicken Sie im Security Center-Dashboard auf „Sicherheitslösungen“.

### <a name="connected-solutions"></a>Verbundene Lösungen

Der Abschnitt **Verbunde Lösungen** enthält Sicherheitslösungen, die derzeit mit Security Center verbunden sind, sowie Informationen zum Integritätsstatus der einzelnen Lösungen.  

![Verbundene Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

### <a name="discovered-solutions"></a>Ermittelte Lösungen

Im Abschnitt mit den **ermittelten Lösungen** werden alle Lösungen angezeigt, die über Azure hinzugefügt wurden. Außerdem enthält er alle Lösungen, für die Security Center eine Verbindungsherstellung vorschlägt.

![Ermittelte Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Andere in Azure ausgeführte Sicherheitslösungen werden von Security Center automatisch ermittelt. Dazu gehören sowohl Azure-Lösungen wie [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) als auch in Azure ausgeführte Partnerlösungen. Klicken Sie auf **VERBINDEN**, um diese Lösungen in Security Center zu integrieren.

### <a name="add-data-sources"></a>Hinzufügen von Datenquellen

Der Abschnitt **Datenquellen hinzufügen** enthält weitere verfügbare Datenquellen, mit denen eine Verbindung hergestellt werden kann. Klicken Sie **HINZUFÜGEN**, um eine Anleitung zum Hinzufügen von Daten aus einer dieser Quellen anzuzeigen.

![Datenquellen](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Partnerlösungen in Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
* [Connecting Microsoft Advanced Threat Analytics to Azure Security Center ](security-center-ata-integration.md) (Verbinden von Microsoft Advanced Threat Analytics mit Azure Security Center)
* [Connecting Azure Active Directory Identity Protection to Azure Security Center](security-center-aadip-integration.md) (Verbinden von Azure Active Directory Identity Protection mit Azure Security Center)
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung von Security Center.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
