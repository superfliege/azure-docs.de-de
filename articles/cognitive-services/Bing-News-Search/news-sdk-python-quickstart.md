---
title: 'Schnellstart: Bing-News-Suche-SDK, Python'
titleSuffix: Azure Cognitive Services
description: Setup für die Konsolenanwendung des Bing-News-Suche-SDK
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 8e4343b053835c0fc2219373ad60f96c7b80636a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803340"
---
# <a name="quickstart-bing-news-search-sdk-with-python"></a>Schnellstart: Bing-News-Suche-SDK mit Python

Das SDK für die News-Suche enthält die Funktionalität der REST-API für Webabfragen und das Analysieren von Ergebnissen. 

Der [Python-Beispielquellcode für das SDK für die Bing-News-Suche](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py) ist auf GitHub verfügbar.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten
Installieren Sie Python, falls noch nicht geschehen. Das SDK ist mit Python 2.7, 3.3, 3.4, 3.5 und 3.6 kompatibel.

Allgemein wird für die Python-Entwicklung die Verwendung einer [virtuellen Umgebung](https://docs.python.org/3/tutorial/venv.html) empfohlen. Installieren und initialisieren Sie die virtuelle Umgebung mit dem [venv-Modul](https://pypi.python.org/pypi/virtualenv). Sie müssen virtualenv für Python 2.7 installieren.
```
python -m venv mytestenv
```
Installieren Sie Abhängigkeiten für das SDK für die Bing-News-Suche:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-newssearch
```
## <a name="news-search-client"></a>Client für die News-Suche
Rufen Sie unter *Suchen* einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab. Fügen Sie Importe hinzu:
```
from azure.cognitiveservices.search.newssearch import NewsSearchAPI
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Erstellen Sie eine Instanz von `CognitiveServicesCredentials`. Instanziieren Sie den Client:
```
client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Suchen Sie nach Ergebnissen, und geben Sie das erste Webseitenergebnis aus:
```
news_result = client.news.search(query="Quantum Computing", market="en-us", count=10)
print("Search news for query \"Quantum Computing\" with market and count")

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
Suchen Sie mit Filtern über die Parameter `freshness` und `sortBy` nach den neusten Meldungen zum Stichwort „Künstliche Intelligenz“. Überprüfen Sie die Anzahl der Ergebnisse, und geben Sie die Elemente `totalEstimatedMatches`, `name`, `url`, `description`, `published time` und `name of provider` für das erste Neuigkeitenelement aus.
```
def news_search_with_filtering(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.search(
            query="Artificial Intelligence",
            market="en-us",
            freshness="Week",
            sort_by="Date"
        )
        print("\r\nSearch most recent news for query \"Artificial Intelligence\" with freshness and sortBy")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Durchsuchen Sie die Kategorie „Neuigkeiten“ für Film- und Fernsehunterhaltung mithilfe einer sicheren Suche. Überprüfen Sie die Anzahl der Ergebnisse, und geben Sie die Elemente `category`, `name`, `url`, `description`, `published time` und `name of provider` für das erste Neuigkeitenelement aus.
```
def news_category(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.category(
            category="Entertainment_MovieAndTV",
            market="en-us",
            safe_search="strict"
        )
        print("\r\nSearch category news for movie and TV entertainment with safe search")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news category: {}".format(first_news_result.category))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))


```
Suchen Sie nach beliebten Neuigkeiten in Bing.  Überprüfen Sie die Anzahl der Ergebnisse, und geben Sie die Elemente `name`, `text of query`, `webSearchUrl`, `newsSearchUrl` und `image Url` für das erste Neuigkeitenergebnis aus.
```
def news_trending(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_topics = client.news.trending(market="en-us")
        print("\r\nSearch news trending topics in Bing")

        if trending_topics.value:
            first_topic = trending_topics.value[0]
            print("News result count: {}".format(len(trending_topics.value)))
            print("First topic name: {}".format(first_topic.name))
            print("First topic query: {}".format(first_topic.query.text))
            print("First topic image url: {}".format(first_topic.image.url))
            print("First topic webSearchUrl: {}".format(first_topic.web_search_url))
            print("First topic newsSearchUrl: {}".format(first_topic.news_search_url))
        else:
            print("Didn't see any topics result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


