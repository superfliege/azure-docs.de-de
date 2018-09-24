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
ROBOTS: NOINDEX
ms.openlocfilehash: 1f186681c7e46d2e47ed7eee55c8f61290c48fcb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987525"
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
