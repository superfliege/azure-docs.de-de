---
title: Azure Front Door Service – Metriken und Protokollierung | Microsoft-Dokumentation
description: Dieser Artikel bietet einen Überblick über die verschiedenen Metriken und Zugriffsprotokolle, die Azure Front Door Service unterstützt.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: 5d4d591c465cae91c59e8f86ea9d3e421db0f952
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58112108"
---
# <a name="monitoring-metrics-for-front-door"></a>Überwachungsmetriken für Front Door Service

Mit dem Azure Front Door Service können Sie Ressourcen auf die folgenden Arten überwachen:
* [Protokolle:](#diagnostic-logging) Protokolle ermöglichen das Speichern und Nutzen von Leistungs-, Zugriffs- und anderen Daten einer Ressource zu Überwachungszwecken.

* [Metriken:](#metrics) Application Gateway verfügt derzeit über sieben Metriken, um Leistungsindikatoren anzuzeigen.

## <a name="metrics"></a>Metriken

Metriken sind ein Feature für bestimmte Azure-Ressourcen, damit Sie die Leistungsindikatoren im Portal anzeigen können. Für Front Door Service stehen die folgenden Metriken zur Verfügung:

| Metrik | Metrikanzeigename | Unit | Dimensionen | BESCHREIBUNG |
| --- | --- | --- | --- | --- |
| RequestCount | Anzahl der Anforderungen | Count | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Anzahl der von Front Door Service bereitgestellten Clientanforderungen.  |
| RequestSize | Anforderungsgröße | Byte | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Anzahl der von Clients als Anforderungen an Front Door Service gesendeten Bytes. |
| ResponseSize | Antwortgröße | Byte | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Anzahl der von Front Door Service als Antworten an Clients gesendeten Bytes. |
| TotalLatency | Gesamtlatenz | Millisekunden | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Zeitspanne zwischen dem Empfang der Clientanforderung durch Front Door Service und der Bestätigung des letzten Antwortbytes von Front Door Service durch den Client. |
| BackendRequestCount | Back-End-Anforderungsanzahl | Count | HttpStatus</br>HttpStatusGroup</br>Back-End | Die Anzahl der von Front Door Service an Back-Ends gesendeten Anforderungen. |
| BackendRequestLatency | Latenz der Back-End-Anforderung | Millisekunden | Back-End | Die Zeitspanne zwischen dem Senden der Anforderung durch Front Door Service an das Back-End und dem Empfang des letzten Antwortbytes durch Front Door Service vom Back-End. |
| BackendHealthPercentage | Prozentsatz der Back-End-Integrität | Prozent | Back-End</br>BackendPool | Der Prozentsatz der erfolgreichen Integritätstests von Front Door Service zu Back-Ends. |
| WebApplicationFirewallRequestCount | Anforderungsanzahl für die Web Application Firewall | Count | PolicyName</br>RuleName</br>Aktion | Die Anzahl der Clientanforderungen, die von der Anwendungssicherheitsschicht von Front Door Service verarbeitet werden. |

## <a name="activity-log"></a>Aktivitätsprotokolle

Aktivitätsprotokolle geben Einblick in die Vorgänge, die in Ihrer Front Door-Instanz ausgeführt wurden. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) in Ihrer Front Door-Instanz ermitteln.

> [!NOTE]
> Aktivitätsprotokolle enthalten keine Lesevorgänge (GET) oder Vorgänge, die im Azure-Portal oder mithilfe der ursprünglichen Management-APIs durchgeführt wurden.

Sie können in Ihrer Front Door-Instanz auf Aktivitätsprotokolle und in Azure Monitor auf Protokolle für alle Ihre Azure-Ressourcen zugreifen. 

So zeigen Sie Aktivitätsprotokolle an:

1. Wählen Sie Ihre Front Door-Instanz aus.
2. Klicken Sie auf **Aktivitätsprotokoll**.

    ![Aktivitätsprotokoll](./media/front-door-diagnostics/activity-log.png)

3. Wählen Sie den gewünschten Filterungsbereich, und klicken Sie auf **Anwenden**.

## <a name="diagnostic-logging"></a>Diagnoseprotokolle
Diagnoseprotokolle bieten umfassende Informationen zu Vorgängen und Fehlern, die zur Überwachung und Problembehandlung relevant sind. Diagnoseprotokolle unterscheiden sich von Aktivitätsprotokollen. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure-Ressourcen ausgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat. Erfahren Sie mehr zu [Azure Monitor-Diagnoseprotokollen](../azure-monitor/platform/diagnostic-logs-overview.md). 

So konfigurieren Sie Diagnoseprotokolle für Ihre Front Door-Instanz

1. Wählen Sie Ihre APIM-Dienstinstanz aus.
2. Klicken Sie auf **Diagnoseeinstellungen**.

    ![Diagnoseprotokolle](./media/front-door-diagnostics/diagnostic-log.png)

3. Klicken Sie auf **Diagnose aktivieren**. Sie können Diagnoseprotokolle zusammen mit Metriken in einem Speicherkonto archivieren, an einen Event Hub streamen oder an Azure Monitor-Protokolle senden. 

Azure Front Door Service bietet derzeit Diagnoseprotokolle (stündlich erfasst) zu einzelnen API-Anforderungen, bei denen jeder Eintrag das folgende Schema aufweist:

| Eigenschaft  | BESCHREIBUNG |
| ------------- | ------------- |
| ClientIp | Die IP-Adresse des Clients, der die Anforderung gestellt hat. |
| ClientPort | Der IP-Port des Clients, der die Anforderung gestellt hat. |
| HttpMethod | Von der Anforderung verwendete HTTP-Methode |
| HttpStatusCode | Der HTTP-Statuscode, der vom Proxy zurückgegeben wurde. |
| HttpStatusDetails | Resultierender Status in der Anforderung. Informationen zur Bedeutung dieses Zeichenfolgenwerts finden Sie in der Verweistabelle zum Status. |
| HttpVersion | Typ der Anforderung oder Verbindung. |
| RequestBytes | Die Größe der HTTP-Anforderungsnachricht in Byte, einschließlich Anforderungsheader und -text. |
| RequestUri | URI der empfangenen Anforderung |
| ResponseBytes | Die vom Back-End-Server als Antwort gesendeten Bytes.  |
| RoutingRuleName | Der Name der Routingregel, der die Anforderung entspricht. |
| SecurityProtocol | Die TLS-/SSL-Protokollversion, die von der Anforderung verwendet wird, oder NULL, wenn keine Verschlüsselung verwendet wird. |
| TimeTaken | Die Dauer der Aktion in Millisekunden. |
| UserAgent | Der vom Client verwendete Browsertyp |
| TrackingReference | Die eindeutige Verweiszeichenfolge zur Identifizierung einer Anforderung, die von der Front Door-Instanz verarbeitet wird. Diese wird auch als X-Azure-Ref-Header an den Client gesendet. Sie ist für die Suche nach Informationen in den Zugriffsprotokollen für eine bestimmte Anforderung erforderlich. |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
