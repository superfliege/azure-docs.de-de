---
title: 'Schnellstart: Aufrufen eines Endpunkts mit Node.js – benutzerdefinierte Bing-Suche'
titlesuffix: Azure Cognitive Services
description: Dieser Schnellstart veranschaulicht das Anfordern von Suchergebnissen aus Ihrer Instanz der benutzerdefinierten Suche unter Verwendung von Node.js zum Aufrufen des Endpunkts der benutzerdefinierten Bing-Suche.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: c0c97dd52f8fc3ff590c86f32f794beeb00f4b05
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52310251"
---
# <a name="quickstart-call-bing-custom-search-endpoint-nodejs"></a>Schnellstart: Aufrufen eines Endpunkts für die benutzerdefinierte Bing-Suche (Node.js)

Dieser Schnellstart veranschaulicht das Anfordern von Suchergebnissen aus Ihrer Instanz der benutzerdefinierten Suche unter Verwendung von Node.js zum Aufrufen des Endpunkts der benutzerdefinierten Bing-Suche. 

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Eine einsatzbereite Instanz für die benutzerdefinierte Suche. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).
- Installation von [Node.js](https://www.nodejs.org/)
- Abonnementschlüssel Sie erhalten einen Abonnementschlüssel, wenn Sie Ihre [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) aktivieren, oder Sie können einen kostenpflichtigen Abonnementschlüssel über Ihr Azure-Dashboard beziehen (siehe [Schnellstart: Erstellen eines Cognitive Services-API-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).   Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie diese Schritte aus, um das Beispiel auszuführen:

1. Erstellen Sie einen Ordner für Ihren Code.  
  
2. Navigieren Sie an einer Eingabeaufforderung oder einem Terminal zu dem Ordner, den Sie gerade erstellt haben.  
  
3. Installieren Sie das Modul zum **Anfordern** eines Knotens:
    <pre>
    npm install request
    </pre>  
    
4. Erstellen Sie eine Datei mit dem Namen „BingCustomSearch.js“ in dem von Ihnen erstellten Ordner, und kopieren Sie den folgenden Code in diese. Ersetzen Sie **YOUR-SUBSCRIPTION-KEY** und **YOUR-CUSTOM-CONFIG-ID** durch Ihren Abonnementschlüssel und Ihre Konfigurations-ID.  
  
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
