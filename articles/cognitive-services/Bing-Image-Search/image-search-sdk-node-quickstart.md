---
title: Node-Schnellstart für das Bildersuche-SDK | Microsoft-Dokumentation
description: Setup für die Konsolenanwendung des Bildersuche-SDK
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e4c8303e39accbb7caec15c0ef47d701971ce632
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377899"
---
# <a name="image-search-sdk-node-quickstart"></a>Node-Schnellstart für das Bildersuche-SDK

Das Bing-Bildersuche-SDK enthält die Funktionalität der REST-API für Bilderabfragen und das Analysieren von Ergebnissen. 

Der [Beispielquellcode für das Bing-Bildersuche-SDK für Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) ist auf GitHub verfügbar.

## <a name="application-dependencies"></a>Anwendungsabhängigkeiten

Führen Sie zum Einrichten einer Konsolenanwendung mit dem Bing-Bildersuche-SDK in Ihrer Entwicklungsumgebung `npm install azure-cognitiveservices-imagesearch` aus.

## <a name="image-search-client"></a>Bildersuche-Client
Rufen Sie unter *Suchen* einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Instanziieren Sie dann den Client:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
Verwenden Sie den Client zur Suche mit einem Abfragetext, in diesem Fall „El Capitan“:
```
client.imagesOperations.search('El Capitan', function (err, result, request, response) {
    if (err) throw err;
    console.log(result.value);
});

```
<!-- Need to sanitize result
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'ImageObjectElementType'

![Imageresults](media/node-sdk-quickstart-image-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)