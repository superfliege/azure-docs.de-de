---
title: Aufruf und Antwort – Python-Schnellstart für Azure Cognitive Services, Bing-Websuche-API | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen und Codebeispiele, mit denen Sie schnell mit der Bing-Websuche-API in Microsoft Cognitive Services in Azure einsteigen können.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: 8d4df9db60c7a74a5b9e53d4622528c0054b4f19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374146"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-python"></a>Aufruf und Antwort: Ihre erste Bing-Websuche-Abfrage in Python

Die Bing-Websuche-API ähnelt Bing.com/Search darin, dass Suchergebnisse zurückgegeben werden, die nach Einschätzung von Bing für die Abfrage des Benutzers relevant sind. Die Ergebnisse können möglicherweise Webseiten, Bilder, Videos, Nachrichten und Entitäten sowie verwandte Suchabfragen, Rechtschreibkorrekturen, Zeitzonen, Einheitenkonvertierung, Übersetzungen und Berechnungen enthalten. Welche Arten von Ergebnissen Sie erhalten, hängt von ihrer Relevanz ab und der Ebene der Bing-Suche-APIs, die Sie abonnieren.

Die [API-Referenz](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) enthält technische Informationen zu den APIs.

Sie können dieses Beispiel als Jupyter Notebook in [MyBinder](https://mybinder.org) ausführen, indem Sie auf das Binder-Badge klicken: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)


## <a name="prerequisites"></a>Voraussetzungen
Sie müssen über ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs** verfügen. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.

## <a name="running-the-walkthrough"></a>Ausführen der exemplarischen Vorgehensweise

Legen Sie `subscription_key` auf Ihren API-Schlüssel für den Bing-API-Dienst fest.


```python
subscription_key = None
assert subscription_key
```

Als Nächstes überprüfen Sie, ob der `search_url`-Endpunkt richtig ist. Zu dem Zeitpunkt, als dieser Artikel geschrieben wurde, wurde nur ein Endpunkt für Bing-Suche-APIs verwendet. Wenn Ihnen Autorisierungsfehler auffallen, vergleichen Sie diesen Wert mit dem Bing-Suche-Endpunkt in Ihrem Azure-Dashboard.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Legen Sie `search_term` fest, um Bing nach Microsoft Cognitive Services abzufragen.


```python
search_term = "Microsoft Cognitive Services"
```

Der folgende Block verwendet die `requests`-Bibliothek in Python, um die Bing-Suche-APIs aufzurufen und die Ergebnisse als JSON-Objekt zurückzugeben. Beachten Sie, dass wir den API-Schlüssel über das `headers`-Wörterbuch übergeben und den Suchbegriff über das `params`-Wörterbuch. Die vollständige Liste der zum Filtern von Suchergebnissen verwendbaren Optionen finden Sie in der [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)-Dokumentation.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

Das `search_results`-Objekt enthält die Suchergebnisse zusammen mit umfassenden Metadaten wie verwandten Abfragen und Seiten. Die folgenden Codezeilen formatieren die ersten von der Abfrage zurückgegebenen Seiten.


```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Einseitige Web-App für die Bing-Websuche](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Weitere Informationen 

[Übersicht zur Bing-Websuche](../overview.md)  
[Testen](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Holen Sie sich einen Zugriffsschlüssel einer kostenlosen Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Referenz zur Bing-Websuche-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
