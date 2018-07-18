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
ms.openlocfilehash: 889762ae0b401438f25546738268c584ddd58389
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "35378496"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Aufrufen des Endpunkts für die benutzerdefinierte Bing-Suche (Python)

Dieser Schnellstart veranschaulicht das Anfordern von Suchergebnissen aus Ihrer Instanz der benutzerdefinierten Suche unter Verwendung von Python zum Aufrufen des Endpunkts der benutzerdefinierten Bing-Suche. 

## <a name="prerequisites"></a>Voraussetzungen
Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Eine Instanz der benutzerdefinierten Suche. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).

-  Installation von [Python](https://www.python.org/)

- Ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **Bing-Suche-APIs**. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden. 

## <a name="run-the-code"></a>Ausführen des Codes

Um den Endpunkt der benutzerdefinierten Bing-Suche aufzurufen, gehen Sie folgendermaßen vor:

1. Erstellen Sie einen Ordner für Ihren Code.
2. Navigieren Sie an einer Administrator-Eingabeaufforderung oder einem Administrator-Terminal zu dem Ordner, den Sie gerade erstellt haben.
3. Installieren Sie das Python-Modul für **Anforderungen**:
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
7. Erstellen Sie die Datei „BingCustomSearch.py“, und kopieren Sie den folgenden Code in diese:
8. Ersetzen Sie **YOUR-SUBSCRIPTION-KEY** und **YOUR-CUSTOM-CONFIG-ID** durch Ihren Schlüssel bzw. Ihre Konfigurations-ID (siehe Schritt 1).

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
9. Führen Sie den Code mit dem folgenden Befehl aus:
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren der gehosteten Benutzeroberfläche](./hosted-ui.md)
- [Verwenden von Dekorationsmarkierungen zum Hervorheben von Text](./hit-highlighting.md)
- [Einteilen von Webseiten](./page-webpages.md)