---
title: Verwenden der API für die Suche nach Anomalien mit cURL – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Abrufen von Informationen für den schnellen Einstieg mit cURL und der API für die Suche nach Anomalien in Cognitive Services
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 9f47a49b41156967e0d4ef1c729a6bbb1cca1770
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456302"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>Verwenden der API für die Suche nach Anomalien mit cURL

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

In diesem Artikel finden Sie Informationen und Codebeispiele, die Ihnen bei der Nutzung der API für die Suche nach Anomalien mit cURL helfen, damit Sie Ergebnisse für Anomalien bei Zeitreihendaten erfassen können.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Abrufen von Anomaliepunkten mithilfe der API für die Suche nach Anomalien mit cURL 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Beispiel für Zeitreihendaten

So sieht das Beispiel für die Datenpunkte der Zeitreihen aus:

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Beispiel zum Analysieren von Daten und Abrufen von Anomaliepunkten mit cURL

Sie können das Beispiel folgendermaßen anwenden:

1. Ersetzen Sie den Wert `[YOUR_SUBSCRIPTION_KEY]` durch Ihren gültigen Abonnementschlüssel.
2. Ersetzen Sie `[YOUR_REGION]` durch die Region, in der Sie Ihren Abonnementschlüssel erhalten haben.
3. Ersetzen Sie `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` durch das Beispiel oder Ihre eigenen Datenpunkte.
4. Führen Sie die Anforderung aus, und überprüfen Sie die Antwort.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Beispielantwort
Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Eine beispielhafte Antwort lautet: [!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
