---
title: 'Schnellstart: Bing-Rechtschreibprüfungs-API, Python'
titlesuffix: Azure Cognitive Services
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Bing-Rechtschreibprüfungs-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: d77fd2637e7516dbae361518c4b729ca1217e6e6
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160211"
---
# <a name="quickstart-for-bing-spell-check-api-with-python"></a>Schnellstart für die Bing-Rechtschreibprüfungs-API mit Python 

In diesem Artikel erfahren Sie, wie Sie die [Bing-Rechtschreibprüfungs-API](https://azure.microsoft.com/services/cognitive-services/spell-check/) mit Python verwenden. Die Rechtschreibprüfungs-API gibt eine Liste mit nicht erkannten Wörtern und Ersetzungsvorschlägen zurück. Normalerweise übergeben Sie Text an die API und nehmen dann entweder die vorgeschlagenen Ersetzungen im Text vor oder zeigen sie für den Benutzer Ihrer Anwendung an, damit er selbst entscheiden kann, ob er die Ersetzungen vornehmen möchte. In diesem Artikel wird erläutert, wie Sie eine Anforderung mit dem Text „Hollo, wrld!“ senden. Die vorgeschlagenen Ersetzungen sind „Hello“ und „world“.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Codes benötigen Sie [Python 3.x](https://www.python.org/downloads/).

Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit der **Bing-Rechtschreibprüfungs-API v7**. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/#lang) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.

## <a name="get-spell-check-results"></a>Abrufen der Ergebnisse der Rechtschreibprüfung

1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Python-Projekt.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `subscriptionKey`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```python
import http.client, urllib.parse, json

text = 'Hollo, wrld!'

data = {'text': text}

# NOTE: Replace this example key with a valid subscription key.
key = 'ENTER KEY HERE'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/spellcheck?'
params = 'mkt=en-us&mode=proof'

headers = {'Ocp-Apim-Subscription-Key': key,
'Content-Type': 'application/x-www-form-urlencoded'}

# The headers in the following example 
# are optional but should be considered as required:
#
# X-MSEdge-ClientIP: 999.999.999.999  
# X-Search-Location: lat: +90.0000000000000;long: 00.0000000000000;re:100.000000000000
# X-MSEdge-ClientID: <Client ID from Previous Response Goes Here>

conn = http.client.HTTPSConnection(host)
body = urllib.parse.urlencode (data)
conn.request ("POST", path + params, body, headers)
response = conn.getresponse ()
output = json.dumps(json.loads(response.read()), indent=4)
print (output)
```

**Antwort**

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial zur Bing-Rechtschreibprüfung](../tutorials/spellcheck.md)

## <a name="see-also"></a>Weitere Informationen

- [Übersicht über die Bing-Rechtschreibprüfung](../proof-text.md)
- [Referenz zur Bing-Rechtschreibprüfungs-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
