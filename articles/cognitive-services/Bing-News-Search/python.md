---
title: 'Schnellstart: Durchführen einer Neuigkeitensuche mit Python – REST-API der Bing-News-Suche'
titlesuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung zum Senden einer Anforderung an die REST-API der Bing-News-Suche mit Python, um eine JSON-Antwort zu erhalten.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 8ce8353df9a6f8354c56d9c9115645c0b7f2136a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53251656"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Schnellstart: Durchführen einer Neuigkeitensuche mit Python und der REST-API der Bing-News-Suche

Diese exemplarische Vorgehensweise veranschaulicht ein einfaches Beispiel für das Aufrufen der Bing-News-Suche-API und die Nachbearbeitung des resultierenden JSON-Objekts. Weitere Informationen finden Sie in der [Bing-News-Suche-Dokumentation](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

Sie können dieses Beispiel als Jupyter Notebook in [MyBinder](https://mybinder.org) ausführen, indem Sie auf das Binder-Badge klicken: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs**. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie bei der Aktivierung Ihrer kostenlosen Testversion erhalten.  Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="running-the-walkthrough"></a>Ausführen der exemplarischen Vorgehensweise
Legen Sie zuerst `subscription_key` auf Ihren API-Schlüssel für den Bing-API-Dienst fest.


```python
subscription_key = None
assert subscription_key
```

Als Nächstes überprüfen Sie, ob der `search_url`-Endpunkt richtig ist. Zu dem Zeitpunkt, als dieser Artikel geschrieben wurde, wurde nur ein Endpunkt für Bing-Suche-APIs verwendet. Wenn Ihnen Autorisierungsfehler auffallen, vergleichen Sie diesen Wert mit dem Bing-Suche-Endpunkt in Ihrem Azure-Dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

Legen Sie `search_term` für die Suche nach Artikeln über Microsoft fest.


```python
search_term = "Microsoft"
```

Der folgende Block verwendet die `requests`-Bibliothek in Python, um die Bing-Suche-APIs aufzurufen und die Ergebnisse als JSON-Objekt zurückzugeben. Beachten Sie, dass wir den API-Schlüssel über das `headers`-Wörterbuch übergeben und den Suchbegriff über das `params`-Wörterbuch. Die vollständige Liste der zum Filtern von Suchergebnissen verwendbaren Optionen finden Sie in der [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference)-Dokumentation.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

Das `search_results`-Objekt enthält die relevanten neuen Artikel sowie umfangreiche Metadaten. Beispielsweise extrahiert die folgende Codezeile die Beschreibungen der Artikel.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

Diese Beschreibungen lassen sich dann als Tabelle darstellen, wobei das Suchschlüsselwort **fett** hervorgehoben ist.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchblättern von Nachrichten](paging-news.md)
> [Verwenden von Dekorationsmarkierungen zum Markieren von Text](hit-highlighting.md)

## <a name="see-also"></a>Weitere Informationen 

 [Suchen nach Nachrichten im Web](search-the-web.md)  
 [Testen](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
