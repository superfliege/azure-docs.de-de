---
title: Aufrufen eines Endpunkts mit Python – benutzerdefinierte Bing-Suche – Microsoft Cognitive Services
description: Dieser Schnellstart veranschaulicht das Anfordern von Suchergebnissen aus Ihrer Instanz der benutzerdefinierten Suche unter Verwendung von Python zum Aufrufen des Endpunkts der benutzerdefinierten Bing-Suche.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 3666e92372e9bed80e5c0c7991dcac730cebb588
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967590"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Aufrufen des Endpunkts für die benutzerdefinierte Bing-Suche (Python)

Dieser Schnellstart veranschaulicht das Anfordern von Suchergebnissen aus Ihrer Instanz der benutzerdefinierten Suche unter Verwendung von Python zum Aufrufen des Endpunkts der benutzerdefinierten Bing-Suche. 

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Eine einsatzbereite Instanz für die benutzerdefinierte Suche. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).
- Installation von [Python](https://www.python.org/)
- Abonnementschlüssel Sie erhalten einen Abonnementschlüssel, wenn Sie Ihre [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) aktivieren, oder Sie können einen kostenpflichtigen Abonnementschlüssel über Ihr Azure-Dashboard beziehen (siehe [Schnellstart: Erstellen eines Cognitive Services-API-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    


## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie diese Schritte aus, um das Beispiel auszuführen:

1. Erstellen Sie einen Ordner für Ihren Code.  
  
2. Navigieren Sie an einer Administrator-Eingabeaufforderung oder einem Administrator-Terminal zu dem Ordner, den Sie gerade erstellt haben.  
  
3. Installieren Sie das Python-Modul für **Anforderungen**:  
  
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>  
      
4. Erstellen Sie eine Datei mit dem Namen „BingCustomSearch.py“ in dem von Ihnen erstellten Ordner, und kopieren Sie den folgenden Code in diese. Ersetzen Sie **YOUR-SUBSCRIPTION-KEY** und **YOUR-CUSTOM-CONFIG-ID** durch Ihren Abonnementschlüssel und Ihre Konfigurations-ID.  
  
    ``` Python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=' + searchTerm + '&customconfig=' + customConfigId
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```  
  
7. Führen Sie den Code mit dem folgenden Befehl aus:  
  
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren der gehosteten Benutzeroberfläche](./hosted-ui.md)
- [Verwenden von Decorator-Markierungen zum Hervorheben von Text](./hit-highlighting.md)
- [Einteilen von Webseiten](./page-webpages.md)
