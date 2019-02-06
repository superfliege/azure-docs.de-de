---
title: Häufig gestellte Fragen zur Emotionen-API
titlesuffix: Azure Cognitive Services
description: Erhalten Sie Antworten auf häufig gestellte Fragen zur Emotionen-API.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: ded91c6de498b130cc26109a70e89955dd70d862
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208985"
---
# <a name="emotion-api-frequently-asked-questions"></a>Häufig gestellte Fragen zur Emotionen-API

> [!IMPORTANT]
> Die Emotionen-API wird am 15. Februar 2019 eingestellt. Die Funktion zur Erkennung von Emotionen ist jetzt als Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/) allgemein verfügbar.

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Wenn Sie in diesen FAQs keine Antwort auf Ihre Frage finden, können Sie die Fragen unter [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) und an die Community für die Emotionen-API stellen oder wenden Sie sich an die [Hilfe und den Support bei UserVoice](https://cognitive.uservoice.com/).  

-----

**Frage:** *Mit welchen Arten von Bildern werden mit der Emotionen-API die besten Ergebnisse erzielt?*

**Antwort:** Verwenden Sie für beste Ergebnisse unversperrte, vollständige frontale Gesichtsaufnahmen. Die Zuverlässigkeit nimmt bei partiellen Gesichtsflächen ab und die Emotionen-API erkennt möglicherweise keine Emotionen in Bildern, in denen das Gesicht um mehr als 45 Grad gedreht ist.

-----

**Frage:** *Wie viele Emotionen kann die Emotionen-API erkennen?*

**Antwort:** Die Emotionen-API erkennt acht verschiedene allgemein akzeptierte Emotionen:
* Freude
* Trauer
* Überraschung
* Wut
* Angst
* Verachtung
* Ekel
* Neutral

-----

**Frage:** *Gibt es eine Möglichkeit, einen Live-Videostream an die API weiterzuleiten und gleichzeitig das Ergebnis zu erhalten?*

**Antwort:** Verwenden Sie die bildbasierte Emotionen-API und rufen Sie sie bei jedem Frame auf, oder überspringen Sie Frames für mehr Leistung.  Es sind Frame-für Frame-Analysebeispiele für Videos verfügbar.

-----

**Frage:** *Ich habe die binären Bilddaten eingegeben, erhalte aber die Fehlermeldung: „Gesichtsbild ungültig“.**

**Antwort:** Diese Meldung bedeutet, dass der Algorithmus ein Problem mit dem Analysieren des Bilds hat.  
* Als Eingabebildformate werden JPEG, PNG, GIF (der erste Frame), BMP unterstützt.
* Bilddateien dürfen maximal 4 MB groß sein.
* Die erkennbare Gesichtsgröße reicht von 36 x 36 bis 4.096 x 4.096 Pixel. Außerhalb dieses Bereichs liegende Gesichter werden nicht erkannt.
* Einige Gesichter werden möglicherweise aufgrund technischer Probleme nicht erkannt: z. B. große Gesichtswinkel (Kopfpose) und starke Verdeckung. Frontalansichten und nahezu der Frontalansicht entsprechende Ansichten von Gesichtern führen zu den besten Ergebnissen.

-----
