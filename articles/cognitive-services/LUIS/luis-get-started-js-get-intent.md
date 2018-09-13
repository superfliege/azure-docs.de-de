---
title: Analysieren von Text in natürlicher Sprache in Language Understanding (LUIS) mit JavaScript – Cognitive Services – Azure Cognitive Services | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Mithilfe von JavaScript senden Sie die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 10210c3759611a77c4430a97896a19a6b97b9fa9
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "43769980"
---
# <a name="quickstart-analyze-text-using-javascript"></a>Schnellstart: Analysieren von Text mit JavaScript

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2


## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Analysieren von Text mit dem Browser

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-javascript"></a>Analysieren von Text mit dem JavaScript 

Sie können mit JavaScript auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden. 

1. Kopieren Sie den folgenden Code, und speichern Sie ihn in einer HTML-Datei:

   [!code-html[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/javascript/call-endpoint.html)]

2. Öffnen Sie die Datei in einem Browser. Geben Sie Ihren LUIS-Endpunktschlüssel in das Formular ein, und klicken Sie auf **Senden**.

    ![Im Browser angezeigtes HTML-Beispiel mit LUIS-Ergebnissen für die Home Automation-App](./media/luis-get-started-js-get-intent/html-results.png)

    Das Ergebnis wird unterhalb des Formulars angezeigt. 

## <a name="luis-keys"></a>LUIS-Schlüssel

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die JavaScript-Datei.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-javascript-add-utterance.md)
