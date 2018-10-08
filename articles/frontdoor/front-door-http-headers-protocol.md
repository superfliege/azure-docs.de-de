---
title: Azure Front Door Service – Unterstützung für HTTP-Headerprotokolle | Microsoft-Dokumentation
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
ms.openlocfilehash: 0dcb769627714be9da55faf2a8e82c8750789498
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038849"
---
# <a name="azure-front-door-service---http-headers-protocol-support"></a>Azure Front Door Service – Unterstützung für HTTP-Headerprotokolle
Dieses Dokument beschreibt das Protokoll, das Azure Front Door Service in verschiedenen Teilen des Aufrufpfads unterstützt, wie in der folgenden Abbildung dargestellt. Die folgenden Abschnitte bieten einen besseren Einblick in die HTTP-Header, die von Front Door unterstützt werden.

![HTTP-Headerprotokoll von Azure Front Door Service][1]

>[!WARNING]
>Azure Front Door Service bietet keine Garantien für HTTP-Header, die hier nicht dokumentiert sind.

## <a name="1-client-to-front-door"></a>1. Client zu Front Door
Front Door akzeptiert die meisten Header aus der eingehenden Anforderung (ohne sie zu ändern). Es gibt jedoch einige reservierte Header, die aus der eingehenden Anforderung entfernt werden, wenn sie gesendet werden. Dazu gehören Header mit den folgenden Präfixen:
 - X-FD*
 - X-MS*

## <a name="2-front-door-to-backend"></a>2. Front Door zum Back-End

Front Door nimmt die Header der eingehenden Anforderung auf, es sei denn, sie wurden aufgrund der oben genannten Einschränkungen entfernt. Front Door fügt außerdem die folgenden Header hinzu:

| Header  | Beispiel und Beschreibung |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ==* </br> Dies ist eine eindeutige Referenzzeichenfolge, die eine von Front Door verarbeitete Anforderung identifiziert. Sie ist wichtig für die Problembehandlung, da sie verwendet wird, um die Zugriffsprotokolle zu durchsuchen.|
| X-MS-RequestChain |  *X-MS-RequestChain: hops=1* </br> Dies ist ein Header, den Front Door verwendet, um Anforderungsschleifen zu erkennen, und Benutzer sollten dafür keine Abhängigkeit erstellen. |
| X-MS-Via |  *X-MS-Via: Azure* </br> Dies wird von Front Door hinzugefügt, um anzugeben, dass Azure/Front Door ein zwischengeschalteter Empfänger für die Anforderung zwischen dem Client und dem Back-End war. |

## <a name="3-front-door-to-client"></a>3. Front Door zum Client

Die folgenden Header werden von Front Door an Clients gesendet. Vom Back-End an Front Door gesendete Header werden auch an den Client weitergeleitet.

| Header  | Beispiel |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ==* </br> Dies ist eine eindeutige Referenzzeichenfolge, die eine von Front Door verarbeitete Anforderung identifiziert. Sie ist wichtig für die Problembehandlung, da sie verwendet wird, um die Zugriffsprotokolle zu durchsuchen.|

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png