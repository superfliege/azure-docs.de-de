---
title: 'Tutorial: Informationen zum Hinzufügen von Äußerungen zu einer LUIS-App mithilfe von JavaScript | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine LUIS-App mithilfe von JavaScript aufrufen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: v-geberr
ms.openlocfilehash: b6d021dcfdddb5449aa989c6aa06d7faf326befb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265458"
---
# <a name="tutorial-add-utterances-to-app-using-javascript"></a>Tutorial: Hinzufügen von Äußerungen zu einer App mithilfe von JavaScript
In diesem Tutorial schreiben Sie mithilfe der Erstellungs-APIs in JavaScript ein Programm, um einer Absicht eine Äußerung hinzuzufügen.

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines Visual Studio-Konsolenprojekts 
> * Hinzufügen einer Methode zum Aufrufen der LUIS-API, um eine Äußerung hinzuzufügen und die App zu trainieren
> * Hinzufügen einer JSON-Datei mit Beispieläußerungen für die Absicht „BookFlight“ (Flug buchen)
> * Ausführen der Konsole und Anzeigen des Trainingsstatus für Äußerungen

Weitere Informationen finden Sie in der technischen Dokumentation für die APIs zum [Hinzufügen einer Beispieläußerung zu einer Absicht](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [Trainieren](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) und Abrufen des [Trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="prerequisites"></a>Voraussetzungen
* LUIS-[**Erstellungsschlüssel**](luis-concept-keys.md#authoring-key). 
* Ihre vorhandene LUIS-**Anwendungs-ID** und **Versions-ID**. 
* Eine neue Projektdatei mit dem Namen `add-utterances.html` in VSCode

> [!NOTE] 
> Die vollständige `add-utterances.html`-Datei steht im [Github-Repository mit **LUIS-Beispielen**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/javascript/add-utterance.html) zur Verfügung.


## <a name="write-the-code"></a>Schreiben des Codes
Erstellen Sie `add-utterances.html`, und fügen Sie den folgenden Code hinzu:

   [!code-javascript[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/javascript/add-utterance.html "Java Dependencies")]

## <a name="view-in-browser"></a>Anzeigen im Browser
1. Öffnen Sie die Datei in einem Browser.

2. Fügen Sie die LUIS-Erstellungs-ID und die LUIS-Anwendungs-ID hinzu, und ändern Sie die Version, falls sie nicht `0.1` lautet.

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
Wenn Sie das Tutorial abgeschlossen haben, entfernen Sie Visual Studio und die Konsolenanwendung, wenn Sie sie nicht mehr benötigen. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Integrieren von LUIS mit einem Bot](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website