---
title: Aufruf und Antwort – Python-Schnellstart für Azure Cognitive Services, Bing-Bildersuche-API | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen und Codebeispiele, mit denen Sie schnell mit der Bing-Bildersuche-API in Microsoft Cognitive Services in Azure einsteigen können.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 3b5d6a961ce4bcde8aaf73f1fbd30689a6c2c2d1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374026"
---
# <a name="call-and-response-your-first-bing-image-search-query-in-python"></a>Aufruf und Antwort: Ihre erste Bing-Bildersuche-Abfrage in Python

Die Bing-Bildersuche-API bietet eine ähnliche Erfahrung wie „Bing.com/Images“: Sie können eine Suchabfrage eines Benutzers an Bing senden und eine Liste mit relevanten Bildern erhalten.

Diese exemplarische Vorgehensweise veranschaulicht ein einfaches Beispiel für das Aufrufen der Bing-Bildersuche-API und die Nachbearbeitung des resultierenden JSON-Objekts. Weitere Informationen finden Sie in der [Bing-Bildersuche-Dokumentation](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

Sie können dieses Beispiel als Jupyter Notebook in [MyBinder](https://mybinder.org) ausführen, indem Sie auf das Binder-Badge klicken: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen über ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs** verfügen. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.

## <a name="running-the-walkthrough"></a>Ausführen der exemplarischen Vorgehensweise
Legen Sie zum Fortsetzen der exemplarischen Vorgehensweise `subscription_key` auf Ihren API-Schlüssel für den Bing-API-Dienst fest.


```python
subscription_key = None
assert subscription_key
```

Als Nächstes überprüfen Sie, ob der `search_url`-Endpunkt richtig ist. Zu dem Zeitpunkt, als dieser Artikel geschrieben wurde, wurde nur ein Endpunkt für Bing-Suche-APIs verwendet. Wenn Ihnen Autorisierungsfehler auffallen, vergleichen Sie diesen Wert mit dem Bing-Suche-Endpunkt in Ihrem Azure-Dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Legen Sie `search_term` für die Suche von Bildern von Welpen fest.


```python
search_term = "puppies"
```

Der folgende Block verwendet die `requests`-Bibliothek in Python, um die Bing-Suche-APIs aufzurufen und die Ergebnisse als JSON-Objekt zurückzugeben. Beachten Sie, dass wir den API-Schlüssel über das `headers`-Wörterbuch übergeben und den Suchbegriff über das `params`-Wörterbuch. Die vollständige Liste der zum Filtern von Suchergebnissen verwendbaren Optionen finden Sie in der [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)-Dokumentation.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

Das `search_results`-Objekt enthält die tatsächlichen Bilder zusammen mit umfassenden Metadaten wie verwandten Elementen. Die folgende Codezeile kann z.B. die Miniaturansicht-URLs für die ersten 16 Ergebnisse extrahieren.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Dann können wir mithilfe der `PIL`-Bibliothek die Miniaturbilder herunterladen und mithilfe der `matplotlib`-Bibliothek, um die Berichte auf einem $4\x 4$-Raster rendern.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

f, axes = plt.subplots(4, 4)
for i in range(4):
    for j in range(4):
        image_data = requests.get(thumbnail_urls[i+4*j])
        image_data.raise_for_status()
        image = Image.open(BytesIO(image_data.content))        
        axes[i][j].imshow(image)
        axes[i][j].axis("off")
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Einseitige Web-App für die Bing-Bildersuche](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Weitere Informationen 

[Übersicht zur Bing-Bildersuche](../overview.md)  
[Testen](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Holen Sie sich einen Zugriffsschlüssel einer kostenlosen Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Referenz zur Bing-Bildersuche-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
