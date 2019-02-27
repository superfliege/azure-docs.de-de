---
title: Azure AD Connect Health – Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“ | Microsoft Docs
description: In diesem Dokument wird beschrieben, warum die Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“ angezeigt wird und welche Schritte zur Problembehandlung Sie ausführen können.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: SamuelD
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ad829b976d8b712ee8027c89fb618c6c07de1bc
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429010"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“

## <a name="overview"></a>Übersicht
Die Agents auf den lokalen Computern, die Azure AD Connect Health überwacht, laden regelmäßig Daten in den Azure AD Connect Health-Dienst hoch. Wenn der Dienst keine Daten von einem Agent erhält, sind die Informationen im Portal veraltet. Um auf dieses Problem aufmerksam zu machen, löst der Dienst die Warnung **Daten des Integritätsdiensts sind nicht aktuell** aus. Diese Meldung wird generiert, wenn der Dienst in den letzten zwei Stunden keine Daten empfangen hat.  

* Die Statusbenachrichtigung **Warnung** wird ausgelöst, wenn Connect Health zwei Stunden lang keine partiellen Datenelemente empfängt, die vom Server gesendet wurden. Die Statusbenachrichtigung „Warnung“ löst keine E-Mail-Benachrichtigungen an den Mandantenadministrator aus.
* Die Statusbenachrichtigung **Fehler** wird ausgelöst, wenn Connect Health zwei Stunden lang keine vom Server gesendeten Datenelemente empfängt. Die Statusbenachrichtigung „Fehler“ löst E-Mail-Benachrichtigungen an den Mandantenadministrator aus.


## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung 

> [!IMPORTANT] 
> Diese Warnung folgt der [Datenaufbewahrungsrichtlinie](reference-connect-health-user-privacy.md#data-retention-policy) von Connect Health.

* Stellen Sie sicher, dass die Azure AD Connect Health-Agentdienste auf dem Computer ausgeführt werden. Beispiel: Connect Health für AD FS sollte über drei Dienste verfügen.  
  ![Überprüfen von Azure AD Connect Health](./media/how-to-connect-health-agent-install/install5.png)

* Lesen Sie unbedingt den [Abschnitt „Anforderungen“](how-to-connect-health-agent-install.md#requirements), und stellen Sie sicher, dass Sie die Anforderungen erfüllen.
* Verwenden Sie das [Tool zum Testen der Konnektivität](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service), um Konnektivitätsprobleme zu ermitteln.
* Wenn Sie einen HTTP-Proxy haben, führen Sie die folgenden [Konfigurationsschritte](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy) aus. 

Auf dem Blatt „Warnungsdetails“ werden die fehlenden Datenelemente von einem Server aufgelistet. In der folgende Tabelle können Sie das Problem weiter eingrenzen. 
### <a name="connect-health-for-sync"></a>Connect Health für Synchronisierung

| Datenelemente | Schritte zur Problembehandlung |
| --- | --- | 
| PerfCounter | - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-Überprüfung für ausgehenden Datenverkehr ist gefiltert oder deaktiviert](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
| AadSyncService-SynchronizationRules, <br /> AadSyncService-Connectors, <br /> AadSyncService-GlobalConfigurations, <br /> AadSyncService-RunProfileResults, <br /> AadSyncService-ServiceConfigurations, <br /> AadSyncService-ServiceStatus | - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>Connect Health für AD FS

Sie können zusätzliche Schritte zur Überprüfung von AD FS durchführen und den Workflow in der [AD FS-Hilfe](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282) ausführen.

| Datenelemente | Schritte zur Problembehandlung |
| --- | --- | 
| PerfCounter, TestResult | - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-Überprüfung für ausgehenden Datenverkehr ist gefiltert oder deaktiviert](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
|  Adfs-UsageMetrics | Informationen zu ausgehender Konnektivität basierend auf IP-Adressen finden Sie unter [Azure-IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653). | 

### <a name="connect-health-for-adds"></a>Connect Health für AD DS

| Datenelemente | Schritte zur Problembehandlung |
| --- | --- | 
| PerfCounter, Adds-TopologyInfo-Json, Common-TestData-Json | - [Ausgehende Konnektivität mit dem Azure-Dienstendpunkt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Firewallports auf dem Server mit dem Agent](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |


## <a name="next-steps"></a>Nächste Schritte
* [Azure AD Connect Health – FAQ](reference-connect-health-faq.md)
