---
title: 'JavaScript-Schnellstartanleitung: Ändern des Modells und Trainieren einer LUIS-App'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine LUIS-App mithilfe von JavaScript aufrufen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 38e25b0634b53f4fcc0507091e78ab49b29c8d38
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033395"
---
# <a name="quickstart-change-model-using-javascript"></a>Schnellstart: Ändern des Modells mit JavaScript

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Visual Studio Code](https://code.visualstudio.com/).

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]


## <a name="create-quickstart-code"></a>Erstellen des Schnellstartcodes

Erstellen Sie `add-utterances.html`, und fügen Sie den folgenden Code hinzu:

   [!code-html[Html code](~/samples-luis/documentation-samples/quickstarts/change-model/javascript/add-utterance.html "Javascript code")]

## <a name="run-code"></a>Ausführen des Codes

1. Öffnen Sie die Datei in einem Browser.

2. Fügen Sie die LUIS-Erstellungs-ID und die LUIS-Anwendungs-ID hinzu.

3. Ändern Sie das **Array der Äußerungen**, das Ihrer Anwendung hinzugefügt werden soll. Sie sind in der Variablen „utteranceJSON“ gespeichert. Passen Sie diese Werte gemäß Ihren Anforderungen an Bereich und Äußerung an. 

    ```json
    // example batch utterances
    var utteranceJSON = [
        {
            "text": "go to Seattle",
            "intentName": "BookFlight",
            "entityLabels": [
                {
                    "entityName": "Location::LocationTo",
                    "startCharIndex": 6,
                    "endCharIndex": 12
                }
            ]
        }
    ,
        {
            "text": "book a flight",
            "intentName": "BookFlight",
            "entityLabels": []
        }
    ];
    ```

4. Wählen Sie die Schaltfläche `Upload utterance` aus. Die LUIS-Ergebnisse werden unterhalb der Schaltflächen angezeigt.

5. Klicken Sie auf die Schaltfläche `Train model`, um Ihre Anwendung mit diesen neuen Äußerungen zu trainieren.

6. Klicken Sie auf die Schaltfläche `Train Status`, um den Trainingsstatus anzuzeigen. 

    ![Add-utterances.html](./media/luis-quickstart-javascript-add-utterance/add-utterance.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Entfernen Sie nach dem Durchführen dieses Schnellstarts alle Dateien, die Sie im Rahmen der Schnellstartanleitung erstellt haben. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Integrieren von LUIS mit einem Bot](luis-csharp-tutorial-build-bot-framework-sample.md)
