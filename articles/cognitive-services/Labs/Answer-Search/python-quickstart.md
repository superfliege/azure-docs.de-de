---
title: Python-Schnellstart für Microsoft Cognitive Services, Project Answer Search | Microsoft-Dokumentation
description: Python-Beispiel für die ersten Schritte mit Project Answer Search, Microsoft Cognitive Services in Azure
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9cb5406c616ed8e96d73c00c788a0d20f66dcabd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376138"
---
# <a name="project-answer-search-python-quickstart"></a>Python-Schnellstart für Project Answer Search

Das folgende Python-Beispiel veranschaulicht das Erstellen und Senden einer Anforderung für Informationen zu „Felsen von Gibraltar“.

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://aka.ms/answersearchsubscription) ab.

In diesem Beispiel wird Python 3.6.4 verwendet.

## <a name="code-scenario"></a>Codeszenario 

Der folgende Code erstellt eine URL-Vorschau.
Er wird folgendermaßen implementiert:
1. Deklarieren Sie Variablen, um den Endpunkt nach Host und Pfad anzugeben.
2. Geben Sie die Abfrage-URL zum Anzeigen einer Vorschau an, und fügen Sie den Abfrageparameter hinzu.  
3. Legen Sie den Abfrageparameter fest.
4. Definieren Sie die Suchfunktion, die die Anforderung erstellt und den Header *Ocp-Apim-Subscription-Key* hinzufügt.
5. Legen Sie den Header *Ocp-Apim-Subscription-Key* fest. 
6. Stellen Sie die Verbindung her, und senden Sie die Anforderung.
7. Drucken Sie die JSON-Ergebnisse.

Der vollständige Code für diese Demo sieht wie folgt aus:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'https://api.labs.cognitive.microsoft.com'
path = '/answerSearch/v7.0/search '

query = 'Rock of Gibraltar'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

````
## <a name="next-steps"></a>Nächste Schritte
- [C#-Schnellstart](c-sharp-quickstart.md)
- [Java-Schnellstart](java-quickstart.md)
- [Node-Schnellstart](node-quickstart.md)