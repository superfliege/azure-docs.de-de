---
title: Auswählen des Modus für die Bing-Spracheingabe | Microsoft-Dokumentation
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie den besten Erkennungsmodus in der Bing-Spracheingabe auswählen.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX
ms.openlocfilehash: a39b357a26823e322d4e902f2d99b67488bbf2df
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950878"
---
# <a name="bing-speech-recognition-modes"></a>Erkennungsmodi für die Bing-Spracheingabe

Die Bing-Spracherkennungs-APIs unterstützen mehrere Spracherkennungsmodi. Wählen Sie den Modus, in dem die besten Erkennungsergebnisse für Ihre Anwendung erzeugt werden.

| Mode | BESCHREIBUNG |
|---|---|
| *Interaktiv* | Erkennung vom Typ „Befehl und Steuerung“ für interaktive Benutzeranwendungsszenarien. Benutzer äußern kurze Ausdrücke als Befehle für eine Anwendung. |
| *Diktieren* | Fortlaufende Erkennung für Diktierszenarien. Benutzer äußern längere Sätze, die als Text angezeigt werden. Hierbei verwenden die Benutzer eine formellere Sprache. |
| *Unterhaltung* | Fortlaufende Erkennung zum Transkribieren von Unterhaltungen zwischen Menschen. Benutzer wählen eine weniger formelle Sprache und können zwischen längeren Sätzen und kürzeren Ausdrücken wechseln.

> [!NOTE]
> Diese Modi gelten bei Verwendung der [REST-APIs](../GetStarted/GetStartedREST.md). Die [Clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) verwenden verschiedene Parameter, um den Erkennungsmodus anzugeben. Weitere Informationen finden Sie in der Clientbibliothek Ihrer Wahl.

Weitere Informationen finden Sie auf der Seite [Erkennungsmodi](../concepts.md#recognition-modes).
