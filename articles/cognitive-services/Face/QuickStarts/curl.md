---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild – Gesichtserkennungs-API, cURL'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Gesichtserkennungs-API mit cURL, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 05/10/2018
ms.author: pafarley
ms.openlocfilehash: da18b7b3666863742f987b2ee0da297c0838d266
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343060"
---
# <a name="quickstart-detect-faces-in-an-image-using-curl"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit cURL

In diesem Schnellstart verwenden Sie die Gesichtserkennungs-API, um Gesichter in einem Bild zu erkennen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des Beispiels benötigen Sie einen Abonnementschlüssel. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie Abonnementschlüssel für eine kostenlose Testversion abrufen.

## <a name="detect-faces-in-an-image"></a>Gesichtserkennung in einem Bild

Verwenden Sie die [Face – Detect](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-Methode, um Gesichter in einem Bild zu erkennen und Gesichtsattribute zurückzugeben, z.B.:

* Gesichtserkennungs-API: Eindeutige ID, die in verschiedenen Gesichtserkennungs-API-Szenarien verwendet wird.
* Gesichtsrechteck: Die Position des linken und oberen Rands sowie die Breite und Höhe, um die Position des Gesichts im Bild anzugeben.
* Besondere Merkmale: Ein Array mit 27 Punkten zu Gesichtszügen, die auf die wichtigen Positionen von Gesichtskomponenten hinweisen.
* Dies können Gesichtsattribute wie Alter, Geschlecht, Intensität des Lächelns, Kopfhaltung und Gesichtsbehaarung sein.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Öffnen Sie eine Eingabeaufforderung.
2. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
3. Ändern Sie die URL (`https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect`) ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
4. Ändern Sie optional das Bild (`"{\"url\":...`), das Sie analysieren möchten.
5. Fügen Sie den Code in das Befehlsfenster ein.
6. Führen Sie den Befehl aus.

### <a name="face---detect-request"></a>„Face – Detect“-Anforderung

> [!NOTE]
> Sie müssen in Ihrem REST-Aufruf den gleichen Standort verwenden, den Sie zum Erwerb Ihrer Abonnementschlüssel verwendet haben. Wenn Sie Ihre Abonnementschlüssel beispielsweise von „westus“ erhalten haben, ersetzen Sie „westcentralus“ in der folgenden URL durch „westus“.

```shell
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise" -H "Content-Type: application/json" --data-ascii "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}"
```

### <a name="face---detect-response"></a>„Face – Detect“-Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben.

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "black",
            "confidence": 0.87
          },
          {
            "color": "other",
            "confidence": 0.51
          },
          {
            "color": "blond",
            "confidence": 0.08
          },
          {
            "color": "red",
            "confidence": 0.08
          },
          {
            "color": "gray",
            "confidence": 0.02
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Gesichtserkennungs-APIs, die verwendet werden, um in einem Bild menschliche Gesichter zu erkennen, die Gesichter mit Rechtecken zu kennzeichnen und Attribute wie Alter und Geschlecht zurückzugeben.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
