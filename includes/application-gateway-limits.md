---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 44f9400a1ecebd3c204ec0b891c5d7fe01bdfd25
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51628138"
---
| Ressource | Standardlimit | Hinweis |
| --- | --- | --- |
| Application Gateway |1.000 pro Abonnement | |
| Front-End-IP-Konfiguration |2 |1 öffentliche und 1 private |
| Front-End-Ports |40 | |
| Back-End-Adresspools |40 | |
| Back-End-Server pro Pool |1200 | |
| HTTP-Listener |40 | |
| HTTP-Lastenausgleichsregeln |400 |# der HTTP-Listener * n |
| Back-End-HTTP-Einstellungen |40 | |
| Instanzen pro Gateway |75 | |
| SSL-Zertifikate |40 |1 pro HTTP-Listener |
| Authentifizierungszertifikate |40 | |
| Anforderungstimeout min. |1 Sekunde | |
| Anforderungstimeout max. |24 Stunden | |
| Anzahl der Websites |20 |1 pro HTTP-Listener |
| URL-Zuordnungen pro Listener |1 | |
| Maximale Anzahl pfadbasierter Regeln pro URL-Zuordnung|100|
| Umleitungskonfiguration |40| |
| Gleichzeitige WebSocket-Verbindungen |5.000| |
|Maximale URL-Länge|8.000|
| Maximale Dateiuploadgröße (Standard) |2 GB | |
| Maximale Dateiuploadgröße (WAF) |Mittlere WAF-Gateways – 100 MB<br>Große WAF-Gateways – 500 MB| |
|WAF-Textgrößenbeschränkung (ohne Dateien)|128 KB|
