---
title: 'C#-Schnellstartanleitung: Absichtsvorhersage – LUIS'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Mithilfe von C# senden Sie die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 51c23029cc771db5351575ce329944a9f06dd286
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035843"
---
# <a name="quickstart-get-intent-using-c"></a>Schnellstartanleitung: Abrufen der Absicht mit C#

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio Community 2017 Edition](https://visualstudio.microsoft.com/vs/community/)
* Programmiersprache C# (in VS Community 2017 enthalten)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Abrufen der Absicht im Browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Verwenden Sie C# zum Abfragen der GET-[API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) des Vorhersageendpunkts, um die gleichen Ergebnisse wie im vorherigen Abschnitt im Browserfenster zu erhalten. 

1. Erstellen Sie eine neue Konsolenanwendung in Visual Studio. 

    ![Zugriff auf das Menü mit den LUIS-Benutzereinstellungen](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. Klicken Sie im Visual Studio-Projekt im Projektmappen-Explorer auf **Verweis hinzufügen**, und wählen Sie dann auf der Registerkarte „Assemblys“ den Eintrag **System.Web** aus.

    ![Zugriff auf das Menü mit den LUIS-Benutzereinstellungen](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Überschreiben Sie „Program.cs“ mit dem folgenden Code:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Ersetzen Sie den Wert von `YOUR_KEY` durch Ihren LUIS-Schlüssel.

5. Erstellen Sie die Konsolenanwendung, und führen Sie sie aus. Es wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

    ![Konsolenfenster mit JSON-Ergebnis aus LUIS](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Schließen Sie nach Beendigung dieser Schnellstartanleitung das Visual Studio-Projekt, und entfernen Sie das Projektverzeichnis aus dem Dateisystem. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen und Trainieren mit C#](luis-get-started-cs-add-utterance.md)