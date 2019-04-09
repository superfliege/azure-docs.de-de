---
title: Häufig gestellte Fragen – Gesichtserkennungs-API
titlesuffix: Azure Cognitive Services
description: Hier erhalten Sie Antworten auf die am häufigsten gestellten Fragen zur Gesichtserkennungs-API.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: 47ce80e1b0cefc01752d2445b751ebe1c2d65d08
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351328"
---
# <a name="face-api-frequently-asked-questions"></a>Häufig gestellte Fragen zur Gesichtserkennungs-API

> [!TIP]
> Wenn Sie in diesen FAQs keine Antwort auf Ihre Frage finden, können Sie die Fragen unter [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) und an die Community für die Gesichtserkennungs-API stellen oder wenden Sie sich an die [Hilfe und den Support bei UserVoice](https://cognitive.uservoice.com/).

-----
**Frage:** Welche Faktoren können die Genauigkeit der Gesichtserkennungs-API für die „Erkennung“, „Verifizierung“ oder „Ähnliches suchen“ beeinträchtigen?

**Antwort:** Im Allgemeinen sind es die gleichen Fälle, in denen Menschen Schwierigkeiten haben, jemanden oder etwas zu identifizieren, einschließlich:
* Hindernisse, die ein oder beide Augen verdecken
* Schwierige Beleuchtung (z. B. starkes Gegenlicht)
* Änderungen der Frisur oder der Gesichtsbehaarung
* Veränderungen aufgrund des Alters
* Extreme Gesichtsausdrücke (z. B. schreien)

Die Gesichtserkennungs-API ist in schwierigen Fällen wie dem vorherigen oft erfolgreich, aber die Genauigkeit kann beeinträchtigt werden. Um die Erkennung zu verbessern und diese Herausforderungen zu meistern, trainieren Sie Ihre Personen mit Fotos, die eine Vielzahl von Winkeln und Lichtverhältnissen beinhalten.

-----
**Frage:**  Ich habe die binären Bilddaten eingegeben, erhalte aber den Fehler „Gesichtsbild ungültig“.

**Antwort:**  Dieser Fehler bedeutet, dass der Algorithmus ein Problem mit dem Analysieren des Bilds hat. Mögliche Ursachen:
* Als Eingabebildformate werden JPEG, PNG, GIF (der erste Frame), BMP unterstützt.
* Bilddateien dürfen maximal 4 MB groß sein.
* Die erkennbare Gesichtsgröße reicht von 36 x 36 bis 4.096 x 4.096 Pixel. Außerhalb dieses Bereichs liegende Gesichter werden nicht erkannt.
* Einige Gesichter werden möglicherweise aufgrund technischer Probleme nicht erkannt: z. B. große Gesichtswinkel (Kopfpose) und starke Verdeckung. Frontalansichten und nahezu der Frontalansicht entsprechende Ansichten von Gesichtern führen zu den besten Ergebnissen.

-----
