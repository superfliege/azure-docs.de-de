---
title: 'Schnellstart: Erkennen von Emotionen auf Gesichtern in einem Bild – Emotionen-API, Python'
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Emotionen-API mit Python.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 02/05/2018
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: f0bcc88b60e0a9b93856aa32a10b9c0ad898ce95
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240697"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Schnellstart: Erstellen einer App zur Erkennung von Emotionen auf Gesichtern in einem Bild

> [!IMPORTANT]
> Die Emotionen-API wird am 15. Februar 2019 eingestellt. Die Funktion zur Erkennung von Emotionen ist jetzt als Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/) allgemein verfügbar. 

Diese exemplarische Vorgehensweise enthält Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der [Recognize-Methode der Emotionen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) mit Python zum Erkennen der von einer oder mehreren Personen auf einem Bild ausgedrückten Emotionen.

Sie können dieses Beispiel als Jupyter Notebook in [MyBinder](https://mybinder.org) ausführen, indem Sie auf das Binder-Badge [![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb) klicken.


## <a name="prerequisite"></a>Voraussetzung
Rufen Sie [hier](https://azure.microsoft.com/try/cognitive-services/) Ihren kostenlosen Abonnementschlüssel ab.

## <a name="running-the-walkthrough"></a>Ausführen der exemplarischen Vorgehensweise
Um mit dieser exemplarischen Vorgehensweise fortzufahren, ersetzen Sie `subscription_key` durch den API-Schlüssel, den Sie zuvor abgerufen haben.


```python
subscription_key = None
assert subscription_key
```

Stellen Sie als Nächstes sicher, dass die Dienst-URL der Region entspricht, über die Sie den API-Schlüssel eingerichtet haben. Wenn Sie einen Schlüssel für die Testversion verwenden, müssen Sie keine Änderungen vornehmen.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

Diese exemplarische Vorgehensweise verwendet Bilder, die auf dem Datenträger gespeichert werden. Sie können auch Bilder verwenden, die über eine öffentlich zugängliche URL verfügbar sind. Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Da die Bilddaten als Teil des Anforderungstexts übergeben werden, denken Sie daran, dass Sie den `Content-Type`-Header auf `application/octet-stream` festlegen müssen. Wenn Sie ein Bild über eine URL übergeben, müssen Sie den Header auf Folgendes festlegen:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
Erstellen Sie ein Wörterbuch, das die folgende URL enthält:
```python
data = {'url': image_url}
```
Übergeben Sie es anschließend mit folgendem Code an die `requests`-Bibliothek:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

Laden Sie zunächst einige Beispielbilder von der Website [Emotionen-API](https://azure.microsoft.com/services/cognitive-services/emotion/) herunter.


```bash
%%bash
mkdir -p images
curl -Ls https://aka.ms/csnb-emotion-1 -o images/emotion_1.jpg
curl -Ls https://aka.ms/csnb-emotion-2 -o images/emotion_2.jpg
```


```python
image_path = "images/emotion_1.jpg"
image_data = open(image_path, "rb").read()
```


```python
import requests
headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
response.raise_for_status()
analysis = response.json()
analysis
```




    [{'faceRectangle': {'height': 162, 'left': 130, 'top': 141, 'width': 162},
      'scores': {'anger': 9.29041e-06,
       'contempt': 0.000118981574,
       'disgust': 3.15619363e-05,
       'fear': 0.000589638,
       'happiness': 0.06630674,
       'neutral': 0.00555004273,
       'sadness': 7.44669524e-06,
       'surprise': 0.9273863}}]



Das zurückgegebene JSON-Objekt enthält die Begrenzungsrahmen der Gesichter, die zusammen mit den erkannten Emotionen erkannt wurden. Jede Emotion ist einer Bewertung zwischen 0 und 1 zugeordnet, in denen ein höheres Ergebnis mehr über eine Emotion aussagt als ein niedrigeres Ergebnis.

Die folgenden Codezeilen stellen die erkannten Emotionen auf den Gesichtern im Bild mithilfe der Bibliothek `matplotlib` dar. Der Übersichtlichkeit halber werden nur die ersten drei Emotionen angezeigt.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from io import BytesIO
from PIL import Image

plt.figure(figsize=(5,5))

image  = Image.open(BytesIO(image_data))
ax     = plt.imshow(image, alpha=0.6)

for face in analysis:
    fr = face["faceRectangle"]
    em = face["scores"]
    origin = (fr["left"], fr["top"])
    p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
    plt.text(origin[0], origin[1], ct, fontsize=20)    
_ = plt.axis("off")
```

Die als Nächstes dargestellte Funktion `annotate_image` kann bei Angabe des Pfads im Dateisystem verwendet werden, um Emotionen zusätzlich zu einer Bilddatei zu überlagern. Sie basiert auf dem Code zum Aufrufen der zuvor gezeigten Emotionen-API.


```python
def annotate_image(image_path):    
    image_data = open(image_path, "rb").read()
    headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
    response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
    response.raise_for_status()
    analysis = response.json()

    plt.figure()

    image  = Image.open(image_path)
    ax     = plt.imshow(image, alpha=0.6)

    for face in analysis:
        fr = face["faceRectangle"]
        em = face["scores"]
        origin = (fr["left"], fr["top"])
        p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
        ax.axes.add_patch(p)
        ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
        plt.text(origin[0], origin[1], ct, fontsize=20, va="bottom")    
    _ = plt.axis("off")
```

Abschließend kann die Funktion `annotate_image` für eine Bilddatei aufgerufen werden, wie in der folgenden Zeile dargestellt wird:


```python
annotate_image("images/emotion_2.jpg")
```
