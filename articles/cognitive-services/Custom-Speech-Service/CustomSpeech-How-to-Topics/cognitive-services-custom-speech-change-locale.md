---
title: Unterstützte Gebietsschemas und Sprachen in Custom Speech Service in Azure | Microsoft-Dokumentation
description: Übersicht über die unterstützten Sprachen von Custom Speech Service in Cognitive Services
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 8493aeb3c1c2436900ae626ad0f34cbe8b060163
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342167"
---
# <a name="supported-locales-in-custom-speech-service"></a>Unterstützte Gebietsschemas in Custom Speech Service

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Der Custom Speech Service unterstützt derzeit die Anpassung von Modellen in der folgenden Gebietsschemas:

| Modelltyp | Sprachunterstützung |
|----|-----|
| Akustikmodelle | US Englisch (en-US) |
| Sprachmodelle | US-Englisch (en-US), Chinesisch (zh-CN) |

Obwohl die Anpassung des Akustikmodells nur in US-Englisch unterstützt wird, wird der Import von chinesischen Akustikdaten für den Offline-Test von kundenspezifischen chinesischen Sprachmodellen unterstützt.

Vor der Ausführung einer Aktion muss das entsprechende Gebietsschema ausgewählt werden. Das aktuelle Gebietsschema ist im Tabellentitel auf allen Daten-, Modell- und Bereitstellungsseiten angegeben. Klicken Sie zum Ändern des Gebietsschemas auf die Schaltfläche „Gebietsschema ändern“ unterhalb des Tabellentitels. Sie gelangen auf eine Seite zum Bestätigen des Gebietsschemas. Klicken Sie auf „OK“, um zur Tabelle zurückzukehren.

Anschließend führen Sie die nächsten Schritte aus.
* Erfahren Sie, wie Sie ein [benutzerdefiniertes Akustikmodell](cognitive-services-custom-speech-create-acoustic-model.md) zur Verbesserung der Erkennungsgenauigkeit erstellen.
* Erfahren Sie, wie Sie ein [benutzerdefiniertes Sprachmodell](cognitive-services-custom-speech-create-language-model.md) zur Verbesserung der Erkennungsrate erstellen.
* Befolgen Sie die [Transkriptionsrichtlinien](cognitive-services-custom-speech-transcription-guidelines.md) zum Vorbereiten von Daten.
