---
title: Analysieren von Text in natürlicher Sprache in Language Understanding (LUIS) mit C# – Azure Cognitive Services | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Mithilfe von C# senden Sie die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: b6ddd48fc6bfa5c099e42f3717a2113f871b4f9a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163259"
---
# <a name="quickstart-analyze-text-using-c"></a>Schnellstart: Analysieren von Text mit C#

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio Community 2017 Edition](https://visualstudio.microsoft.com/vs/community/)
* Programmiersprache C# (in VS Community 2017 enthalten)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Analysieren von Text mit dem Browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-c"></a>Analysieren von Text mit C# 

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