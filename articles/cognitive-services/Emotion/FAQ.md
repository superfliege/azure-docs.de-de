---
title: Häufig gestellte Fragen zur Emotionen-API | Microsoft-Dokumentation
description: Erhalten Sie Antworten auf häufig gestellte Fragen zur Emotionen-API in Microsoft Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 8532d7c00fd8d7b01d84b5e55cb9bbc60241789c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372995"
---
# <a name="emotion-api-frequently-asked-questions"></a>Häufig gestellte Fragen zur Emotionen-API
 
> [!IMPORTANT]
> Die Video-API-Vorschau ist am 30. Oktober 2017 abgelaufen. Testen Sie die neue [Video-Indexer-API (Vorschau)](https://azure.microsoft.com/services/cognitive-services/video-indexer/). Extrahieren Sie Erkenntnisse aus Videos, und verbessern Sie die Auffindbarkeit von Inhalten, z.B. mit Suchergebnissen durch die Erkennung von gesprochenen Wörtern, Gesichtern, Zeichen und Emotionen. [Weitere Informationen](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview)

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Wenn Sie in diesen FAQs keine Antwort auf Ihre Frage finden, können Sie die Fragen unter [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) und an die Community für die Emotionen-API stellen oder wenden Sie sich an die [Hilfe und den Support bei UserVoice](https://cognitive.uservoice.com/).  

-----

**Frage**: *Mit welchen Arten von Bildern werden mit der Emotionen-API die besten Ergebnisse erzielt?*

**Antwort**: Verwenden Sie für beste Ergebnisse unversperrte, vollständige frontale Gesichtsaufnahmen. Die Zuverlässigkeit nimmt bei partiellen Gesichtsflächen ab und die Emotionen-API erkennt möglicherweise keine Emotionen in Bildern, in denen das Gesicht um 45 Grad oder mehr gedreht ist.

-----

**Frage**: *Wie viele Emotionen kann die Emotionen-API erkennen?*

**Antwort**: Die Emotionen-API erkennt acht verschiedene allgemein akzeptierte Emotionen: 
* Freude
* Trauer
* Überraschung
* Wut
* Angst
* Verachtung
* Ekel 
* Neutral 

-----

**Frage**: *Gibt es eine Möglichkeit, einen Live-Videostream an die API weiterzuleiten und gleichzeitig das Ergebnis zu erhalten?*

**Antwort**: Verwenden Sie die bildbasierte Emotionen-API und rufen Sie sie bei jedem Frame auf oder überspringen Sie Frames für mehr Leistung.  Es sind Frame-für Frame-Analysebeispiele für Videos verfügbar.

-----

**Frage**: *Ich habe die binären Bilddaten eingegeben, erhalte aber den Fehler „Gesichtsbild ungültig“.**

**Antwort**: Dies bedeutet, dass der Algorithmus ein Problem mit dem Analysieren des Bilds hat.  
* Als Eingabebildformate werden JPEG, PNG, GIF (der erste Frame), BMP unterstützt. 
* Bilddateien dürfen maximal 4 MB groß sein.
* Die erkennbare Gesichtsgröße reicht von 36 x 36 bis 4.096 x 4.096 Pixel. Außerhalb dieses Bereichs liegende Gesichter werden nicht erkannt.
* Einige Gesichter werden möglicherweise aufgrund technischer Probleme nicht erkannt: z.B. sehr große Gesichtswinkel (Kopfpose) und starke Verdeckung. Frontalansichten und nahezu der Frontalansicht entsprechende Ansichten von Gesichtern führen zu den besten Ergebnissen.

-----
