---
title: 'Python-Schnellstartanleitung: Absichtsvorhersage – LUIS'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine LUIS-App mithilfe von Python aufrufen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8fe39a2876d37e4897c03a5654e500e2e2d3cb9f
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51276949"
---
# <a name="quickstart-get-intent-using-python"></a>Schnellstartanleitung: Abrufen der Absicht mit Python
In dieser Schnellstartanleitung übergeben Sie Äußerungen an einen LUIS-Endpunkt, um Absichten und Entitäten zurückzugeben.

[!INCLUDE[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.6](https://www.python.org/downloads/) oder höher
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Abrufen der Absicht im Browser

[!INCLUDE[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent--programmatically"></a>Programmgesteuertes Abrufen der Absicht

Sie können mit Python auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden.

1. Kopieren Sie einen der folgenden Codeausschnitte in eine Datei namens `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Ersetzen Sie den Wert des Felds `Ocp-Apim-Subscription-Key` durch Ihren LUIS-Endpunktschlüssel.

3. Installieren Sie Abhängigkeiten mit `pip install requests`.

4. Führen Sie das Skript mit `python ./quickstart-call-endpoint.py` aus. Es wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die Python-Datei. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-python-add-utterance.md)