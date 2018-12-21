---
title: Veröffentlichen einer Wissensdatenbank, REST, Node.js
titleSuffix: QnA Maker - Azure Cognitive Services
description: In dieser Node.js-Schnellstartanleitung wird das programmgesteuerte Veröffentlichen Ihrer Wissensdatenbank (Knowledge Base, KB) Schritt für Schritt beschrieben. Bei der Veröffentlichung wird die aktuelle Version der Wissensdatenbank per Pushvorgang an einen dedizierten Azure Search-Index übertragen und ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: 5970cb0b4f5e38862d41fc0db84847b1bb92c173
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53164176"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-nodejs"></a>Schnellstart: Veröffentlichen einer Wissensdatenbank in QnA Maker mit Node.js

In diesem REST-basierten Schnellstart wird das programmgesteuerte Veröffentlichen Ihrer Wissensdatenbank (Knowledge Base, KB) Schritt für Schritt beschrieben. Bei der Veröffentlichung wird die aktuelle Version der Wissensdatenbank per Pushvorgang an einen dedizierten Azure Search-Index übertragen und ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Veröffentlichen](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): Für diese API sind keine Informationen im Text der Anforderung erforderlich.

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js 6+](https://nodejs.org/en/download/)
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md). Wählen Sie zum Abrufen des Schlüssels auf Ihrem Dashboard unter **Ressourcenverwaltung** die Option **Schlüssel** aus. 
* ID der QnA Maker-Wissensdatenbank, die in der URL des Abfragezeichenfolgenparameters „kbid“ enthalten ist, wie nachfolgend gezeigt:

    ![ID der QnA Maker-Wissensdatenbank](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Falls Sie noch keine Wissensdatenbank besitzen, können Sie für diese Schnellstartanleitung eine Beispieldatenbank erstellen: [Erstellen Sie eine neue Wissensdatenbank](create-new-kb-nodejs.md).


> [!NOTE] 
> Die vollständigen Projektmappendateien sind über das [GitHub-Repository **Azure-Samples/cognitive-services-qnamaker-nodejs**](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/publish-knowledge-base-short) verfügbar.

## <a name="create-a-knowledge-base-nodejs-file"></a>Erstellen einer Node.js-Datei für die Wissensdatenbank

Erstellen Sie eine Datei mit dem Namen `publish-knowledge-base.js`.

## <a name="add-required-dependencies"></a>Hinzufügen von erforderlichen Abhängigkeiten

Fügen Sie am Anfang von `publish-knowledge-base.js` die folgenden Zeilen ein, um dem Projekt die erforderlichen Abhängigkeiten hinzuzufügen:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=1-3 "Add the dependencies")]

## <a name="add-required-constants"></a>Hinzufügen der erforderlichen Konstanten

Fügen Sie nach den obigen erforderlichen Abhängigkeiten die erforderlichen Konstanten für den Zugriff auf QnA Maker hinzu. Ersetzen Sie die Werte dabei durch Ihre eigenen Werte.

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=11-14 "Add required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Hinzufügen einer POST-Anforderung zur Veröffentlichung der Wissensdatenbank

Fügen Sie nach den erforderlichen Konstanten den folgenden Code hinzu, der eine HTTPS-Anforderung an die QnA Maker-API sendet, um eine Wissensdatenbank zu veröffentlichen, und die Antwort empfängt:

[!code-nodejs[Add a POST request to publish knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=16-47 "Add a POST request to publish knowledge base")]

Der API-Aufruf gibt den Status 204 für eine erfolgreiche Veröffentlichung ohne Inhalt im Text der Antwort zurück. Mit dem Code wird Inhalt für Antworten vom Typ 204 hinzugefügt.

Für alle anderen Antworten wird diese Antwort unverändert zurückgegeben.

## <a name="run-the-program"></a>Ausführen des Programms

Erstellen Sie das Programm, und führen Sie es aus. Die Anforderung wird automatisch an die QnA Maker-API gesendet, um die Wissensdatenbank zu veröffentlichen. Anschließend wird die Antwort im Konsolenfenster angezeigt.

Nachdem Ihre Wissensdatenbank veröffentlicht wurde, können Sie sie vom Endpunkt mit einer Clientanwendung oder einem Chatbot abfragen. 

```bash
node publish-knowledge-base.js
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Wissensdatenbank veröffentlicht wurde, benötigen Sie die [Endpunkt-URL, um eine Antwort zu generieren](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
