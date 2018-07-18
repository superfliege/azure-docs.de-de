---
title: Part-of-Speech-Tagging in der API für linguistische Analyse | Microsoft-Dokumentation
description: Erfahren Sie, wie im Rahmen der Wortartmarkierung (Part-of-Speech-Tagging) in Microsoft Cognitive Services die Kategorie oder eine Wortart der einzelnen Wörter des Texts identifiziert wird.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 09/27/2016
ms.author: lesun
ms.openlocfilehash: 90fd5b05c2dabdac88c6c8da288ab629177be38d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082637"
---
# <a name="part-of-speech-tagging"></a>Part-of-Speech-Tagging

## <a name="background-and-motivation"></a>Hintergrund und Motivation

Nachdem Sie einen Text in Sätze und Token segmentiert haben, besteht der nächste Analyseschritt darin, die Kategorie oder die Wortart der einzelnen Wörter zu identifizieren.
Hierzu gehören Kategorien wie etwa *Nomen* (in der Regel Personen, Orte, Gegenstände, Konzepte etc.) und *Verben* (in der Regel Aktionen, Zustandsänderungen etc.). Bei einigen Wörtern lässt sich die Wortart eindeutig bestimmen (z.B. kann *quagmire* (Sumpf) nur ein Nomen sein), bei vielen anderen Wörtern ist diese jedoch schwieriger zu erkennen.
So könnte *table* ein Tisch zum Arbeiten (oder ein 2D-Layout von Zahlen) sein, jedoch auch den Ausdruck „table a discussion“ (eine Diskussion eröffnen) meinen.

## <a name="list-of-part-of-speech-tags"></a>Liste der Wortarttags

| Tag | BESCHREIBUNG | Beispielwörter |
|-----|-------------|---------------|
| $ | US-Dollar | $ |
| \`\` | Öffnendes Anführungszeichen | \` \`\` |
| '' | Schließendes Anführungszeichen | ' '' |
| ( | Öffnende Klammer | ( [ { |
| ) | Schließende Klammer | ) ] } |
| dann | Komma | dann |
| -- | Gedankenstrich | -- |
| zu erstellen und zu verwalten. | Satzendezeichen | zu erstellen und zu verwalten. ! ? |
| : | Doppelpunkt oder Auslassungszeichen | : ; ... |
| CC | Konjunktion, gleichordnend | and, but, or, yet|
| CD | Kardinalzahl | nine, 20, 1980, '96 |
| DT | Artikel |a, the, an, all, both, neither|
| EX | Existenzwort | there |
| FW | Fremdwort | enfant terrible, hoi polloi, je ne sais quoi |
| IN | Präposition oder subordinierende Konjunktion| in, inside, if, upon, whether |
| JJ | Adjektiv oder Ordnungszahl | ninth, pretty, execrable, multimodal |
| JJR | Adjektiv, Komparativ | better, faster, cheaper |
| JJS | Adjektiv, Superlativ | best, fastest, cheapest | 
| LS | Listenelementmarkierung | (a) (b), 1 2, A B, A. B. |
| MD | Modales Hilfsverb | can, may, shall, will, could, might, should, ought |
| NN | Gattungsname, Singular oder Stoffname | potato, money, shoe |
| NNP | Eigenname, Singular | Kennedy, Roosevelt, Chicago, Weehawken |
| NNPS | Eigenname, Plural | Springfields, Bushes |
| NNS | Gattungsname, Plural | pieces, mice, fields |
| PDT | Predeterminer | all, both, half, many, quite, such, sure, this |
| POS | Genitivmarkierung | ', 's |
| PRP | Pronomen, Personal | she, he, it, I, we, they, you |
| PRP$ | Pronomen, Possessiv | hers, his, its, my, our, their, your |
| RB | Adverb | clinically, only |
| RBR | Adverb, Komparativ | further, gloomier, grander, graver, greater, grimmer, harder, harsher, healthier, heavier, higher, however, larger, later, leaner, lengthier, less perfectly, lesser, lonelier, longer, louder, lower, more... |
| RBS | Adverb, Superlativ | best, biggest, bluntest, earliest, farthest, first, furthest, hardest, heartiest, highest, largest, least, less, most, nearest, second, tightest, worst |
| RP | Partikel | on, off, up, out, about |
| SYM | Symbol | %, & |
| TO | „to“ als Präposition oder Infinitivmarkierung | in: |
| UH | Ausruf | uh, hooray, howdy, hello |
| VB | Verb, Grundform | give, assign, fly |
| VBD | Verb, Vergangenheitsform | gave, assigned, flew |
| VBG | Verb, Präsenspartizip oder Gerundium | giving, assigning, flying |
| VBN | Verb, Perfektpartizip | given, assigned, flown |
| VBP | Verb, Präsens, nicht 3. Person Singular | give, assign, fly |
| VBZ | Verb, Präsens, 3. Person Singular | gives, assigns, flies |
| WDT | WH-Determiner, Determinativ mit W-Wort | that, what, which |
| WP | WH-Pronoun, Pronomen mit W-Wort | who, whom |
| WP$ | WH-Pronoun, Possessivpronomen mit W-Wort | whose |
| WRB | Wh-adverb, Adverb mit W-Wort | how, however, whenever, where |

## <a name="specification"></a>Spezifikation

Für die Tokenisierung wird die Spezifikation von der [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42) herangezogen.
