---
title: 'Schnellstart: Bing-Videosuche-SDK, Node'
titleSuffix: Azure Cognitive Services
description: Setup für die Konsolenanwendung des Bing-Videosuche-SDK.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: rosh
ms.openlocfilehash: 4dcc9220d4d38bfe34514edd6a3ad47c7a7d4ba8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225615"
---
# <a name="quickstart-bing-video-search-sdk-with-node"></a>Schnellstart: Bing-Videosuche-SDK mit Node

Das Bing-Videosuche-SDK enthält die Funktionalität der REST-API für Videoabfragen und das Analysieren von Ergebnissen. 

[Beispielquellcode mit dem Bing-Videosuche-SDK für Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js) ist bei GitHub verfügbar.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten

Führen Sie zum Einrichten einer Konsolenanwendung mit dem Bing-Videosuche-SDK in Ihrer Entwicklungsumgebung `npm install azure-cognitiveservices-videosearch` aus.

## <a name="video-search-client"></a>Videosuche-Client
Rufen Sie unter *Suchen* einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Instanziieren Sie dann den Client:
```
const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
let client = new VideoSearchAPIClient(credentials);
```
Suchen Sie nach Ergebnissen.
```
client.videosOperations.search('Interstellar Trailer').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
