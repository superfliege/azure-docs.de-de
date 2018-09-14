---
title: 'Schnellstart: Ändern des Modells und Trainieren der LUIS-App mit Go – Azure Cognitive Services | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung für Go fügen Sie einer Home Automation-App Beispieläußerungen hinzu, und Sie trainieren die App. Bei Beispieläußerungen handelt es sich um Konversationstext von Benutzern, der einer Absicht zugeordnet ist. Indem Sie Beispieläußerungen für Absichten bereitstellen, teilen Sie LUIS mit, welche Arten des vom Benutzer angegebenen Texts zu welcher Absicht gehören.
titleSuffix: Microsoft Cognitive Services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: da57a7e46cccbf0a9b34b3961a831e7982160e6b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157666"
---
# <a name="quickstart-change-model-using-go"></a>Schnellstart: Ändern des Modells mit Go

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Programmiersprache [Go](https://golang.org/) installiert
* [VSCode](https://code.visualstudio.com) 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Erstellen des Schnellstartcodes 

1. Erstellen Sie `add-utterances.go` mit VSCode. 

2. Fügen Sie Abhängigkeiten hinzu. 

   [!code-go[Add dependencies.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=2-10 "Add dependencies.")]

3. Fügen Sie die generische HTTP-Anforderungsfunktion hinzu, die das Übergeben des Erstellungsschlüssels im Header beinhaltet. 

   [!code-go[Add HTTP request function which includes passing authoring key in header. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=12-36 "Add HTTP request function, which includes passing authoring key in header. ")]

4. Fügen Sie Beispieläußerungen aus der JSON-Datei hinzu.

   [!code-go[Add example utterances from JSON file.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=62-76 "Add example utterances from JSON file.")]

5. Fordern Sie das Training an. Hierbei wird eine Hilfsfunktion verwendet, um das VERB für die gleiche Route als Trainingsstatus festzulegen. 

   [!code-go[Request training. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=77-86 "Request training. ")]

6. Fordern Sie den Trainingsstatus an. Hierbei wird eine Hilfsfunktion verwendet, um das VERB für die gleiche Route als Trainingsanforderung festzulegen. 

   [!code-go[Request training status. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=87-90 "Request training status. ")]

7. Fügen Sie die main-Funktion zum Behandeln der Befehlszeilenanalyse hinzu.

   [!code-go[Add main function to handle command line parsing. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=38-60 "Add main function to handle command-line parsing.")]

## <a name="add-an-utterance-from-the-command-line-train-and-get-status"></a>Hinzufügen einer Äußerung über die Befehlszeile, Trainieren und Abrufen des Status

1. Geben Sie in dem Verzeichnis, in dem Sie die Go-Datei erstellt haben, an einer Eingabeaufforderung `go build add-utterances.go` ein, um die Go-Datei zu kompilieren. Die Eingabeaufforderung gibt keine Informationen für eine erfolgreiche Erstellung zurück.

2. Führen Sie die Go-Anwendung über die Befehlszeile aus, indem Sie den folgenden Text an der Eingabeaufforderung eingeben: 

    ```CMD
    add-utterances -appID <your-app-id> -authoringKey <add-your-authoring-key> -version <your-version-id> -region westus -utteranceFile utterances.json

    ```

    Ersetzen Sie `<add-your-authoring-key>` durch den Wert Ihres Erstellungsschlüssels (auch bezeichnet als Startschlüssel). Ersetzen Sie `<your-app-id>` durch den Wert Ihrer App-ID. Ersetzen Sie `<your-version-id>` durch den Wert Ihrer Version. Die Standardversion ist `0.1`.

    Mit dieser Eingabeaufforderung werden die Ergebnisse angezeigt:

    ```CMD
    add example utterances requested
    [
        {
            "text": "go lang 1",
            "intentName": "None",
            "entityLabels": []
        }
    ,
        {
            "text": "go lang 2",
            "intentName": "None",
            "entityLabels": []
        }
    ]
    201
    [
        {
            "value": {
                "ExampleId": 77783998,
                "UtteranceText": "go lang 1"
            },
            "hasError": false
        },
        {
            "value": {
                "ExampleId": 77783999,
                "UtteranceText": "go lang 2"
            },
            "hasError": false
        }
    ]
    training selected
    202
    {"statusId":9,"status":"Queued"}
    training status selected
    200
    [
        {
            "modelId": "c52d6509-9261-459e-90bc-b3c872ee4a4b",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "5119cbe8-97a1-4c1f-85e6-6449f3a38d77",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "01e6b6bc-9872-47f9-8a52-da510cddfafe",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "33b409b2-32b0-4b0c-9e91-31c6cfaf93fb",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "1fb210be-2a19-496d-bb72-e0c2dd35cbc1",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "3d098beb-a1aa-423f-a0ae-ce08ced216d6",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "cce854f8-8f8f-4ed9-a7df-44dfea562f62",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "4d97bf0d-5213-4502-9712-2d6e77c96045",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        }
    ]
    ```

    Diese Antwort enthält den HTTP-Statuscode für jeden der drei HTTP-Aufrufe und alle im Text der Antwort zurückgegebenen JSON-Antworten. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Entfernen Sie nach dem Durchführen dieses Schnellstarts alle Dateien, die Sie im Rahmen der Schnellstartanleitung erstellt haben. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Erstellen einer App mit einer benutzerdefinierten Domäne](luis-quickstart-intents-only.md) 