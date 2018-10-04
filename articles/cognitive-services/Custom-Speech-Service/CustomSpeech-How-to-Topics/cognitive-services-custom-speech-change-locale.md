---
title: Unterstützte Gebietsschemata und Sprachen – Custom Speech Service
titlesuffix: Azure Cognitive Services
description: Übersicht über die unterstützten Sprachen von Custom Speech Service in Cognitive Services
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 34af6673689244364ab3a1fe3f2a6ab056ea8598
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223954"
---
# <a name="supported-locales-in-custom-speech-service"></a>Unterstützte Gebietsschemas in Custom Speech Service
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
