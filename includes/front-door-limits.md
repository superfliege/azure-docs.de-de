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
ms.openlocfilehash: f0c2d1501b9aa19dec8c4ad157e004a57e0e5070
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006513"
---
| Ressource | Standardlimit |
| --- | --- |
| Front Door-Ressourcen pro Abonnement | 100 |
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
#### <a name="front-door-to-application-backend"></a>Front Door zum Anwendungs-Back-End
- Wenn es sich bei der Antwort um eine segmentierte Antwort handelt, wird der Status 200 zurückgegeben, falls/wenn der erste Block empfangen wird.
- Nachdem die HTTP-Anforderung an das Back-End weitergeleitet wurde, wartet Front Door 30 Sekunden lang auf das erste Paket vom Back-End, bevor ein Fehler vom Typ 503 an den Client zurückgegeben wird.
- Nachdem das erste Paket vom Back-End empfangen wurde, wartet Front Door 30 Sekunden lang (Leerlauftimeout), bevor ein Fehler vom Typ 503 an den Client zurückgegeben wird.
- Das Timeout für TCP-Sitzungen von Front Door zu Back-End beträgt 30 Minuten.

### <a name="upload--download-data-limit"></a>Datenlimit für Upload/Download

|  | Mit Codierung für segmentierte Übertragung (Chunked Transfer Encoding, CTE) | Ohne HTTP-Segmentierung |
| ---- | ------- | ------- |
| **Download** | Es gibt keine Beschränkung für die Downloadgröße. | Es gibt keine Beschränkung für die Downloadgröße. |
| **Upload** |  Keine Beschränkung, so lange jeder CTE-Upload kleiner als 28,6 MB ist. | Die Größe darf 28,6 MB nicht überschreiten. |
