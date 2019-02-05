---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und Ruby'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Gesichtserkennungs-API mit Ruby, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 05/30/2018
ms.author: pafarley
ms.openlocfilehash: 6320e05172673abf809ac7fdbfea544ed74b0019
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221078"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-ruby"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und Ruby

In dieser Schnellstartanleitung führen Sie die Gesichtserkennung für menschliche Gesichter in einem Bild durch, indem Sie die Gesichtserkennungs-API verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des Beispiels benötigen Sie einen Abonnementschlüssel. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie Abonnementschlüssel für eine kostenlose Testversion abrufen.

## <a name="face---detect-request"></a>„Face – Detect“-Anforderung

Verwenden Sie die [Face – Detect](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-Methode, um Gesichter in einem Bild zu erkennen und Gesichtsattribute zurückzugeben, z.B.:

* Gesichts-ID: Eine eindeutige ID, die in verschiedenen Szenarien mit der Gesichtserkennungs-API verwendet wird.
* Gesichtsrechteck: Die Position des linken und oberen Rands sowie die Breite und Höhe, um die Position des Gesichts im Bild anzugeben.
* Besondere Merkmale: Ein Array mit 27 Punkten zu Gesichtszügen, die auf die wichtigen Positionen von Gesichtskomponenten hinweisen.
* Dies können Gesichtsattribute wie Alter, Geschlecht, Intensität des Lächelns, Kopfhaltung und Gesichtsbehaarung sein.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie den Wert von `uri` ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Legen Sie optional `imageUri` auf das Bild fest, das Sie analysieren möchten.
1. Speichern Sie die Datei mit der Erweiterung `.rb`.
1. Öffnen Sie die Ruby-Eingabeaufforderung, und führen Sie die Datei aus, z.B. `ruby myfile.rb`.

```ruby
require 'net/http'

# You must use the same location in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from  westus,
# replace "westcentralus" in the URL below with "westus".
uri = URI('https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect')
uri.query = URI.encode_www_form({
    # Request parameters
    'returnFaceId' => 'true',
    'returnFaceLandmarks' => 'false',
    'returnFaceAttributes' => 'age,gender,headPose,smile,facialHair,glasses,' +
        'emotion,hair,makeup,occlusion,accessories,blur,exposure,noise'
})

request = Net::HTTP::Post.new(uri.request_uri)

# Request headers
# Replace <Subscription Key> with your valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '<Subscription Key>'
request['Content-Type'] = 'application/json'

imageUri = "https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg"
request.body = "{\"url\": \"" + imageUri + "\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts response.body
```

## <a name="face---detect-response"></a>„Face – Detect“-Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie ein Beispiel:

```json
[
  {
    "faceId": "e93e0db1-036e-4819-b5b6-4f39e0f73509",
    "faceRectangle": {
      "top": 621,
      "left": 616,
      "width": 195,
      "height": 195
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 6.8,
        "yaw": 3.7
      },
      "gender": "male",
      "age": 37,
      "facialHair": {
        "moustache": 0.4,
        "beard": 0.4,
        "sideburns": 0.1
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.999,
        "sadness": 0.001,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.89
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.51
      },
      "noise": {
        "noiseLevel": "medium",
        "value": 0.59
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.04,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.98
          },
          {
            "color": "brown",
            "confidence": 0.87
          },
          {
            "color": "gray",
            "confidence": 0.85
          },
          {
            "color": "other",
            "confidence": 0.25
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.02
          }
        ]
      }
    }
  },
  {
    "faceId": "37c7c4bc-fda3-4d8d-94e8-b85b8deaf878",
    "faceRectangle": {
      "top": 693,
      "left": 1503,
      "width": 180,
      "height": 180
    },
    "faceAttributes": {
      "smile": 0.003,
      "headPose": {
        "pitch": 0,
        "roll": 2,
        "yaw": -2.2
      },
      "gender": "female",
      "age": 56,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0.001,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.003,
        "neutral": 0.984,
        "sadness": 0.011,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.83
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.41
      },
      "noise": {
        "noiseLevel": "high",
        "value": 0.76
      },
      "makeup": {
        "eyeMakeup": false,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.06,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.99
          },
          {
            "color": "gray",
            "confidence": 0.89
          },
          {
            "color": "other",
            "confidence": 0.64
          },
          {
            "color": "brown",
            "confidence": 0.34
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.03
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
