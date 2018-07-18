---
title: Emotionen-API cURL-Schnellstart | Microsoft Docs
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Emotionen-API mit cURL in Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: a7ca2cac718797462bb4dc889b3f1361b252435e
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37021097"
---
# <a name="emotion-api-curl-quick-start"></a>Emotionen-API cURL-Schnellstart

> [!IMPORTANT]
> Die Video-API-Vorschau ist am 30. Oktober 2017 abgelaufen. Testen Sie die neue [Video-Indexer-API (Vorschau)](https://azure.microsoft.com/services/cognitive-services/video-indexer/). Extrahieren Sie Erkenntnisse aus Videos, und verbessern Sie die Auffindbarkeit von Inhalten, z.B. mit Suchergebnissen durch die Erkennung von gesprochenen Wörtern, Gesichtern, Zeichen und Emotionen. [Weitere Informationen](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview)

Dieser Artikel enthält Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der [Recognize-Methode der Emotionen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) mit cURL zum Erkennen der von einer oder mehreren Personen auf einem Bild ausgedrückten Emotionen. 

## <a name="prerequisite"></a>Voraussetzung
* Rufen Sie [hier](https://azure.microsoft.com/try/cognitive-services/) Ihren kostenlosen Abonnementschlüssel ab.

## <a name="recognize-emotions-curl-example-request"></a>cURL-Beispielanforderung zur Emotionserkennung

> [!NOTE]
> Sie müssen in Ihrem REST-Aufruf den gleichen Standort verwenden, den Sie zum Erwerb Ihrer Abonnementschlüssel verwendet haben. Wenn Sie beispielsweise Ihre Abonnementschlüssel von „westcentralus“ erhalten haben, ersetzen Sie „westus“ in der URL unten durch „westcentralus“.

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}" 
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

