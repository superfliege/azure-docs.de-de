---
title: 'Schnellstart: Hinzufügen von Äußerungen zu einer LUIS-App mit JavaScript – Azure Cognitive Services | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine LUIS-App mithilfe von JavaScript aufrufen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: ffc19d12c1d3fbb24c514ac87f298d1a52d23eb8
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43770079"
---
# <a name="quickstart-change-model-using-javascript"></a>Schnellstart: Ändern des Modells mit JavaScript

[!include[Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!include[Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Visual Studio Code](https://code.visualstudio.com/).

[!include[Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!include[Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]


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
