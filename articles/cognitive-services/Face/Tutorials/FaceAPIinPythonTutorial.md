---
title: 'Tutorial: Erkennen und Umranden von Gesichtern in einem Bild – Gesichtserkennungs-API, Python'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Gesichtserkennungs-API mit dem Python SDK zum Erkennen menschlicher Gesichter in einem Bild verwenden.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6cc3ac25d2196c0275b445503b79b9ac06a791d3
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127736"
---
# <a name="tutorial-detect-and-frame-faces-with-the-face-api-and-python"></a>Tutorial: Erkennen und Umranden von Gesichtern mit der Gesichtserkennungs-API und Python 

In diesem Tutorial erfahren Sie, wie Sie die Gesichtserkennungs-API über das Python SDK aufrufen, um menschliche Gesichter in einem Bild zu erkennen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden des Tutorials müssen Sie die folgenden Schritte ausführen:

- Installieren Sie entweder Python 2.7+ oder Python 3.5+.
- Installieren Sie pip.
- Installieren Sie das Python SDK für die Gesichtserkennungs-API wie folgt:

```bash
pip install cognitive_face
```

- Rufen Sie einen [Abonnementschlüssel](https://azure.microsoft.com/try/cognitive-services/) für Microsoft Cognitive Services ab. Sie können entweder Ihren Primärschlüssel oder Sekundärschlüssel in diesem Tutorial verwenden. (Beachten Sie, dass Sie zur Verwendung einer Gesichtserkennungs-API über einen gültigen Abonnementschlüssel verfügen müssen.)

## <a name="detect-a-face-in-an-image"></a>Erkennen eines Gesichts in einem Bild

```python
import cognitive_face as CF

KEY = '<Subscription Key>'  # Replace with a valid subscription key (keeping the quotes in place).
CF.Key.set(KEY)

BASE_URL = 'https://westus.api.cognitive.microsoft.com/face/v1.0/'  # Replace with your regional Base URL
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

Nachfolgend ist ein Beispielergebnis aufgeführt. Es ist ein `list`-Objekt mit erkannten Gesichtern. Jeder Eintrag in der Liste ist eine `dict`-Instanz, wobei `faceId` eine eindeutige ID für das erkannte Gesicht ist und `faceRectangle` die Position des erkannten Gesichts beschreibt. Eine Gesichtserkennungs-ID läuft nach 24 Stunden ab.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Zeichnen von Rechtecken um Gesichter

Mithilfe der JSON-Koordination, die Sie durch den vorherigen Befehl erhalten haben, können Sie Rechtecke auf dem Bild zeichnen und so die einzelnen Gesichter visuell darstellen. Sie müssen `pip install Pillow` ausführen, um das Bildverarbeitungsmodul `PIL` zu verwenden.  Fügen Sie am Anfang der Datei Folgendes hinzu:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Dann fügen Sie nach `print(faces)` Folgendes in das Skript ein:

```python
#Convert width height to a point in a rectangle
def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))

#Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

#For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

#Display the image in the users default image browser.
img.show()
```

## <a name="further-exploration"></a>Weiteres Erkunden

Damit Sie die Gesichtserkennungs-API weiter erkunden können, bietet dieses Tutorial ein Beispiel der grafischen Benutzeroberfläche. Um es auszuführen, installieren Sie zuerst [wxPython](https://wxpython.org/pages/downloads/), und führen Sie dann die folgenden Befehle aus.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie die grundlegende Vorgehensweise zur Verwendung der Gesichtserkennungs-API über einen Aufruf des Python SDK kennengelernt. Weitere Informationen zu API-Details finden Sie unter den Vorgehensweisen und in der [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Verwandte Themen

- [Erste Schritte mit der Gesichtserkennungs-API in CSharp](FaceAPIinCSharpTutorial.md)
- [Erste Schritte mit der Gesichtserkennungs-API in Java für Android](FaceAPIinJavaForAndroidTutorial.md)
