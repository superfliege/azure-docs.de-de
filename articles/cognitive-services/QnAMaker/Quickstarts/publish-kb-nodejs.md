---
title: 'Schnellstart: Veröffentlichen einer Wissensdatenbank: REST, Node.js – QnA Maker'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung wird das programmgesteuerte Veröffentlichen Ihrer Wissensdatenbank (Knowledge Base, KB) Schritt für Schritt beschrieben. Bei der Veröffentlichung wird die aktuelle Version der Wissensdatenbank per Push an einen dedizierten Azure Search-Index übertragen und ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: diberry
ms.openlocfilehash: c70b90a6e465c72193f63afd7ab9106579e2c634
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886609"
---
# <a name="quickstart-publish-a-qna-maker-knowledge-base-in-nodejs"></a>Schnellstart: Veröffentlichen einer QnA Maker-Wissensdatenbank in Node.js

In dieser Schnellstartanleitung wird das programmgesteuerte Veröffentlichen Ihrer Wissensdatenbank (Knowledge Base, KB) Schritt für Schritt beschrieben. Bei der Veröffentlichung wird die aktuelle Version der Wissensdatenbank per Push an einen dedizierten Azure Search-Index übertragen und ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Veröffentlichen](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): Diese API erfordert keine Informationen im Anforderungstext.

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js 6+](https://nodejs.org/en/download/)
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md). Wählen Sie zum Abrufen des Schlüssels auf Ihrem Dashboard unter **Ressourcenverwaltung** die Option **Schlüssel** aus. 
* ID der QnA Maker-Wissensdatenbank, die in der URL des Abfragezeichenfolgenparameters „kbid“ enthalten ist, wie nachfolgend gezeigt:

    ![ID der QnA Maker-Wissensdatenbank](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Falls Sie noch keine Wissensdatenbank besitzen, können Sie für diese Schnellstartanleitung eine Beispieldatenbank erstellen: [Create a new knowledge base in Python](create-new-kb-nodejs.md) (Erstellen einer neuen Wissensdatenbank in Python)

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Erstellen einer Node.js-Datei für die Wissensdatenbank

Erstellen Sie eine Datei mit dem Namen `publish-knowledge-base.js`.

## <a name="add-required-dependencies"></a>Hinzufügen der erforderlichen Abhängigkeiten

Fügen Sie am Anfang von `publish-knowledge-base.js` die folgenden Zeilen ein, um dem Projekt die erforderlichen Abhängigkeiten hinzuzufügen:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Hinzufügen der erforderlichen Konstanten

Fügen Sie im Anschluss an die erforderlichen Abhängigkeiten die benötigten Konstanten für den Zugriff auf QnA Maker hinzu. Ersetzen Sie den Wert der Variablen `subscriptionKey` durch Ihren QnA Maker-Schlüssel. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Hinzufügen der Wissensdatenbank-ID

Fügen Sie im Anschluss an die oben genannten Konstanten die Wissensdatenbank-ID ein, und fügen Sie sie dem Pfad hinzu:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-supporting-functions"></a>Hinzufügen von unterstützenden Funktionen

Fügen Sie als Nächstes die folgenden unterstützenden Funktionen hinzu.

1. Fügen Sie die folgende Funktion zum Ausgeben von JSON in einem lesbaren Format hinzu:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=25-28 "Add supporting functions, step 1")]

2. Fügen Sie zum Verwalten der HTTP-Antwort und zum Abrufen des Status des Erstellvorgangs die folgenden Funktionen hinzu:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=30-52 "Add supporting functions, step 2")]

## <a name="add-the-publishkb-function-and-main-function"></a>Hinzufügen der publish_kb- und der main-Funktion

Der folgende Code sendet eine HTTPS-Anforderung an die QnA Maker-API, um eine KB zu veröffentlichen, und empfängt die Antwort:

[!code-nodejs[Add POST request to publish KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=54-71 "Add POST request to publish KB")]

[!code-nodejs[Add the publish_kb function](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=73-91 "Add the publish_kb function and main function")]

## <a name="add-the-main-function"></a>Hinzufügen der main-Funktion

Fügen Sie zum Verwalten der Anforderung und der Antwort die folgende Funktion hinzu:

[!code-nodejs[Add the main function](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=94-97 "Add the main function")]

## <a name="run-the-program"></a>Ausführen des Programms

Erstellen Sie das Programm, und führen Sie es aus. Die Anforderung wird automatisch an die QnA Maker-API gesendet, um die KB zu veröffentlichen. Anschließend wird die Antwort im Konsolenfenster angezeigt.

Sobald Ihre Wissensdatenbank veröffentlicht wurde, können Sie sie mit einer Clientanwendung oder einem Chatbot über den Endpunkt abfragen. 

```bash
node publish-knowledge-base.js
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)