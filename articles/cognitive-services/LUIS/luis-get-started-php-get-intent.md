---
title: 'Schnellstart: Analysieren von Text in natürlicher Sprache in Language Understanding (LUIS) mit PHP – Cognitive Services – Azure Cognitive Services | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung bestimmen Sie mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers. Mithilfe von PHP senden Sie die Absicht des Benutzers als Text an den HTTP-Vorhersageendpunkt der öffentlichen App. Auf dem Endpunkt wendet LUIS das Modell der öffentlichen App an, um den Text in natürlicher Sprache im Hinblick auf seine Bedeutung zu analysieren. Dabei werden die allgemeine Absicht bestimmt und relevante Daten für die Antragstellerdomäne der App extrahiert.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 14ac32ffc0f4332d0cbc0da59a55ccc500e216d7
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "43769967"
---
# <a name="quickstart-analyze-text-using-php"></a>Schnellstart: Analysieren von Text mit PHP

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Voraussetzungen

* Programmiersprache [PHP](http://php.net/)
* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Analysieren von Text mit dem Browser

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-php"></a>Analysieren von Text mit PHP 

Sie können mit PHP auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden. 

1. Kopieren Sie den folgenden Code, und speichern Sie ihn in einer Datei namens `endpoint-call.php`:

   [!code-php[PHP code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/php/endpoint-call.php)]

2. Ersetzen Sie `"YOUR-KEY"` durch Ihren Endpunktschlüssel.

3. Führen Sie die PHP-Anwendung mit `php endpoint-call.php` aus. Es wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

## <a name="luis-keys"></a>LUIS-Schlüssel

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die PHP-Datei.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-php-add-utterance.md)