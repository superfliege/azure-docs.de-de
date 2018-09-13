---
title: 'cURL-Schnellstart: Analysieren von Remotebildern mit der Maschinelles Sehen-API | Microsoft-Dokumentation'
titleSuffix: Microsoft Cognitive Services
description: In dieser Schnellstartanleitung analysieren Sie ein Remotebild mithilfe der Maschinelles Sehen-API mit cURL in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: c092432adbcba9cf7a131584131fd05991d1edef
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770303"
---
# <a name="quickstart-analyze-a-remote-image---rest-curl"></a>Schnellstart: Analysieren eines Remotebilds – REST und cURL

In dieser Schnellstartanleitung analysieren Sie mithilfe der Maschinelles Sehen-API ein Remotebild, um visuelle Merkmale zu extrahieren. Informationen zum Analysieren eines lokalen Bilds finden Sie unter [Analyze a local image with cURL](curl-disk.md) (Analysieren von lokalen Bildern mit cURL).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).

## <a name="analyze-a-remote-image"></a>Analysieren von Remotebildern

Mit der [Methode zum Analysieren von Bildern](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) können Sie basierend auf dem Inhalt des Bilds visuelle Merkmale extrahieren. Sie können ein Bild hochladen oder eine Bild-URL angeben und auswählen, welche Merkmale zurückgegeben werden sollen. Zu den verfügbaren Merkmalen zählen beispielsweise:

* Eine detaillierte Liste der Tags im Zusammenhang mit dem Bildinhalt
* Eine Beschreibung des Bildinhalts in einem vollständigen Satz
* Die Koordinaten, das Geschlecht und das Alter aller im Bild enthaltenen Gesichter
* Der Bildtyp (ClipArt oder Strichzeichnung)
* Die dominante Farbe, die Akzentfarbe und, ob ein Bild schwarzweiß ist
* Die in dieser [Taxonomie](../Category-Taxonomy.md) definierte Kategorie
* Ob das Bild nicht jugendfreie oder freizügige Inhalte enthält

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie die Anforderungs-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) ggf. in den Speicherort, vom dem Sie Ihren Abonnementschlüssel abgerufen haben.
1. Ändern Sie optional das Bild (`{\"url\":\"...`), das Sie analysieren möchten.
1. Ändern Sie optional die Sprache der Antwort (`language=en`).
1. Öffnen Sie auf einem Computer, auf dem cURL installiert ist, ein Befehlsfenster.
1. Fügen Sie den Code in das Fenster ein, und führen Sie den Befehl aus.

>[!NOTE]
>Sie müssen in Ihrem REST-Aufruf den gleichen Standort verwenden, den Sie zum Erwerb Ihrer Abonnementschlüssel verwendet haben. Wenn Sie Ihre Abonnementschlüssel beispielsweise von „westus“ erhalten haben, ersetzen Sie „westcentralus“ in der unten stehenden URL durch „westus“.

## <a name="analyze-image-request"></a>Bildanalyseanforderung

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg\"}"
```

## <a name="analyze-image-response"></a>Bildanalyseantwort

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
