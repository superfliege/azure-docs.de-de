---
title: Python-Schnellstart für die API für die visuelle Bing-Suche | Microsoft-Dokumentation
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Erfahren Sie, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und dadurch Erkenntnisse zu diesem Bild erhalten.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 96bd94e37c75d10726245fbcea7044d4ae2ed07e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070374"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>Ihre erste Abfrage für die visuelle Bing-Suche in Python

Die API für die visuelle Bing-Suche gibt Informationen zu einem von Ihnen bereitgestellten Bild zurück. Sie können ein Bild über die Bild-URL, ein Erkenntnistoken oder durch Hochladen des Bilds bereitstellen. Informationen zu diesen Optionen finden Sie unter [Was ist die API für die visuelle Bing-Suche?](../overview.md). In diesem Artikel wird gezeigt, wie Sie ein Bild hochladen. Das Hochladen eines Bilds ist besonders in Szenarien mit einem mobilen Gerät nützlich, wenn Sie eine bekannte Sehenswürdigkeit fotografiert haben und Informationen dazu erhalten möchten. Die Informationen können z.B. Wissenswertes zur Sehenswürdigkeit beinhalten. 

Wenn Sie ein lokales Bild hochladen, müssen Sie die folgenden Formulardaten in den Text der POST-Anforderung einfügen. Die Formulardaten müssen den Header „Content-Disposition“ enthalten. Der `name`-Parameter muss auf „image“ festgelegt werden. Der `filename`-Parameter kann auf eine beliebige Zeichenfolge festgelegt werden. Der Inhalt des Formulars sind die Binärdaten des Bilds. Sie können eine maximale Bildgröße von 1 MB hochladen. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dieser Artikel zeigt eine einfache Konsolenanwendung, die eine Anforderung an die API für die visuelle Bing-Suche sendet und die Suchergebnisse im JSON-Format anzeigt. Die Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen senden und JSON analysieren kann. 

## <a name="prerequisites"></a>Voraussetzungen

Um diesen Code auszuführen, benötigen Sie [Python 3](https://www.python.org/).

Für diese Schnellstartanleitung können Sie den Schlüssel eines [kostenlosen Testabonnements](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) oder eines kostenpflichtigen Abonnements verwenden.

## <a name="running-the-walkthrough"></a>Ausführen der exemplarischen Vorgehensweise

Führen Sie die folgenden Schritte aus, um diese Anwendung auszuführen:

1. Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Python-Projekt.
2. Erstellen Sie eine Datei namens „visualsearch.py“, und fügen Sie den in dieser Schnellstartanleitung gezeigten Code hinzu.
3. Ersetzen Sie den `SUBSCRIPTION_KEY`-Wert durch Ihren Abonnementschlüssel.
3. Ersetzen Sie den `imagePath`-Wert durch den Pfad des Bilds, das Sie hochladen möchten.
4. Führen Sie das Programm aus.



Das folgende Beispiel zeigt, wie Sie eine Nachricht mit mehrteiligen Formulardaten in Python senden.

```python
"""Bing Visual Search upload image example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json


BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

imagePath = '<pathtoyourimagetouploadgoeshere>'

file = {'image' : ('myfile', open(imagePath, 'rb'))}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```


## <a name="next-steps"></a>Nächste Schritte

[Abrufen von Informationen zu einem Bild mithilfe eines Erkenntnistokens](../use-insights-token.md)  
[Tutorial zum Bildupload für die visuelle Bing-Suche](../tutorial-visual-search-image-upload.md)
[Tutorial zu einer Single-Page-App für die visuelle Bing-Suche](../tutorial-bing-visual-search-single-page-app.md)  
[Übersicht über die visuelle Bing-Suche](../overview.md)  
[Testen](https://aka.ms/bingvisualsearchtryforfree)  
[Abrufen eines Zugriffsschlüssels für eine kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referenz zur API für die visuelle Bing-Suche](https://aka.ms/bingvisualsearchreferencedoc)
