---
title: cURL-Schnellstart zum Analysieren von lokalen Bildern mit der Maschinelles Sehen-API | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: In dieser Schnellstartanleitung analysieren Sie ein lokales Bild, indem Sie die Maschinelles Sehen-API mit cURL in Cognitive Services verwenden.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 93ca3ea6eee3743dfd0c25c9514375ae63a531ee
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770380"
---
# <a name="quickstart-analyze-a-local-image---rest-curl"></a>Schnellstart: Analysieren eines lokalen Bilds – REST, cURL

In dieser Schnellstartanleitung analysieren Sie mithilfe der Maschinelles Sehen-API ein lokales Bild, um visuelle Merkmale zu extrahieren. Informationen zur Analyse eines Remotebilds finden Sie unter [Analyze a remote image with cURL](curl-analyze.md) (Analysieren eines Remotebilds mit cURL).

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).

## <a name="analyze-a-local-image"></a>Analysieren von lokalen Bildern

Dieses Beispiel ähnelt [Analyze a remote image with cURL](curl-analyze.md) (Analysieren eines Remotebilds mit cURL), aber das zu analysierende Bild wird lokal vom Datenträger gelesen. Drei Änderungen sind erforderlich:

- Ändern Sie den Inhaltstyp (Content-Type) in `"Content-Type: application/octet-stream"`.
- Ändern Sie den Switch `-d` in `--data-binary`.
- Geben Sie das zu analysierende Bild mit der folgenden Syntax an: `@C:/Pictures/ImageToAnalyze.jpg`.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie die Anforderungs-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ersetzen Sie `<Image To Analyze>` durch das lokale Bild, das analysiert werden soll.
1. Ändern Sie optional die Sprache der Antwort (`language=en`).
1. Öffnen Sie auf einem Computer, auf dem cURL installiert ist, ein Befehlsfenster.
1. Fügen Sie den Code in das Fenster ein, und führen Sie den Befehl aus.

>[!NOTE]
>Sie müssen in Ihrem REST-Aufruf den gleichen Standort verwenden, den Sie zum Erwerb Ihrer Abonnementschlüssel verwendet haben. Wenn Sie Ihre Abonnementschlüssel beispielsweise von „westus“ erhalten haben, ersetzen Sie „westcentralus“ in der URL unten durch „westus“.

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -H "Content-Type: application/octet-stream" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" --data-binary <Image To Analyze>
```

## <a name="analyze-image-response"></a>Antwort zur Bildanalyse

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie ein Beispiel:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "requestId": "b6e33879-abb2-43a0-a96e-02cb5ae0b795",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden. Wenn Sie schnell mit Ihren Experimenten mit den Maschinelles Sehen-APIs beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
