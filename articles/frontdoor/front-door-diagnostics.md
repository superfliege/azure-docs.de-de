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
ms.openlocfilehash: 643ae03a9350868b172d99b2af7ce23e34fedf47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997997"
---
# <a name="monitoring-metrics-for-front-door"></a>Überwachungsmetriken für Front Door Service

Mithilfe von Azure Front Door Service können Sie Ihre wichtigsten Metriken zum Überprüfen der Front Door Service-Konfiguration sowie der Nutzung, Integrität und Leistung von Front Door Service überwachen.

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


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
