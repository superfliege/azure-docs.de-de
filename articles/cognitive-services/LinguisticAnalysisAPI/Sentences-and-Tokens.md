---
title: Sätze und Token in der API für linguistische Analyse | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Trennung und Tokenisierung in der API für linguistische Analyse in Cognitive Services.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/21/2016
ms.author: davl
ms.openlocfilehash: 78e539f365728ad540308e9cfb07af44bf6d8fe7
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084041"
---
# <a name="sentence-separation-and-tokenization"></a>Trennung und Tokenisierung von Sätzen

## <a name="background-and-motivation"></a>Hintergrund und Motivation

Der erste Schritt bei der linguistischen Analyse eines Texts ist das Segmentieren des Texts in Sätze und Token.

### <a name="sentence-separation"></a>Segmentieren in Sätze

Die Segmentierung eines Texts in Sätze wirkt auf den ersten Blick einfach: Es muss lediglich nach Satzendemarkierungen gesucht, um die Sätze an diesen Stellen zu segmentieren.
Diese Markierungen erweisen sich jedoch häufig als kompliziert und mehrdeutig.

Betrachten Sie den folgenden Beispieltext:

> What did you say?!? I didn't hear about the director's „new proposal.“ (Ich habe nichts von dieser „neuen Ausschreibung“ des Geschäftsführers gehört.) It's important to Mr. and Mrs. Smith. (Was haben Sie gesagt?!? Ich habe nichts von dieser „neuen Ausschreibung“ des Geschäftsführers gehört. Das ist für Herrn und Frau Schmidt wichtig.)

Dieser Text enthält drei Sätze:

- What did you say?!? (Was haben Sie gesagt?!?)
- I didn't hear about the director's „new proposal.“ (Ich habe nichts von dieser „neuen Ausschreibung“ des Geschäftsführers gehört.)
- It's important to Mr. and Mrs. Smith. (Das ist für Herrn und Frau Schmidt wichtig.)

Ist Ihnen aufgefallen, dass die Enden der Sätze auf sehr unterschiedliche Weise markiert sind?
Der erste Satz endet mit einer Kombination aus Frage- und Ausrufezeichen (mitunter als „Interrobang“ bezeichnet).
Der zweite Satz endet mit einem Punkt, aber das nachfolgende schließende Anführungszeichen sollte in den vorherigen Satz einbezogen werden.
Im dritten Satz können Sie erkennen, dass der Punkt auch zur Markierung von Abkürzungen verwendet werden kann.
Die Interpunktion ist für den Anfang zwar ein guter Anhaltspunkt, es sind jedoch weitere Schritte erforderlich, um die tatsächlichen Satzgrenzen zu identifizieren.

### <a name="tokenization"></a>Tokenisierung

Die nächste Aufgabe besteht darin, diese Sätze in Token zu segmentieren.
In den meisten Fällen werden englische Token durch einen Leerraum segmentiert.
(Die Suche nach Token oder Wörtern ist in der englischen Sprache weitaus einfacher als in der chinesischen Sprache, in der weitestgehend keine Leerräume zwischen Wörtern verwendet werden.
Der erste Satz könnte auf folgende Weise geschrieben werden: „Whatdidyousay?“)

Es gibt einige Knackpunkte.
Zunächst einmal sollte die Zeichensetzung in vielen Fällen (jedoch nicht immer) von dem ihm umgebenden Kontext getrennt werden.
Zudem weist die englische Sprache *Kontraktionen* wie „didn’t“ oder „it’s“ auf, bei denen Wörter komprimiert und auf diese Weise abgekürzt werden. Das Ziel des Tokenizers ist es, die Zeichensequenz in Wörter zu segmentieren.

Kehren wir zu den oben aufgeführten Beispielsätzen zurück.
Jetzt haben wir einen Punkt in die Leerräume (&middot;) zwischen die einzelnen Token eingefügt.

- What &middot; did &middot; you &middot; say &middot; ?!?
- I &middot; did &middot; n't &middot; hear &middot; about &middot; the &middot; director &middot; 's &middot; „ &middot; new &middot; proposal &middot; . &middot; “
- It &middot; 's &middot; important &middot; to &middot; Mr. &middot; and &middot; Mrs. &middot; Smith &middot; .

Beachten Sie, dass es sich bei den meisten Token um Wörter handelt, die im Wörterbuch vorgefunden werden können (z.B. *important*, *director*).
Andere Token hingegen bestehen ausschließlich aus Satzzeichen.
Zu guter Letzt gibt es ungewöhnlichere Token, die z.B. Kontraktionen wie *n’t* für *not* und besitzanzeigende Formen wie *’s* darstellen. Dank dieser Tokenisierung können beispielsweise das Wort *didn’t* und die Phrase *did not* konsistenter behandelt werden.

## <a name="specification"></a>Spezifikation

Es ist wichtig, konsistente Entscheidungen darüber zu treffen, woraus ein Satz und ein Token bestehen.
Wir arbeiten mit der Spezifikation der [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42) (Informationen dazu finden Sie hier: ftp://ftp.cis.upenn.edu/pub/treebank/public_html/tokenization.html).
