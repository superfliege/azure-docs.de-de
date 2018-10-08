---
title: 'Schnellstart: SDK für die visuelle Bing-Suche, Python'
titleSuffix: Azure Cognitive Services
description: Setup für die Python SDK-Konsolenanwendung für die thematische Suche.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 06/11/2018
ms.author: v-gedod
ms.openlocfilehash: 269eaccbf834646b540123dfeeeec7c569b8ced4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222640"
---
# <a name="quickstart-bing-visual-search-sdk-python"></a>Schnellstart: SDK für die visuelle Bing-Suche, Python

Das Bing SDK für die thematische Suche nutzt die Funktionen der REST-API für Webanforderungen und Ergebnisanalysen.
[Python-Beispielquellcode für das Bing-SDK für die thematische Suche](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/visual_search_samples.py) ist bei GitHub verfügbar.

Codeszenarien sind unter den folgenden Themen dokumentiert:
* [Client für die visuelle Suche](#client)
* [Vollständige Konsolenanwendung](#complete-console)
* [post-Anforderung für Bildbinärdatei mit „cropArea“](#binary-crop)
* [KnowledgeRequest-Parameter](#knowledge-req)
* [Tags, Aktionen und Aktionstyp](#tags-actions)

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten
* Für die Authentifizierung von SDK-Aufrufen ist ein Cognitive Services-API-Schlüssel erforderlich. Registrieren Sie sich für einen [kostenlosen Testschlüssel](https://azure.microsoft.com/try/cognitive-services/?api=search-api-v7). Der Testschlüssel ist sieben Tage lang gültig und auf einen Aufruf pro Sekunde beschränkt. Für ein Produktionsszenario können Sie einen [Zugriffsschlüssel](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7) kaufen. Weitere Details finden Sie in den [Preisinformationen](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/visual/).
* Installieren Sie Python, falls Sie es noch nicht getan haben. Das SDK ist mit Python 2.7, 3.3, 3.4, 3.5 und 3.6 kompatibel.
* Allgemein wird für die Python-Entwicklung die Verwendung einer [virtuellen Umgebung](https://docs.python.org/3/tutorial/venv.html) empfohlen. Installieren und initialisieren Sie die virtuelle Umgebung mit dem [venv-Modul](https://pypi.python.org/pypi/virtualenv). Installieren Sie virtualenv für Python 2.7.
```
python -m venv mytestenv
```
Installieren Sie Abhängigkeiten des Bing-SDK für die thematische Suche:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-visualsearch
```

<a name="client"></a> 
## <a name="visual-search-client"></a>Client für die thematische Suche
Um eine Instanz des `VisualSearchAPI`-Clients zu erstellen, importieren Sie die folgenden Bibliotheken:
```
import http.client, urllib.parse
import json
import os.path
from azure.cognitiveservices.search.visualsearch import VisualSearchAPI
from azure.cognitiveservices.search.visualsearch.models import (
    VisualSearchRequest,
    CropArea,
    ImageInfo,
    Filters,
    KnowledgeRequest,
)
```
Ersetzen Sie den Zeichenfolgenwert von „subscriptionKey“ durch Ihren gültigen Abonnementschlüssel.
```
subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'
```
Instanziieren Sie dann den Client:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
```
Verwenden Sie den Client, um nach Bildern zu suchen, und analysieren Sie die Ergebnisse:
```
PATH = 'C:\\Users\\USER\\azure-cognitive-samples\\mytestenv\\TestImages\\'
image_path = os.path.join(PATH, "image.jpg")

with open(image_path, "rb") as image_fd:

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(VisualSearchRequest().serialize())

    print("\r\nSearch visual search request with binary of dog image")
    result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
        
    if not result:
        print("No visual search result data.")

        # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")

```

<a name="complete-console"></a> 
## <a name="complete-console-application"></a>Vollständige Konsolenanwendung

Die folgende Konsolenanwendung führt die zuvor definierte Abfrage aus und analysiert die Ergebnisse:
```
import http.client, urllib.parse
import json
import os.path

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'

PATH = 'C:\\Users\\v-gedod\\azure-cognitive-samples\\mytestenv\\TestImages\\'

from azure.cognitiveservices.search.visualsearch import VisualSearchAPI
from azure.cognitiveservices.search.visualsearch.models import (
    VisualSearchRequest,
    CropArea,
    ImageInfo,
    Filters,
    KnowledgeRequest,)

from msrest.authentication import CognitiveServicesCredentials

client = VisualSearchAPI(CognitiveServicesCredentials(subscription_key))

image_path = os.path.join(PATH, "image.jpg")

with open(image_path, "rb") as image_fd:

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(VisualSearchRequest().serialize())

    print("\r\nSearch visual search request with binary of dog image")
    result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
        
    if not result:
        print("No visual search result data.")

        # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")


# Uncomment these methods to include code under the following headings of this documentation.
#search_image_binary_with_crop_area(client, subscription_key, PATH)
#search_url_with_filters(client, subscription_key)
#search_insights_token_with_crop_area(client, subscription_key)

```

Die Beispiele für die Bing-Suche veranschaulichen verschiedene Features des SDK.  Fügen Sie der zuvor definierten Klasse `VisualSrchSDK` die folgenden Funktionen hinzu.

<a name="binary-crop"></a>
## <a name="image-binary-post-with-croparea"></a>post-Anforderung für Bildbinärdatei mit „cropArea“

Der folgende Code sendet eine Bildbinärdatei im Hauptteil der post-Anforderung sowie ein cropArea-Objekt.  Anschließend werden „imageInsightsToken“, die Anzahl von Tags, die Anzahl von Aktionen und der erste Aktionstyp ausgegeben.

```
def search_image_binary_with_crop_area(client, sub_key, file_path):

    #client = VisualSearchAPI(CognitiveServicesCredentials(sub_key))

    image_path = os.path.join(file_path, "image.jpg")
    with open(image_path, "rb") as image_fd:

        crop_area = CropArea(top=0.1,bottom=0.5,left=0.1,right=0.9)
        knowledge_request = VisualSearchRequest(image_info=ImageInfo(crop_area=crop_area))

        # You need to pass the serialized form of the model
        knowledge_request = json.dumps(knowledge_request.serialize())

        print("\r\nSearch visual search request with binary of dog image")
        result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)

        if not result:
            print("No visual search result data.")
            return

        # Visual Search results
        if result.image.image_insights_token:
            print("Uploaded image insights token: {}".format(result.image.image_insights_token))
        else:
            print("Couldn't find image insights token!")

        # List of tags
        if result.tags:
            first_tag = result.tags[0]
            print("Visual search tag count: {}".format(len(result.tags)))

            # List of actions in first tag
            if first_tag.actions:
                first_tag_action = first_tag.actions[0]
                print("First tag action count: {}".format(len(first_tag.actions)))
                print("First tag action type: {}".format(first_tag_action.action_type))
            else:
                print("Couldn't find tag actions!")
        else:
            print("Couldn't find image tags!")


```
<a name="knowledge-req"></a>
## <a name="knowledgerequest-parameter"></a>KnowledgeRequest-Parameter

Der folgende Code sendet eine Bild-URL im `knowledgeRequest`-Parameter sowie einen Filter vom Typ \"site:www.bing.com\". Anschließend werden `imageInsightsToken`, die Anzahl von Tags, die Anzahl von Aktionen und der erste Aktionstyp ausgegeben.
```
def search_url_with_filters(client_in, sub_key):

    client = client_in

    image_url = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80"
    filters = Filters(site="www.bing.com")

    knowledge_request = VisualSearchRequest(
        image_info=ImageInfo(url=image_url),
        knowledge_request=KnowledgeRequest(filters=filters)
    )

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(knowledge_request.serialize())

    print("\r\nSearch visual search request with url of dog image")
    result = client.images.visual_search(knowledge_request=knowledge_request)

    if not result:
        print("No visual search result data.")
        return

    # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")

```
<a name="tags-actions"></a>
## <a name="tags-actions-and-actiontype"></a>Tags, Aktionen und Aktionstyp

Der folgende Code sendet ein Bildauswertungstoken im knowledgeRequest-Parameter sowie ein cropArea-Objekt. Anschließend werden „imageInsightsToken“, die Anzahl von Tags, die Anzahl von Aktionen und der erste Aktionstyp ausgegeben.

```
    client = client_in

    image_insights_token = "bcid_113F29C079F18F385732D8046EC80145*ccid_oV/QcH95*mid_687689FAFA449B35BC11A1AE6CEAB6F9A9B53708*thid_R.113F29C079F18F385732D8046EC80145"
    crop_area = CropArea(top=0.1,bottom=0.5,left=0.1,right=0.9)

    knowledge_request = VisualSearchRequest(
        image_info=ImageInfo(
            image_insights_token=image_insights_token,
            crop_area=crop_area
        ),
    )

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(knowledge_request.serialize())

    print("\r\nSearch visual search request with URL of dog image")
    result = client.images.visual_search(knowledge_request=knowledge_request)

    if not result:
        print("No visual search result data.")
        return

    # Visual Search results
    if result.image.image_insights_token:
        print("Uploaded image insights token: {}".format(result.image.image_insights_token))
    else:
        print("Couldn't find image insights token!")

    # List of tags
    if result.tags:
        first_tag = result.tags[0]
        print("Visual search tag count: {}".format(len(result.tags)))

        # List of actions in first tag
        if first_tag.actions:
            first_tag_action = first_tag.actions[0]
            print("First tag action count: {}".format(len(first_tag.actions)))
            print("First tag action type: {}".format(first_tag_action.action_type))
        else:
            print("Couldn't find tag actions!")
    else:
        print("Couldn't find image tags!")
```

## <a name="next-steps"></a>Nächste Schritte

[Cognitive services .NET SDK samples (Cognitive Services SDK-Beispiele für .NET)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)