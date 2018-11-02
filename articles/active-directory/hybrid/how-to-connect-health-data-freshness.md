---
title: Azure AD Connect Health – Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“ | Microsoft Docs
description: In diesem Dokument wird beschrieben, warum die Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“ angezeigt wird und welche Schritte zur Problembehandlung Sie ausführen können.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: ca9f23703315424fcf08350ae3111a20dd94c160
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233224"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“

## <a name="overview"></a>Übersicht
<li>Azure AD Connect Health generiert diese Warnung über Daten, wenn Connect Health zwei Stunden lang nicht alle Datenpunkte vom Server empfangen konnte. Der Titel der Warnung lautet **Die Daten des Integritätsdiensts sind nicht aktuell**. </li>
<li>Die Statusbenachrichtigung **Warnung** wird ausgelöst, wenn Connect Health zwei Stunden lang keine partiellen Datenelemente empfängt, die vom Server gesendet wurden. Die Statusbenachrichtigung „Warnung“ löst keine E-Mail-Benachrichtigungen an den Mandantenadministrator aus. </li>
<li>Die Statusbenachrichtigung **Fehler** wird ausgelöst, wenn Connect Health zwei Stunden lang keine vom Server gesendeten Datenelemente empfängt. Die Statusbenachrichtigung „Fehler“ löst E-Mail-Benachrichtigungen an den Mandantenadministrator aus. </li>

>[!IMPORTANT] 
> Diese Warnung folgt der [Datenaufbewahrungsrichtlinie](reference-connect-health-user-privacy.md#data-retention-policy) von Connect Health.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung 
* Lesen Sie unbedingt den [Abschnitt „Anforderungen“](how-to-connect-health-agent-install.md#requirements), und stellen Sie sicher, dass Sie die Anforderungen erfüllen.
* Verwenden Sie das [Tool zum Testen der Konnektivität](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service), um Konnektivitätsprobleme zu ermitteln.
* Wenn Sie HTTP-Proxys besitzen, führen Sie [diese Konfigurationsschritte](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy) durch. 

### <a name="connect-health-for-sync"></a>Connect Health für Synchronisierung

| Datenelemente | Schritte zur Problembehandlung |
| --- | --- | 
| PerfCounter | - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-Überprüfung für ausgehenden Datenverkehr ist gefiltert oder deaktiviert](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Zulassen der festgelegten Websites, wenn die verstärkte IE-Sicherheit aktiviert ist](https://technet.microsoft.com/en-us/windows/ms537180(v=vs.60)) |
| AadSyncService-SynchronizationRules, <br /> AadSyncService-Connectors, <br /> AadSyncService-GlobalConfigurations, <br /> AadSyncService-RunProfileResults, <br /> AadSyncService-ServiceConfigurations, <br /> AadSyncService-ServiceStatus | – Informationen zu ausgehender Konnektivität basierend auf IP-Adressen finden Sie unter [Azure-IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653). <br /> - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>Connect Health für AD FS

Sie können zusätzliche Schritte zur Überprüfung von AD FS durchführen und den Workflow in der [AD FS-Hilfe](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282) ausführen.

| Datenelemente | Schritte zur Problembehandlung |
| --- | --- | 
| PerfCounter, TestResult | - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-Überprüfung für ausgehenden Datenverkehr ist gefiltert oder deaktiviert](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Zulassen der festgelegten Websites, wenn die verstärkte IE-Sicherheit aktiviert ist](https://technet.microsoft.com/en-us/windows/ms537180(v=vs.60)) |
|  Adfs-UsageMetrics | Informationen zu ausgehender Konnektivität basierend auf IP-Adressen finden Sie unter [Azure-IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653). | 

### <a name="connect-health-for-adds"></a>Connect Health für AD DS

| Datenelemente | Schritte zur Problembehandlung |
| --- | --- | 
| PerfCounter, Adds-TopologyInfo-Json, Common-TestData-Json | - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> - [SSL-Überprüfung für ausgehenden Datenverkehr ist gefiltert oder deaktiviert](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [Zulassen der festgelegten Websites, wenn die verstärkte IE-Sicherheit aktiviert ist](https://technet.microsoft.com/en-us/windows/ms537180(v=vs.60)) <br />  – Informationen zu ausgehender Konnektivität basierend auf IP-Adressen finden Sie unter [Azure-IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653).  |


## <a name="next-steps"></a>Nächste Schritte
* [Azure AD Connect Health – FAQ](reference-connect-health-faq.md)
