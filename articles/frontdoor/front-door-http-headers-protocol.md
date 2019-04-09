---
title: Azure Front Door Service – Unterstützung für HTTP-Header | Microsoft-Dokumentation
description: Dieser Artikel hilft Ihnen, sich mit den von Front Door unterstützten HTTP-Headerprotokollen vertraut zu machen.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 40bfdfc3837da12f62864433508482a65def291c
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407316"
---
# <a name="azure-front-door-service---http-headers-support"></a>Unterstützung für HTTP-Header von Azure Front Door Service
Dieses Dokument beschreibt das Protokoll, das Azure Front Door Service in verschiedenen Teilen des Aufrufpfads unterstützt, wie in der folgenden Abbildung dargestellt. Die folgenden Abschnitte bieten einen besseren Einblick in die HTTP-Header, die von Front Door unterstützt werden.

![HTTP-Headerprotokoll von Azure Front Door Service][1]

>[!WARNING]
>Azure Front Door Service bietet keine Garantien für HTTP-Header, die hier nicht dokumentiert sind.

## <a name="1-client-to-front-door"></a>1. Client zu Front Door
Front Door akzeptiert die meisten Header aus der eingehenden Anforderung (ohne sie zu ändern). Es gibt jedoch einige reservierte Header, die aus der eingehenden Anforderung entfernt werden, wenn sie gesendet werden. Dazu gehören Header mit den folgenden Präfixen:
 - X-FD-*

## <a name="2-front-door-to-backend"></a>2. Front Door zum Back-End

Front Door nimmt die Header der eingehenden Anforderung auf, es sei denn, sie wurden aufgrund der oben genannten Einschränkungen entfernt. Front Door fügt außerdem die folgenden Header hinzu:

| Header  | Beispiel und Beschreibung |
| ------------- | ------------- |
| Via |  *Via: 1.1 Azure* </br> Front Door fügt die HTTP-Version des Clients, gefolgt von „Azure“, als Wert für den Via-Header hinzu. Dies wird hinzugefügt, um die HTTP-Version des Clients und Azure Front Door als zwischengeschalteten Empfänger für die Anforderung zwischen dem Client und dem Back-End anzugeben.  |
| X-Azure-ClientIP | *X-Azure-ClientIP: 127.0.0.1* </br> Stellt die IP-Adresse des „Clients“ dar, der mit der zu verarbeitenden Anforderung verknüpft ist. Beispiel: Eine Anforderung von einem Proxy wird möglicherweise dem X-Forwarded-For-Header hinzugefügt, um die IP-Adresse des ursprünglichen Aufrufers anzugeben. |
| X-Azure-SocketIP | *X-Azure-SocketIP: 127.0.0.1* </br> Stellt die Socket-IP-Adresse dar, die mit der TCP-Verbindung verknüpft ist, von der die aktuelle Anforderung stammt. Die Client-IP-Adresse einer Anforderung entspricht möglicherweise nicht der Socket-IP-Adresse, da sie durch einen Endbenutzer willkürlich überschrieben werden kann.|
| X-Azure-Ref | *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Dies ist eine eindeutige Referenzzeichenfolge, die eine von Front Door verarbeitete Anforderung identifiziert. Sie ist wichtig für die Problembehandlung, da sie verwendet wird, um die Zugriffsprotokolle zu durchsuchen.|
| X-Azure-RequestChain |  *X-Azure-RequestChain: hops=1* </br> Dies ist ein Header, den Front Door verwendet, um Anforderungsschleifen zu erkennen, und Benutzer sollten dafür keine Abhängigkeit erstellen. |
| X-Forwarded-For | *X-Forwarded-For: 127.0.0.1* </br> Das HTTP-Headerfeld „X-Forwarded-For (XFF)“ ist eine gängige Methode zum Identifizieren der Ursprungs-IP-Adresse eines Clients, der über einen HTTP-Proxy oder Lastverteiler eine Verbindung zu einem Webserver herstellt. Falls es einen vorhandenen XFF-Header gibt, fügt Front Door die Socket-IP-Adresse des Clients daran an. Andernfalls fügt Front Door den XFF-Header mit der Socket-IP-Adresse des Clients hinzu. |
| X-Forwarded-Host | *X-Forwarded-Host: contoso.azurefd.net* </br> Das HTTP-Headerfeld „X-Forwarded-Host“ ist eine gängige Methode zum Identifizieren des ursprünglichen Hosts, der vom Client im HTTP-Host-Anforderungsheader angefordert wird, da sich der Hostname von Front Door von dem des Backendservers, der die Anforderung verarbeitet, unterscheiden kann. |
| X-Forwarded-Proto | *X-Forwarded-Proto: http* </br> Das HTTP-Headerfeld „X-Forwarded-Proto“ ist eine gängige Methode zum Identifizieren des ursprünglichen Protokolls einer HTTP-Anforderung, da Front Door je nach Konfiguration möglicherweise mit dem Back-End über HTTPS kommuniziert, auch wenn an den Reverseproxy eine HTTP-Anforderung gerichtet wird. |

## <a name="3-front-door-to-client"></a>3. Front Door zum Client

Die folgenden Header werden von Front Door an Clients gesendet. Vom Back-End an Front Door gesendete Header werden auch an den Client weitergeleitet.

| Header  | Beispiel |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Dies ist eine eindeutige Referenzzeichenfolge, die eine von Front Door verarbeitete Anforderung identifiziert. Sie ist wichtig für die Problembehandlung, da sie verwendet wird, um die Zugriffsprotokolle zu durchsuchen.|

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png