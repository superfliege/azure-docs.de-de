---
title: Analysieren von Text in natürlicher Sprache in Language Understanding (LUIS) mit Go – Azure Cognitive Services | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Mithilfe von Go senden Sie die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: ca1cba4416f0855a5838e8f3024be87351978ac8
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "43769999"
---
# <a name="quickstart-call-a-luis-endpoint-using-go"></a>Schnellstart: Aufrufen eines LUIS-Endpunkts mit Go

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Programmiersprache [Go](https://golang.org/)  
* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Analysieren von Text mit dem Browser

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-go"></a>Analysieren von Text mit Go

Sie können mit Go auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden. 

1. Erstellen Sie eine neue Datei mit dem Namen `endpoint.go`. Fügen Sie den folgenden Code hinzu:
    
   [!code-go[Go code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/go/endpoint.go?range=36-98)]

2. Geben Sie in dem Verzeichnis, in dem Sie die Datei erstellt haben, an einer Eingabeaufforderung `go build endpoint.go` ein, um die Go-Datei zu kompilieren. Die Eingabeaufforderung gibt keine Informationen für eine erfolgreiche Erstellung zurück.

3. Führen Sie die Go-Anwendung über die Befehlszeile aus, indem Sie den folgenden Text an der Eingabeaufforderung eingeben: 

    ```CMD
    go run endpoint.go -appID df67dcdb-c37d-46af-88e1-8b97951ca1c2 -endpointKey <add-your-key> -region westus
    ```
    
    Ersetzen Sie `<add-your-key>` durch den Wert Ihres Schlüssels.  
    
    Die Antwort der Eingabeaufforderung sieht wie folgt aus: 
    
    ```CMD
    appID has value df67dcdb-c37d-46af-88e1-8b97951ca1c2
    endpointKey has value xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    region has value westus
    utterance has value turn on the bedroom light
    response
    {
        "query": "turn on the bedroom light",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.809439957
        },
        "entities": [
            {
            "entity": "bedroom",
            "type": "HomeAutomation.Room",
            "startIndex": 12,
            "endIndex": 18,
            "score": 0.8065475
            }
        ]
    }
    ```
    
## <a name="luis-keys"></a>LUIS-Schlüssel

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Schließen Sie die Go-Datei, und entfernen Sie sie aus dem Dateisystem. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-go-add-utterance.md)