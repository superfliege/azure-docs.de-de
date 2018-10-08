---
title: Konzepte zur Datenkonvertierung in LUIS – Language Understanding
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Äußerungen vor der Vorhersage in Language Understanding (LUIS) geändert werden können.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 3862a0dbd94b5764cf506b05201d8dc60430fc7d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038968"
---
# <a name="data-conversion-concepts-in-luis"></a>Konzepte zur Datenkonvertierung in LUIS
LUIS verwendet den Cognitive Services-Sprachdienst zum Konvertieren von Äußerungen aus gesprochenen Äußerungen in Textäußerungen vor der Vorhersage. 

## <a name="speech-to-intent-conversion-concepts"></a>Konzepte zur Sprache-Absichts-Umsetzung
Die Sprache-Absichts-Umsetzung in LUIS ermöglicht es Ihnen, gesprochene Äußerungen an einen Endpunkt zu senden und als Antwort eine LUIS-Vorhersage zu erhalten. Dieser Vorgang stellt eine Integration des [Sprachverständnis](https://docs.microsoft.com/azure/cognitive-services/Speech)-Diensts in LUIS dar. 

### <a name="key-requirements"></a>Schlüsselanfoderungen
Sie brauchen für diese Integration keinen Schlüssel für eine **Bing-Sprach-API** zu erstellen. Ein im Azure-Portal erstellter **Language Understanding**-Schlüssel funktioniert bei dieser Integration. Verwenden Sie nicht den LUIS-Startschlüssel, er funktioniert bei dieser Integration nicht.

### <a name="new-endpoint"></a>Neuer Endpunkt 
Diese Integration erstellt einen neuen Endpunkt und ein neues [Preismodell](luis-boundaries.md#key-limits). Der Endpunkt kann mithilfe des [Spracherkennungs-SDKs](https://github.com/Azure-Samples/cognitive-services-speech-sdk) Äußerungen sowohl in gesprochener als auch in Textform empfangen und daher als einzelner Endpunkt verwendet werden. 

### <a name="quota-usage"></a>Kontingentverbrauch
Informationen finden Sie unter [Schlüsselgrenzwerte](luis-boundaries.md#key-limits). 

### <a name="data-retention"></a>Beibehaltung von Daten
Die mithilfe des Spracherkennungs-SDKs an den Endpunkt gesendeten Daten werden nur zur Verbesserung Ihres Sprachmodells verwendet, ganz gleich, ob sie als Sprache oder Text vorliegen. Sie werden nicht außerhalb Ihres Modells verwendet, um die allgemeinen Fähigkeiten der Spracherkennung oder von LUIS zu verbessern. Wenn die LUIS-App gelöscht wird, werden die gespeicherten Daten ebenfalls gelöscht.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden der Spracherkennung](luis-tutorial-speech-to-intent.md)

