---
title: 'Schnellstart: Bing-Entitätssuche-SDK, Python'
titlesuffix: Azure Cognitive Services
description: Setup für die Konsolenanwendung des Bing-Entitätssuche-SDK
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 50620eba35be136e38d5b1f8c3083f9ddf189d9e
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757802"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Schnellstart: Bing-Entitätssuche-SDK mit Python

Verwenden Sie diese Schnellstartanleitung, um unter Verwendung des Bing-Entitätssuche-SDK für Python mit der Suche nach Entitäten zu beginnen. Die Bing-Entitätssuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, aber das SDK ist eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py).

## <a name="prerequisites"></a>Voraussetzungen

* Python [2.x oder 3.x](https://www.python.org/)

* Das [Bing-Entitätssuche-SDK für Python](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

Es wird empfohlen, eine virtuelle Python-Umgebung zu verwenden. Sie können eine virtuelle Umgebung mit dem venv-Modul installieren und initialisieren. „virtualenv“ für Python 2.7 kann wie folgt installiert werden:

```Console
python -m venv mytestenv
```

Installieren Sie das Bing-Entitätssuche-SDK wie folgt:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE bzw. in einem Editor eine neue Python-Datei, und fügen Sie die folgenden Importanweisungen hinzu. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchAPI
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Erstellen Sie eine Variable für Ihren Abonnementschlüssel, und instanziieren Sie den Client, indem Sie mit ihm ein neues Objekt vom Typ `CognitiveServicesCredentials` erstellen.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Senden einer Suchanforderung und Erhalten einer Antwort

1. Senden Sie an die Bing-Entitätssuche eine Suchanforderung mit `client.entities.search()` und einer Suchabfrage. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Falls Entitäten zurückgegeben wurden, konvertieren Sie `entity_data.entities.value` in eine Liste, und geben Sie das erste Ergebnis aus.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](../tutorial-bing-entities-search-single-page-app.md)

* [Was ist die Bing-Entitätssuche-API?](../overview.md )