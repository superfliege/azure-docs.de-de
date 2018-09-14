---
title: Python-Schnellstart zum Domänenmodell mit der Maschinelles Sehen-API | Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie die Domänenmodelle, um in einem Bild Prominente und Sehenswürdigkeiten zu identifizieren, indem Sie die Maschinelles Sehen-API mit Python in Cognitive Services verwenden.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 357cab72c0a6c9a2254350c84cda91c366ac685a
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770108"
---
# <a name="quickstart-use-a-domain-model---rest-python"></a>Schnellstart: Verwenden eines Domänenmodells – REST, Python

In dieser Schnellstartanleitung verwenden Sie die Domänenmodelle, um in einem Bild Prominente und Sehenswürdigkeiten zu identifizieren, indem Sie die Maschinelles Sehen-API verwenden.

Sie können diese Schnellstartanleitung Schritt für Schritt mit einem Jupyter-Notebook in [MyBinder](https://mybinder.org) ausführen. Klicken Sie zum Starten von Binder auf die folgende Schaltfläche:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden der Maschinelles Sehen-API benötigen Sie einen Abonnementschlüssel. Siehe [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Abrufen von Abonnementschlüsseln).

## <a name="identify-celebrities-and-landmarks"></a>Identifizieren von Prominenten und Sehenswürdigkeiten

Mit der [Recognize Domain Specific Content-Methode](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) können Sie in einem Bild einen bestimmten Satz mit Objekten identifizieren. Die beiden domänenspezifischen Modelle, die derzeit verfügbar sind, sind _celebrities_ (Prominente) und _landmarks_ (Sehenswürdigkeiten).

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in eine neue Python-Skriptdatei.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie den Wert von `vision_base_url` ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ändern Sie optional den Wert von `image_url` in ein anderes Bild.
1. Führen Sie das Skript aus.

Im folgenden Code wird die Python-Bibliothek `requests` verwendet, um die Bildanalyse-API für maschinelles Sehen aufzurufen. Die Ergebnisse werden als JSON-Objekt zurückgegeben. Der API-Schlüssel wird über das `headers`-Wörterbuch übergeben. Das zu verwendende Modell wird über das `params`-Wörterbuch übergeben.

## <a name="landmark-identification"></a>Identifikation von Sehenswürdigkeiten

### <a name="recognize-landmark-request"></a>Recognize Landmark-Anforderung

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

landmark_analyze_url = vision_base_url + "models/landmarks/analyze"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/f/f6/" + \
    "Bunker_Hill_Monument_2005.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'model': 'landmarks'}
data    = {'url': image_url}
response = requests.post(
    landmark_analyze_url, headers=headers, params=params, json=data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The
# most relevant landmark for the image is obtained from the 'result' property.
analysis = response.json()
assert analysis["result"]["landmarks"] is not []
print(analysis)
landmark_name = analysis["result"]["landmarks"][0]["name"].capitalize()

# Display the image and overlay it with the landmark name.
image = Image.open(BytesIO(requests.get(image_url).content))
plt.imshow(image)
plt.axis("off")
_ = plt.title(landmark_name, size="x-large", y=-0.1)
```

### <a name="recognize-landmark-response"></a>Recognize Landmark-Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie ein Beispiel:

```json
{
  "result": {
    "landmarks": [
      {
        "name": "Bunker Hill Monument",
        "confidence": 0.9768505096435547
      }
    ]
  },
  "requestId": "659a10cd-44bb-44db-9147-a295b853b2b8",
  "metadata": {
    "height": 1600,
    "width": 1200,
    "format": "Jpeg"
  }
}
```

## <a name="celebrity-identification"></a>Identifikation von Sehenswürdigkeiten

### <a name="recognize-celebrity-request"></a>Recognize Celebrity-Anforderung

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

vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

celebrity_analyze_url = vision_base_url + "models/celebrities/analyze"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/d/d9/" + \
    "Bill_gates_portrait.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'model': 'celebrities'}
data    = {'url': image_url}
response = requests.post(
    celebrity_analyze_url, headers=headers, params=params, json=data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The
# most relevant celebrity for the image is obtained from the 'result' property.
analysis = response.json()
assert analysis["result"]["celebrities"] is not []
print(analysis)
celebrity_name = analysis["result"]["celebrities"][0]["name"].capitalize()

# Display the image and overlay it with the celebrity name.
image = Image.open(BytesIO(requests.get(image_url).content))
plt.imshow(image)
plt.axis("off")
_ = plt.title(celebrity_name, size="x-large", y=-0.1)
```

### <a name="recognize-celebrity-response"></a>Recognize Celebrity-Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Hier sehen Sie ein Beispiel:

```json
{
  "result": {
    "celebrities": [
      {
        "faceRectangle": {
          "top": 123,
          "left": 156,
          "width": 187,
          "height": 187
        },
        "name": "Bill Gates",
        "confidence": 0.9993845224380493
      }
    ]
  },
  "requestId": "f14ec1d0-62d4-4296-9ceb-6b5776dc2020",
  "metadata": {
    "height": 521,
    "width": 550,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie eine einfache Python-Anwendung kennen, die maschinelles Sehen verwendet, um eine optische Zeichenerkennung (Optical Character Recognition, OCR) durchzuführen, intelligent zugeschnittene Miniaturansichten zu erstellen sowie visuelle Merkmale (einschließlich Gesichter) in einem Bild zu erkennen, zu kategorisieren, zu markieren und zu beschreiben. Wenn Sie schnell mit Ihren Experimenten mit den Maschinelles Sehen-APIs beginnen möchten, können Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) verwenden.

> [!div class="nextstepaction"]
> [Tutorial zur Maschinelles Sehen-API in Python](../Tutorials/PythonTutorial.md)
