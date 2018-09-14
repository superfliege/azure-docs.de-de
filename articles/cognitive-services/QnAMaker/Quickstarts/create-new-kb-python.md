---
title: 'Erstellen einer neuen Wissensdatenbank: Python-Schnellstart für die Microsoft QnA Maker-API (V4) – Azure Cognitive Services | Microsoft-Dokumentation'
description: Erstellen Sie eine Wissensdatenbank in Python für Ihre häufig gestellten Fragen oder Produkthandbücher, um die ersten Schritte mit QnA Maker auszuführen.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: a8e3ae73c76c13d44267f221d37a07eea721a608
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "43769821"
---
# <a name="create-a-new-knowledge-base-in-python"></a>Erstellen einer neuen Wissensdatenbank in Python

Diese Schnellstartanleitung führt Sie durch das programmgesteuerte Erstellen einer Beispielwissensdatenbank für QnA Maker, die auf dem Azure-Dashboard Ihres Cognitive Services-API-Kontos angezeigt wird.

Weiter unten finden Sie zwei Beispiel-URLs für häufig gestellte Fragen (im **req**-Wörterbuchelement „urls“). QnA Maker extrahiert automatisch Fragen und Antworten aus teilweise strukturiertem Inhalt (etwa häufig gestellten Fragen). Dies wird in diesem Dokument zu [Datenquellen](../Concepts/data-sources-supported.md) ausführlicher erläutert. Sie können in diesem Schnellstart auch Ihre eigenen URLs für häufig gestellte Fragen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Codes benötigen Sie [Python 3.x](https://www.python.org/downloads/).

Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit **QnA Maker** als Ressource. Sie benötigen einen kostenpflichtigen Abonnementschlüssel aus Ihrem [Azure-Dashboard](https://ms.portal.azure.com/). Wählen Sie zum Abrufen des Schlüssels auf Ihrem Dashboard unter **Ressourcenverwaltung** die Option **Schlüssel** aus. Beide Schlüssel können in dieser Schnellstartanleitung verwendet werden.

![Dienstschlüssel auf dem Azure-Dashboard](../media/sub-key.png)

Weitere Informationen zu Visual Studio und Python finden Sie unter [Arbeiten mit Python in Visual Studio unter Windows](https://docs.microsoft.com/en-us/visualstudio/python/overview-of-python-tools-for-visual-studio).

## <a name="create-knowledge-base"></a>Erstellen einer Wissensdatenbank

Über den folgenden Code können Sie mithilfe der [Methode zum Erstellen einer Wissensdatenbank](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) eine neue Wissensdatenbank erstellen.

1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Python-Projekt.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den Wert `subscriptionKey` durch Ihren gültigen Abonnementschlüssel.
4. Führen Sie das Programm aus.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, json, time

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace this with a valid subscription key.
subscriptionKey = 'YOUR SUBSCRIPTION KEY HERE'

# Represents the various elements used to create HTTP request path
# for QnA Maker operations.
host = 'westus.api.cognitive.microsoft.com'
service = '/qnamaker/v4.0'
method = '/knowledgebases/create'

'''
Formats and indents JSON for display.
:param content: The JSON to format and indent.
:type: string
:return: A string containing formatted and indented JSON.
:rtype: string
'''
def pretty_print(content):
  # Note: We convert content to and from an object so we can pretty-print it.
  return json.dumps(json.loads(content), indent=4)

'''
Sends the POST request to create the knowledge base.
:param path: The URL path being called.
:type: string
:param content: The contents of your POST.
:type: string
:return: A header that creates the knowledge base, the JSON response
:rtype: string, string
'''
def create_kb(path, content):
  print('Calling ' + host + path + '.')
  headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-Type': 'application/json',
    'Content-Length': len (content)
  }
  conn = http.client.HTTPSConnection(host)
  conn.request ("POST", path, content, headers)
  response = conn.getresponse ()
  # /knowledgebases/create returns an HTTP header named Location that contains a URL
  # to check the status of the operation in creating the knowledge base.
  return response.getheader('Location'), response.read ()

'''
Checks the status of the request to create the knowledge base.
:param path: The URL path being checked
:type: string
:return: The header Retry-After if request is not finished, the JSON response
:rtype: string, string
'''
def check_status(path):
  print('Calling ' + host + path + '.')
  headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
  conn = http.client.HTTPSConnection(host)
  conn.request("GET", path, None, headers)
  response = conn.getresponse ()
  # If the operation is not finished, /operations returns an HTTP header named Retry-After
  # that contains the number of seconds to wait before we query the operation again.
  return response.getheader('Retry-After'), response.read ()

'''
Dictionary that holds the knowledge base.
The data source includes a QnA pair with metadata, the URL for the
QnA Maker FAQ article, and the URL for the Azure Bot Service FAQ article.
'''
req = {
  "name": "QnA Maker FAQ",
  "qnaList": [
    {
      "id": 0,
      "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/58994a073d9e04097c7ba6fe/operations/58994a073d9e041ad42d9baa",
      "source": "Custom Editorial",
      "questions": [
        "How do I programmatically update my Knowledge Base?"
      ],
      "metadata": [
        {
          "name": "category",
          "value": "api"
        }
      ]
    }
  ],
  "urls": [
    "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/en-us/bot-framework/resources-bot-framework-faq"
  ],
  "files": []
}

# Builds the path URL.
path = service + method
# Convert the request to a string.
content = json.dumps(req)
# Retrieve the operation ID to check status, and JSON result
operation, result = create_kb(path, content)
# Print request response in JSON with presentable formatting
print(pretty_print(result))

'''
Iteratively gets the operation state, creating the knowledge base.
Once state is no longer "Running" or "NotStarted", the loop ends.
'''
done = False
while False == done:
  path = service + operation
  # Gets the status of the operation.
  wait, status = check_status(path)
  # Print status checks in JSON with presentable formatting
  print(pretty_print(status))

  # Convert the JSON response into an object and get the value of the operationState field.
  state = json.loads(status)['operationState']
  # If the operation isn't finished, wait and query again.
  if state == 'Running' or state == 'NotStarted':
    print('Waiting ' + wait + ' seconds...')
    time.sleep(int(wait))
  else:
    done = True # request has been processed, if successful, knowledge base is created
```

## <a name="understand-what-qna-maker-returns"></a>Informationen zur QnA Maker-Rückgabe

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt. Ihre Ergebnisse können geringfügig abweichen. Wird mit dem letzten Aufruf der Status „Succeeded“ zurückgegeben, wurde die Wissensdatenbank erfolgreich erstellt. Informationen zur Problembehandlung finden Sie in den [Details zum GET-Vorgang](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails) der QnA Maker-API.

```json
Calling https://westus.api.cognitive.microsoft.com/qnamaker/v4.0/knowledgebases/create.
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917tb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
Calling https://westus.api.cognitive.microsoft.com/qnamaker/v4.0/operations/d9d40918-01bd-49f4-88b4-129fbc434c94.
{
  "operationState": "Running",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bt4ebha1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
Waiting 30 seconds...
Calling https://westus.api.cognitive.microsoft.com/qnamaker/v4.0/operations/d9d40918-01bd-49f4-88b4-129fbc434c94.
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:46Z",
  "resourceLocation": "/knowledgebases/b0288f33-27b9-4258-a304-8b9f63427dad",
  "userId": "2280ef5917bt4ebfa1aae40fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
Press any key to continue.
```

Nach der Erstellung der Wissensdatenbank können Sie sie im QnA Maker-Portal auf der Seite [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Meine Wissensdatenbanken) anzeigen. Wählen Sie zum Anzeigen den Namen Ihrer Wissensdatenbank aus, etwa „QnA Maker FAQ“.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)