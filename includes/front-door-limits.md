---
title: Includedatei
description: Includedatei
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 9/17/2018
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: e3fa5616518675d8475937ec63afdd8e1742e8c6
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553520"
---
| Ressource | Standardlimit |
| --- | --- |
| Azure Front Door Service-Ressourcen pro Abonnement | 100 |
| Front-End-Hosts (einschließlich benutzerdefinierter Domänen) pro Ressource | 100 |
| Routingregeln pro Ressource | 100 |
| Back-End-Pools pro Ressource | 50 |
| Back-Ends pro Back-End-Pool | 100 |
| Abzustimmende Pfadmuster für eine Routingregel | 25 |
| Benutzerdefinierte Web Application Firewall-Regeln pro Richtlinie | 10 |
| Web Application Firewall-Richtlinien pro Ressource | 100 |

### <a name="timeout-values"></a>Timeoutwerte
#### <a name="client-to-front-door"></a>Client zu Front Door
- Front Door verfügt über ein Timeout für eine TCP-Verbindung im Leerlauf von 61 Sekunden.

#### <a name="front-door-to-application-back-end"></a>Front Door zum Anwendungs-Back-End
- Wenn es sich bei der Antwort um eine segmentierte Antwort handelt, wird der Status 200 zurückgegeben, falls/wenn der erste Block empfangen wird.
- Nach der Weiterleitung der HTTP-Anforderung an das Back-End wartet Front Door 30 Sekunden lang auf das erste Paket vom Back-End. Anschließend wird ein 503-Fehler an den Client zurückgegeben.
- Nach dem Empfang des ersten Pakets vom Back-End wartet Front Door 30 Sekunden in einem Leerlauftimeout. Anschließend wird ein 503-Fehler an den Client zurückgegeben.
- Das Timeout für die Front Door-TCP-Sitzung mit dem Back-End beträgt 30 Minuten.

### <a name="upload-and-download-data-limit"></a>Datenlimit beim Hoch- und Herunterladen

|  | Mit Codierung für segmentierte Übertragung (Chunked Transfer Encoding, CTE) | Ohne HTTP-Segmentierung |
| ---- | ------- | ------- |
| **Download** | Es gibt keine Beschränkung für die Downloadgröße. | Es gibt keine Beschränkung für die Downloadgröße. |
| **Upload** |  Es gibt keine Beschränkung, so lange jeder CTE-Upload kleiner als 28,6 MB ist. | Die Größe darf 28,6 MB nicht überschreiten. |
