---
title: Python-Schnellstart für Project URL Preview – Microsoft Cognitive Services | Microsoft Docs
description: Skriptbeispiel für den schnellen Einstieg in das Project URL Preview in Microsoft Cognitive Services in Azure
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 78b2d83b02aa9ea32509029c7456e04e420b8572
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376250"
---
# <a name="url-preview-python-quickstart"></a>Python-Schnellstart für die URL-Vorschau

Im folgenden Python-Beispiel wird eine URL-Vorschau für die SwiftKey-Website erstellt: https://swiftkey.com/en.

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://aka.ms/answersearchsubscription) ab.

In diesem Beispiel wird Python 3.6 verwendet.

## <a name="code-scenario"></a>Codeszenario 

Der folgende Code erstellt eine URL-Vorschau.
Er wird in den folgenden Schritten implementiert:
1. Deklarieren Sie Variablen zum Angeben des Endpunkts nach Host und Pfad.
2. Geben Sie die Abfrage-URL zum Anzeigen einer Vorschau an, und fügen Sie den Abfrageparameter hinzu.  
3. Legen Sie den Abfrageparameter fest.
4. Definieren Sie die Suchfunktion, mit der die Anforderung erstellt und der Header *Ocp-Apim-Subscription-Key* hinzugefügt wird.
5. Legen Sie den Header *Ocp-Apim-Subscription-Key* fest. 
6. Stellen Sie die Verbindung her, und senden Sie die Anforderung.
7. Drucken Sie die JSON-Ergebnisse.

Der vollständige Code für diese Demo sieht wie folgt aus:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'your-subscription-key'

host = 'api.labs.cognitive.microsoft.com'
path = '/urlpreview/v7.0/search'

query = 'https://SwiftKey.com'

params = '?q=' + urllib.parse.quote (query)

def get_preview ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_preview ()
print (json.dumps(json.loads(result), indent=4))
````
## <a name="next-steps"></a>Nächste Schritte
- [C#-Schnellstart](csharp.md)
- [Java-Schnellstart](java-quickstart.md)
- [JavaScript-Schnellstart](javascript.md)
- [Node-URL-Schnellstart](node-quickstart.md)