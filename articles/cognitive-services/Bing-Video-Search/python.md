---
title: 'Schnellstart: Bing-Videosuche, Python'
titlesuffix: Azure Cognitive Services
description: Hier erhalten Sie Informationen und Codebeispiele für einen schnellen Einstieg in die Bing-Videosuche-API.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 797eb476aa3386949b08efb957edf48a97e40d6b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220013"
---
# <a name="quickstart-bing-video-search-api-with-python"></a>Schnellstart: Bing-Videosuche-API mit Python

In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie die Bing-Videosuche-API verwenden, die Ihnen im Rahmen von Microsoft Cognitive Services in Azure zur Verfügung steht. Die [API-Referenz](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) enthält technische Informationen zu den APIs.

Sie können dieses Beispiel als Jupyter Notebook in [MyBinder](https://mybinder.org) ausführen, indem Sie auf das Binder-Badge klicken: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs**. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.

## <a name="running-the-walkthrough"></a>Ausführen der exemplarischen Vorgehensweise

Legen Sie zuerst `subscription_key` auf Ihren API-Schlüssel für den Bing-API-Dienst fest.


```python
subscription_key = None
assert subscription_key
```

Als Nächstes überprüfen Sie, ob der `search_url`-Endpunkt richtig ist. Zu dem Zeitpunkt, als dieser Artikel geschrieben wurde, wurde nur ein Endpunkt für Bing-Suche-APIs verwendet. Wenn Ihnen Autorisierungsfehler auffallen, vergleichen Sie diesen Wert mit dem Bing-Suche-Endpunkt in Ihrem Azure-Dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Legen Sie `search_term` für die Suche nach Videos von Kätzchen fest.


```python
search_term = "kittens"
```

Der folgende Block verwendet die `requests`-Bibliothek in Python, um die Bing-Suche-APIs aufzurufen und die Ergebnisse als JSON-Objekt zurückzugeben. Beachten Sie, dass wir den API-Schlüssel über das `headers`-Wörterbuch übergeben und den Suchbegriff über das `params`-Wörterbuch. Die vollständige Liste der zum Filtern von Suchergebnissen verwendbaren Optionen finden Sie in der [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference)-Dokumentation.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

Das `search_results`-Objekt enthält die relevanten Videos sowie umfangreiche Metadaten. Zum Ansehen eines der Videos fügen Sie dessen `embedHtml`-Eigenschaft in ein `IFrame`-Element ein.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchsuchen von Videos](paging-videos.md)
> [Anpassen der Größe und Zuschneiden von Miniaturansichten](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Weitere Informationen 

 [Suchen nach Videos im Web](search-the-web.md) [Testen](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
