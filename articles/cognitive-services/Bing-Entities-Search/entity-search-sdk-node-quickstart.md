---
title: 'Schnellstart: Bing-Entitätssuche-SDK, Node'
titleSuffix: Azure Cognitive Services
description: Setup für die Konsolenanwendung des Entitätssuche-SDK mit Node
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 0a61a6b1ada68307af7e7e574cba9910841f5939
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55153010"
---
# <a name="quickstart-bing-entity-search-sdk-with-node"></a>Schnellstart: Bing-Entitätssuche-SDK mit Node

Das Bing-Entitätssuche-SDK enthält die Funktionalität der REST-API für Entitätsabfragen und das Analysieren von Ergebnissen. 

[Beispielquellcode mit dem Bing-Entitätssuche-SDK für C#](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) ist bei GitHub verfügbar.
## <a name="application-dependencies"></a>Anwendungsabhängigkeiten
Rufen Sie unter **Suchen** einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab.  Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

So richten Sie eine Konsolenanwendung mit dem SDK für die Bing-Entitätssuche ein:
* Führen Sie `npm install ms-rest-azure` in Ihrer Entwicklungsumgebung aus.
* Führen Sie `npm install azure-cognitiveservices-entitysearch` in Ihrer Entwicklungsumgebung aus.

## <a name="entity-search-client"></a>Entitätssuche-Client
Rufen Sie unter *Suchen* einen [Cognitive Services-Zugriffsschlüssel](https://azure.microsoft.com/try/cognitive-services/) ab. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Instanziieren Sie dann den Client, und suchen Sie nach Ergebnissen:
```
const EntitySearchAPIClient = require('azure-cognitiveservices-entitysearch');

let entitySearchApiClient = new EntitySearchAPIClient(credentials);

entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
    console.log(result.queryContext);
    console.log(result.entities.value);
    console.log(result.entities.value[0].description);
}).catch((err) => {
    throw err;
});

```
Der Code gibt `result.value` Elemente an der Konsole aus, ohne Text zu analysieren.  Die Ergebnisse pro Kategorie (sofern vorhanden) schließen Folgendes ein:
- _type: 'Thing'
- _type: 'ImageObject'

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

[Cognitive Services SDK-Beispiele für Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
