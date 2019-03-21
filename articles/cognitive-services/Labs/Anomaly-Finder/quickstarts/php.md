---
title: Verwenden der API für die Suche nach Anomalien mit PHP – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der API für die Suche nach Anomalien mit PHP in Cognitive Services
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: f65f389d93a90df1bc16d2228586beead5f96a73
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728742"
---
# <a name="use-the-anomaly-finder-api-with-php"></a>Verwenden der API für die Suche nach Anomalien mit PHP

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

In diesem Artikel finden Sie Informationen und Codebeispiele, die Ihnen bei der Nutzung der API für die Suche nach Anomalien mit PHP helfen, damit Sie Ergebnisse für Anomalien bei Zeitreihendaten erfassen können.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-php"></a>Abrufen von Anomaliepunkten mithilfe der API für die Suche nach Anomalien mit PHP
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Beispiel für Zeitreihendaten
So sieht das Beispiel für die Zeitreihendaten aus:
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-php-example"></a>Beispiel zum Analysieren von Daten und Abrufen von Anomaliepunkten mit PHP
1. Ersetzen Sie den Wert `[YOUR_SUBSCRIPTION_KEY]` durch Ihren gültigen Abonnementschlüssel.
2. Ersetzen Sie `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` durch das Beispiel oder Ihre eigenen Datenpunkte.
3. Führen Sie die Anforderung aus, und überprüfen Sie die Antwort.

```PHP
<?php
# This sample uses the Apache HTTP client from HTTP components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

$request = new HTTP_Request2('https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection');
$url = $request->getUrl();

$requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

$headers = array(
    # Request headers
    'Content-Type' => 'application/json',
    # NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '[YOUR_SUBSCRIPTION_KEY]',
);

$request->setHeader($headers);

$request->setMethod(HTTP_Request2::METHOD_POST);

# Request body
$request->setBody($requestData);

try
{
    $response = $request->send();
    echo $response->getBody();
}
catch (HttpException $ex)
{
    echo $ex;
}
?>
```

### <a name="example-response"></a>Beispielantwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie eine Beispielantwort:
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
