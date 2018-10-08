---
title: 'Node.js-Schnellstartanleitung: Absichtsvorhersage – LUIS'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Mithilfe von Node.js senden Sie die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 37a94b217e2b9c0d417ad0bdd7678b13e333305d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038917"
---
# <a name="quickstart-get-intent-using-nodejs"></a>Schnellstartanleitung: Abrufen der Absicht mit Node.js

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Voraussetzungen

* Programmiersprache [Node.js](https://nodejs.org/) 
* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2


> [!NOTE] 
> Die vollständige Node.js-Lösung steht im [GitHub-Repository mit **LUIS-Beispielen**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/analyze-text/node) zur Verfügung.

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Abrufen der Absicht im Browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Sie können mit Node.js auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden.

1. Kopieren Sie den folgenden Codeausschnitt:

   [!code-nodejs[Console app code that calls a LUIS endpoint for Node.js](~/samples-luis/documentation-samples/quickstarts/analyze-text/node/call-endpoint.js)]

2. Erstellen Sie die `.env`-Datei mit dem folgenden Text, oder legen Sie diese Variablen in der Umgebung fest:

    ```CMD
    LUIS_APP_ID=df67dcdb-c37d-46af-88e1-8b97951ca1c2
    LUIS_ENDPOINT_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

3. Legen Sie die Umgebungsvariable `LUIS_ENDPOINT_KEY` auf Ihren Schlüssel fest.

4. Installieren Sie Abhängigkeiten, indem Sie den folgenden Befehl an der Befehlszeile ausführen: `npm install`.

5. Führen Sie den Code mit `npm start` aus. Es werden die gleichen Werte wie zuvor im Browserfenster angezeigt.

## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Node.js-Datei.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-node-add-utterance.md)