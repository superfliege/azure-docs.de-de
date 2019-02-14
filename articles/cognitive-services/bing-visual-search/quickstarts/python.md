---
title: 'Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und Python'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und Erkenntnisse dazu gewinnen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 873da64592c2c2e925d8731d4b1154db95bed31d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863226"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-python"></a>Schnellstart: Ihre erste Abfrage für die visuelle Bing-Suche in Python

Verwenden Sie diese Schnellstartanleitung, um die API für die visuelle Bing-Suche zum ersten Mal aufzurufen und die Suchergebnisse anzuzeigen. Mit dieser einfachen JavaScript-Anwendung wird ein Bild in die API hochgeladen, und es werden die zurückgegebenen Informationen angezeigt. Diese Anwendung ist zwar in JavaScript geschrieben, aber die API ist ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

Beim Hochladen eines lokalen Bilds müssen die POST-Formulardaten den Content-Disposition-Header enthalten. Der `name`-Parameter muss auf „image“ festgelegt werden. Der `filename`-Parameter kann auf eine beliebige Zeichenfolge festgelegt werden. Der Inhalt des Formulars sind die Binärdaten des Bilds. Sie können eine maximale Bildgröße von 1 MB hochladen.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.x](https://www.python.org/)


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE bzw. in einem Editor eine neue Python-Datei, und fügen Sie die folgende Importanweisung hinzu.

    ```python
    import requests, json
    ```

2. Erstellen Sie Variablen für Ihren Abonnementschlüssel, den Endpunkt und den Pfad zu dem Bild, das hochgeladen werden soll.

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. Erstellen Sie ein Wörterbuchobjekt für die Headerinformationen Ihrer Anforderungen. Binden Sie Ihren Abonnementschlüssel wie unten gezeigt an die Zeichenfolge `Ocp-Apim-Subscription-Key`.

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Erstellen Sie ein weiteres Wörterbuch für Ihr Bild, das beim Senden der Anforderung geöffnet und hochgeladen wird. 

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Analysieren der JSON-Antwort

1. Erstellen Sie eine Methode mit dem Namen `print_json()` für die API-Antwort und zum Ausgeben des JSON-Codes.

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>Senden der Anforderung

1. Verwenden Sie `requests.post()`, um eine Anforderung an die API für die visuelle Bing-Suche zu senden. Fügen Sie die Zeichenfolge für Ihren Endpunkt, den Header und die Dateiinformationen ein. Geben Sie `response.json()` mit `print_json()` aus.

    ```python
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())
    
    except Exception as ex:
        raise ex
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Web-App für die benutzerdefinierte Suche](../tutorial-bing-visual-search-single-page-app.md)
