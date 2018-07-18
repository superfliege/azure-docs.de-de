---
title: Emotionen-API PHP-Schnellstart | Microsoft Docs
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Emotionen-API mit PHP in Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 987d5a3eedaa17f1127be34e5f90ec2456fab99b
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019396"
---
# <a name="emotion-api-php-quick-start"></a>Emotionen-API PHP-Schnellstart

> [!IMPORTANT]
> Die Video-API-Vorschau ist am 30. Oktober 2017 abgelaufen. Testen Sie die neue [Video-Indexer-API (Vorschau)](https://azure.microsoft.com/services/cognitive-services/video-indexer/). Extrahieren Sie Erkenntnisse aus Videos, und verbessern Sie die Auffindbarkeit von Inhalten, z.B. mit Suchergebnissen durch die Erkennung von gesprochenen Wörtern, Gesichtern, Zeichen und Emotionen. [Weitere Informationen](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview)

Dieser Artikel enthält Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der [Recognize-Methode der Emotionen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) mit PHP zum Erkennen der von einer oder mehreren Personen auf einem Bild ausgedrückten Emotionen. 

## <a name="prerequisite"></a>Voraussetzung
* Rufen Sie [hier](https://azure.microsoft.com/try/cognitive-services/) Ihren kostenlosen Abonnementschlüssel ab.

## <a name="recognize-emotions-php-example-request"></a>PHP-Beispielanforderung zur Emotionserkennung

Ändern Sie die REST-URL so, dass der Standort verwendet wird, an dem Sie Ihre Abonnementschlüssel erworben haben, ändern Sie den Textkörper in die URL eines Bilds, das Sie testen möchten, und ersetzen Sie den Wert von „Ocp-Apim-Subscription-Key“ durch Ihren gültigen Abonnementschlüssel.

```php
<?php
// This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

// NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
//   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
//   URL below with "westcentralus".
$request = new Http_Request2('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',

    // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '13hc77781f7e4b19b5fcdd72a8df7156',
);

$request->setHeader($headers);

$parameters = array(
    // Request parameters
);

$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body
$request->setBody('{"url": "http://www.example.com/images/image.jpg"}');

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

## <a name="recognize-emotions-sample-response"></a>Beispielantwort der Emotionserkennung
Ein erfolgreicher Aufruf gibt ein Array von Gesichtseinträgen zusammen mit den ihnen zugeordneten Emotionsbewertungen in der Rangfolge der Rechteckgröße der Gesichter in absteigender Reihenfolge zurück. Eine leere Antwort zeigt an, dass keine Gesichter erkannt wurden. Ein Emotionseintrag enthält die folgenden Felder:
* faceRectangle: Rechteckposition des Gesichts auf dem Bild
* scores: Emotionsbewertungen für jedes Gesicht auf dem Bild 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]

