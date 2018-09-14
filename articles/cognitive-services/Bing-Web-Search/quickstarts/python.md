---
title: 'Schnellstart: Verwenden von Python zum Aufrufen der Bing-Websuche-API'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie die Bing-Websuche-API zum ersten Mal aufrufen, indem Sie Python verwenden, und wie Sie eine JSON-Antwort erhalten.
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 8/16/2018
ms.author: erhopf
ms.openlocfilehash: cd53a323a07617284e82004a6b3feed57b6e15e2
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888606"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Schnellstart: Verwenden von Python zum Aufrufen der Bing-Websuche-API  

Verwenden Sie diese Schnellstartanleitung, um die Bing-Websuche-API zum ersten Mal aufzurufen und innerhalb von weniger als zehn Minuten eine JSON-Antwort zu erhalten.  

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

Dieses Beispiel wird als Jupyter-Notebook unter [MyBinder](https://mybinder.org) ausgeführt. Klicken Sie auf den Badge zum Starten von Binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="define-variables"></a>Definieren von Variablen

Ersetzen Sie den Wert von `subscription_key` durch einen gültigen Abonnementschlüssel aus Ihrem Azure-Konto.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Deklarieren Sie den Endpunkt für die Bing-Websuche-API. Wenn Autorisierungsfehler auftreten, sollten Sie diesen Wert mit dem Endpunkt für die Bing-Suche in Ihrem Azure-Dashboard vergleichen.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Sie können die Suchabfrage auch anpassen, indem Sie den Wert für `search_term` ersetzen.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>Erstellen einer Anforderung

In diesem Block wird die `requests`-Bibliothek verwendet, um die Bing-Websuche-API aufzurufen und die Ergebnisse als JSON-Objekt zurückzugeben. Der API-Schlüssel wird im `headers`-Wörterbuch übergeben, und der Suchbegriff und die Abfrageparameter werden im `params`-Wörterbuch übergeben. Eine vollständige Liste mit Optionen und Parametern finden Sie in der Dokumentation zur [Bing-Websuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).

```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Formatieren und Anzeigen der Antwort

Das `search_results`-Objekt enthält die Suchergebnisse und Metadaten, z.B. verwandte Abfragen und Seiten. In diesem Code wird die `IPython.display`-Bibliothek verwendet, um die Antwort in Ihrem Browser zu formatieren und anzuzeigen.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Beispielcode auf GitHub

Wenn Sie diesen Code lokal ausführen möchten, können Sie das vollständige [Beispiel auf GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js) verwenden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Einseitige Web-App für die Bing-Websuche](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
