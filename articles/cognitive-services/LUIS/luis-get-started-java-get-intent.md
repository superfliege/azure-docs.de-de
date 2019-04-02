---
title: Abrufen der Absicht, Java
titleSuffix: Language Understanding - Azure Cognitive Services
description: In dieser Java-Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers.
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 5e5a186be19c0ecc9730bc79bddff41fb6bbb571
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401658"
---
# <a name="quickstart-get-intent-using-java"></a>Schnellstart: Abrufen der Absicht mit Java

In dieser Schnellstartanleitung übergeben Sie Äußerungen an einen LUIS-Endpunkt, um Absichten und Entitäten zurückzugeben.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Voraussetzungen

* [JDK SE](https://aka.ms/azure-jdks) (Java Development Kit, Standard Edition)
* [Visual Studio Code](https://code.visualstudio.com/) oder Ihre bevorzugte IDE
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Abrufen der Absicht im Browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Sie können mit Java auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden. Fügen Sie Ihrem Projekt unbedingt die Apache-Bibliotheken hinzu.

1. Kopieren Sie den folgenden Code, um eine Klasse in einer Datei namens `LuisGetRequest.java` zu erstellen:

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. Ersetzen Sie den Wert der `YOUR-KEY`-Variable durch Ihren LUIS-Schlüssel.

3. Setzen Sie Ihren Dateipfad ein, und kompilieren Sie das Java-Programm über eine Befehlszeile: `javac -cp .;<FILE_PATH>\* LuisGetRequest.java`.

4. Setzen Sie Ihren Dateipfad ein, und führen Sie die Anwendung über eine Befehlszeile aus: `java -cp .;<FILE_PATH>\* LuisGetRequest.java`. Es wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

    ![Konsolenfenster mit JSON-Ergebnis aus LUIS](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Java-Datei bzw. den Java-Projektordner.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-java-add-utterance.md)
