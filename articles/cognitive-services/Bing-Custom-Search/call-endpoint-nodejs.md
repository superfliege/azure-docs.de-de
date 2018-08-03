---
title: Aufrufen eines Endpunkts mit Node.js – benutzerdefinierte Bing-Suche – Microsoft Cognitive Services
description: Dieser Schnellstart veranschaulicht das Anfordern von Suchergebnissen aus Ihrer Instanz der benutzerdefinierten Suche unter Verwendung von Node.js zum Aufrufen des Endpunkts der benutzerdefinierten Bing-Suche.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 5d9391cc486dc868a1a291ccc7095291cddd3e4c
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858458"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Aufrufen des Endpunkts für die benutzerdefinierte Bing-Suche (Node.js)

Dieser Schnellstart veranschaulicht das Anfordern von Suchergebnissen aus Ihrer Instanz der benutzerdefinierten Suche unter Verwendung von Node.js zum Aufrufen des Endpunkts der benutzerdefinierten Bing-Suche. 

## <a name="prerequisites"></a>Voraussetzungen
Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Eine Instanz der benutzerdefinierten Suche. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).

- Installation von [Node.js](https://www.nodejs.org/)

-  Ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs** Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.

## <a name="run-the-code"></a>Ausführen des Codes

Um den Endpunkt der benutzerdefinierten Bing-Suche aufzurufen, gehen Sie folgendermaßen vor:

1. Erstellen Sie einen Ordner für Ihren Code.

2. Navigieren Sie an einer Eingabeaufforderung oder einem Terminal zu dem Ordner, den Sie gerade erstellt haben.

3. Installieren Sie das Modul zum **Anfordern** eines Knotens:
    <pre>
    npm install request
    </pre>
    
4. Erstellen Sie die Datei „BingCustomSearch.js“, und kopieren Sie den folgenden Code in die Datei.

5. Ersetzen Sie **YOUR-SUBSCRIPTION-KEY** und **YOUR-CUSTOM-CONFIG-ID** durch Ihren Schlüssel bzw. Ihre Konfigurations-ID (siehe Schritt 1).

    ``` javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    
    var options = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
          'q=' + searchTerm + 
          '&customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    
    request(options, function(error, response, body){
        var searchResponse = JSON.parse(body);
        for(var i = 0; i < searchResponse.webPages.value.length; ++i){
            var webPage = searchResponse.webPages.value[i];
            console.log('name: ' + webPage.name);
            console.log('url: ' + webPage.url);
            console.log('displayUrl: ' + webPage.displayUrl);
            console.log('snippet: ' + webPage.snippet);
            console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
            console.log();
        }
    })
    ```
6. Führen Sie den Code mit dem folgenden Befehl aus:
    ```    
    node BingCustomSearch.js
   ``` 

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren der gehosteten Benutzeroberfläche](./hosted-ui.md)
- [Verwenden von Decorator-Markierungen zum Hervorheben von Text](./hit-highlighting.md)
- [Einteilen von Webseiten](./page-webpages.md)
