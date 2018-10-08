---
title: 'JavaScript-Schnellstartanleitung: Absichtsvorhersage – LUIS'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Mithilfe von JavaScript senden Sie die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: cbbf0d2388f83f4e38f0a63b6bc181d4a24e0aa0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039479"
---
# <a name="quickstart-get-intent-using-javascript"></a>Schnellstartanleitung: Abrufen der Absicht mithilfe von Javascript

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2


## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Abrufen der Absicht im Browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Sie können mit JavaScript auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden. 

1. Kopieren Sie den folgenden Code, und speichern Sie ihn in einer HTML-Datei:

   [!code-html[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/javascript/call-endpoint.html)]

2. Öffnen Sie die Datei in einem Browser. Geben Sie Ihren LUIS-Endpunktschlüssel in das Formular ein, und klicken Sie auf **Senden**.

    ![Im Browser angezeigtes HTML-Beispiel mit LUIS-Ergebnissen für die Home Automation-App](./media/luis-get-started-js-get-intent/html-results.png)

    Das Ergebnis wird unterhalb des Formulars angezeigt. 

## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die JavaScript-Datei.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-javascript-add-utterance.md)
