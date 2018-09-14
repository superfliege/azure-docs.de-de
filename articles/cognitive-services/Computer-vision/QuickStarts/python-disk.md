---
title: Python-Schnellstart zum Analysieren von lokalen Bildern mit der Maschinelles Sehen-API | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: In dieser Schnellstartanleitung analysieren Sie ein lokales Bild, indem Sie die Maschinelles Sehen-API mit Python in Cognitive Services verwenden.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: a1f3fce5a547f143f7c4884c6642e78f53d160e9
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770095"
---
# <a name="quickstart-analyze-a-local-image---rest-python"></a>Schnellstart: Analysieren eines lokalen Bilds – REST, Python

In dieser Schnellstartanleitung analysieren Sie ein lokales Bild mit der Maschinelles Sehen-API. Informationen zur Analyse eines Remotebilds finden Sie unter [Analyze a remote image with Python](python-analyze.md) (Analysieren eines Remotebilds mit Python).

Sie können diese Schnellstartanleitung Schritt für Schritt mit einem Jupyter-Notebook in [MyBinder](https://mybinder.org) ausführen. Klicken Sie zum Starten von Binder auf die folgende Schaltfläche:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).

## <a name="analyze-a-local-image"></a>Analysieren von lokalen Bildern

Dieses Beispiel ähnelt [Analyze a remote image with Python](python-analyze.md) (Analysieren eines Remotebilds mit Python), aber das zu analysierende Bild wird lokal vom Datenträger gelesen. Zwei Änderungen sind erforderlich:

- Fügen Sie der Anforderung den Header `{"Content-Type": "application/octet-stream"}` hinzu.
- Fügen Sie die Bilddaten (Bytearray) dem Text der Anforderung hinzu.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in eine neue Python-Skriptdatei.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie den Wert von `vision_base_url` ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ändern Sie den Wert von `image_path` in den Pfad eines lokalen Bilds.
1. Führen Sie das Skript aus.

Im folgenden Code wird die Python-Bibliothek `requests` verwendet, um die Bildanalyse-API für maschinelles Sehen aufzurufen. Die Ergebnisse werden als JSON-Objekt zurückgegeben. Der API-Schlüssel wird über das `headers`-Wörterbuch übergeben. Die Merkmale, die erkannt werden sollen, werden über das `params`-Wörterbuch übergeben. Die binären Bilddaten werden über den Parameter `data` an `requests.post` übergeben.

## <a name="analyze-image-request"></a>Anforderung zur Bildanalyse

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the westcentralus region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

analyze_url = vision_base_url + "analyze"

# Set image_path to the local path of an image that you want to analyze.
image_path = "C:/Documents/ImageToAnalyze.jpg"

# Read the image into a byte array
image_data = open(image_path, "rb").read()
headers    = {'Ocp-Apim-Subscription-Key': subscription_key,
              'Content-Type': 'application/octet-stream'}
params     = {'visualFeatures': 'Categories,Description,Color'}
response = requests.post(
    analyze_url, headers=headers, params=params, data=image_data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The most
# relevant caption for the image is obtained from the 'description' property.
analysis = response.json()
print(analysis)
image_caption = analysis["description"]["captions"][0]["text"].capitalize()

# Display the image and overlay it with the caption.
image = Image.open(BytesIO(image_data))
plt.imshow(image)
plt.axis("off")
_ = plt.title(image_caption, size="x-large", y=-0.1)
```

## <a name="analyze-image-response"></a>Antwort zur Bildanalyse

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie ein Beispiel:

```json
{
  "categories": [
    {
      "name": "outdoor_",
      "score": 0.00390625,
      "detail": {
        "landmarks": []
      }
    },
    {
      "name": "outdoor_street",
      "score": 0.33984375,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "building",
      "outdoor",
      "street",
      "city",
      "people",
      "busy",
      "table",
      "walking",
      "traffic",
      "filled",
      "large",
      "many",
      "group",
      "night",
      "light",
      "crowded",
      "bunch",
      "standing",
      "man",
      "sign",
      "crowd",
      "umbrella",
      "riding",
      "tall",
      "woman",
      "bus"
    ],
    "captions": [
      {
        "text": "a group of people on a city street at night",
        "confidence": 0.9122243847383961
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Brown",
    "dominantColorBackground": "Brown",
    "dominantColors": [
      "Brown"
    ],
    "accentColor": "B54316",
    "isBwImg": false
  },
  "requestId": "c11894eb-de3e-451b-9257-7c8b168073d1",
  "metadata": {
    "height": 600,
    "width": 450,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie eine einfache Python-Anwendung kennen, die maschinelles Sehen verwendet, um eine optische Zeichenerkennung (Optical Character Recognition, OCR) durchzuführen, intelligent zugeschnittene Miniaturansichten zu erstellen sowie visuelle Merkmale (einschließlich Gesichter) in einem Bild zu erkennen, zu kategorisieren, zu markieren und zu beschreiben. Wenn Sie schnell mit Ihren Experimenten mit den Maschinelles Sehen-APIs beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Tutorial zur Maschinelles Sehen-API in Python](../Tutorials/PythonTutorial.md)
