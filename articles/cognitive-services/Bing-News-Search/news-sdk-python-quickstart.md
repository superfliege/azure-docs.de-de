---
title: 'Schnellstart: Durchführen einer News-Suche – Bing-News-Suche-SDK für Python'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um mit dem Bing-News-Suche-SDK für Python nach Nachrichten zu suchen und die Antwort zu verarbeiten.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: v-gedod
ms.custom: seodec2018
ms.openlocfilehash: 1d6f4bb2bda550c72566243b2d2d98b1685e9147
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58119801"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-python"></a>Schnellstart: Durchführen einer News-Suche mit dem Bing-News-Suche-SDK für Python

Verwenden Sie diese Schnellstartanleitung, um unter Verwendung des Bing-News-Suche-SDK für Python mit der Suche nach Nachrichten zu beginnen. Die Bing-News-Suche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, aber das SDK ist eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py).

## <a name="prerequisites"></a>Voraussetzungen

* [Python](https://www.python.org/) 2.x oder 3.x

Für die Python-Entwicklung wird die Verwendung einer [virtuellen Umgebung](https://docs.python.org/3/tutorial/venv.html) empfohlen. Sie können die virtuelle Umgebung mit dem [venv-Modul](https://pypi.python.org/pypi/virtualenv) installieren und initialisieren. Sie müssen virtualenv für Python 2.7 installieren. Sie können wie folgt eine virtuelle Umgebung erstellen:

```console
python -m venv mytestenv
```

Sie können die Abhängigkeiten des SDK für die Bing-News-Suche mit dem folgenden Befehl installieren:
    
```console
python -m pip install azure-cognitiveservices-search-newssearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor eine neue Python-Datei, und importieren Sie die folgenden Bibliotheken. Erstellen Sie eine Variable für Ihren Abonnementschlüssel und einen Suchbegriff.

    ```python
    from azure.cognitiveservices.search.newssearch import NewsSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    search_term = "Quantum Computing"
    ```

## <a name="initialize-the-client-and-send-a-request"></a>Initialisieren des Clients und Senden einer Anforderung

1. Erstellen Sie eine Instanz von `CognitiveServicesCredentials`. Instanziieren Sie den Client:
    
    ```python
    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

2. Senden Sie eine Suchabfrage an die News-Suche-API, und speichern Sie die Antwort.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## <a name="parse-the-response"></a>Analysieren der Antwort

Werden Suchergebnisse gefunden, geben Sie das erste Webseitenergebnis aus:

```python
if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](tutorial-bing-news-search-single-page-app.md)
