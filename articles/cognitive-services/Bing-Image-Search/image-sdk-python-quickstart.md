---
title: Python-Schnellstart für das Bildersuche-SDK | Microsoft-Dokumentation
description: Einrichten des Bildersuche-SDK für die Konsolenanwendung
titleSuffix: Azure Image Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: e30852439ad8ec2d5ddc667b75167e8b5d35be33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377891"
---
# <a name="image-search-sdk-python-quickstart"></a>Python-Schnellstart für das Bildersuche-SDK

Das Bing-Videosuche-SDK enthält die Funktionalität der REST-API für Webabfragen und das Analysieren von Ergebnissen. 

Der [Python-Beispielquellcode für das Bing-Bildersuche-SDK](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image_search_samples.py) ist auf GitHub verfügbar.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten
Installieren Sie Python, falls Sie dies noch nicht getan haben. Das SDK ist mit Python 2.7, 3.3, 3.4, 3.5 und 3.6 kompatibel.

Allgemein wird für die Python-Entwicklung die Verwendung einer [virtuellen Umgebung](https://docs.python.org/3/tutorial/venv.html) empfohlen. Installieren und initialisieren Sie die virtuelle Umgebung mit dem [venv-Modul](https://pypi.python.org/pypi/virtualenv). Sie müssen virtualenv für Python 2.7 installieren.
```
python -m venv mytestenv
```
Installieren Sie Abhängigkeiten des Bing-Bildersuche-SDK:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-imagesearch
```
## <a name="image-search-client"></a>Client für die Bildersuche
Rufen Sie unter *Suchen* einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab. Fügen Sie Importe hinzu:
```
from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
from azure.cognitiveservices.search.imagesearch.models import ImageType, ImageAspect, ImageInsightModule
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Erstellen Sie eine Instanz von `CognitiveServicesCredentials`, und instanziieren Sie den Client:
```
client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Suchbilder zu einer Abfrage („Yosemite“), die nach animierten GIFs und Bildern im Breitformat gefiltert werden. Prüfen Sie die Anzahl der Ergebnisse, und geben Sie den insightsToken-Parameter, die URL zum Miniaturbild und die Web-URL des ersten Ergebnisses aus.
```
image_results = client.images.search(
        query="Yosemite",
        image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
        aspect=ImageAspect.wide # Could be the str "Wide"
    )
    print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

    if image_results.value:
        first_image_result = image_results.value[0]
        print("Image result count: {}".format(len(image_results.value)))
        print("First image insights token: {}".format(first_image_result.image_insights_token))
        print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
        print("First image web search url: {}".format(first_image_result.web_search_url))
    else:
        print("Couldn't find image results!")

```
Suchbilder zu einer Abfrage („Yosemite“), die nach animierten GIFs und Bildern im Breitformat gefiltert werden.  Prüfen Sie die Anzahl der Ergebnisse.  Geben Sie die Elemente `insightsToken`, `thumbnail url` und `web url` für das erste Ergebnis aus.
```
image_results = client.images.search(
    query="Yosemite",
    image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
    aspect=ImageAspect.wide # Could be the str "Wide"
)
print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

if image_results.value:
    first_image_result = image_results.value[0]
    print("Image result count: {}".format(len(image_results.value)))
    print("First image insights token: {}".format(first_image_result.image_insights_token))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image web search url: {}".format(first_image_result.web_search_url))
else:
    print("Couldn't find image results!")

```

Abrufen beliebter Ergebnisse:
```
trending_result = client.images.trending()
print("\r\nSearch trending images")

# Categorires
if trending_result.categories:
    first_category = trending_result.categories[0]
    print("Category count: {}".format(len(trending_result.categories)))
    print("First category title: {}".format(first_category.title))
    if first_category.tiles:
        first_tile = first_category.tiles[0]
        print("Subcategory tile count: {}".format(len(first_category.tiles)))
        print("First tile text: {}".format(first_tile.query.text))
        print("First tile url: {}".format(first_tile.query.web_search_url))
    else:
        print("Couldn't find subcategory tiles!")
    else:
        print("Couldn't find categories!")

```

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


