---
title: Auswählen des Erkennungsmodus | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie den besten Erkennungsmodus wählen.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 4f02b683dde16b537ae5554e6435c068f0fcb808
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373794"
---
# <a name="speech-recognition-modes"></a>Spracherkennungsmodi

Die Microsoft-APIs für die *Spracherkennung* unterstützen mehrere Spracherkennungsmodi. Wählen Sie den Modus, in dem die besten Erkennungsergebnisse für Ihre Anwendung erzeugt werden.

| Modus | Beschreibung |
|---|---|
| *Interaktiv* | Erkennung vom Typ „Befehl und Steuerung“ für interaktive Benutzeranwendungsszenarien. Benutzer äußern kurze Ausdrücke als Befehle für eine Anwendung. |
| *Diktieren* | Fortlaufende Erkennung für Diktierszenarien. Benutzer äußern längere Sätze, die als Text angezeigt werden. Hierbei verwenden die Benutzer eine formellere Sprache. |
| *Unterhaltung* | Fortlaufende Erkennung zum Transkribieren von Unterhaltungen zwischen Menschen. Benutzer wählen eine weniger formelle Sprache und können zwischen längeren Sätzen und kürzeren Ausdrücken wechseln.

> [!NOTE]
> Diese Modi gelten bei Verwendung der [REST-APIs](../GetStarted/GetStartedREST.md). Die [Clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) verwenden verschiedene Parameter, um den Erkennungsmodus anzugeben. Weitere Informationen finden Sie in der Clientbibliothek Ihrer Wahl.

Weitere Informationen finden Sie auf der Seite [Erkennungsmodi](../concepts.md#recognition-modes).
