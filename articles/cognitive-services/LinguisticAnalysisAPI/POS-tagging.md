---
title: Part-of-Speech-Tagging in der API für linguistische Analyse | Microsoft-Dokumentation
description: Erfahren Sie, wie im Rahmen der Wortartmarkierung (Part-of-Speech-Tagging) in Microsoft Cognitive Services die Kategorie oder die Wortart der einzelnen Wörter des Texts identifiziert wird.
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
Hierzu gehören Kategorien wie etwa *Nomen* (in der Regel Personen, Orte, Gegenstände, Konzepte etc.) und *Verben* (in der Regel Aktionen, Zustandsänderungen etc.). Bei einigen Wörtern lässt sich die Wortart eindeutig bestimmen (*Sumpf* ist beispielsweise tatsächlich nur ein Nomen), bei vielen anderen Wörtern ist die Wortart jedoch schwieriger zu erkennen.
So könnte *table* ein Tisch zum Arbeiten (oder ein 2D-Layout von Zahlen) sein, jedoch auch den Ausdruck „table a discussion“ (eine Diskussion eröffnen) meinen.

## <a name="list-of-part-of-speech-tags"></a>Liste der Wortarttags

| Tag | Beschreibung | Beispielwörter |
|-----|-------------|---------------|
| $ | Dollar | $ |
| \`\` | Öffnendes Anführungszeichen | \` \`\` |
| '' | Schließendes Anführungszeichen | ' '' |
| ( | Öffnende Klammer | ( [ { |
| ) | Schließende Klammer | ) ] } |
| , | Komma | , |
| -- | Gedankenstrich | -- |
| . | Satzendezeichen | . ! ? |
| : | Doppelpunkt oder Auslassungszeichen | : ; ... |
| CC | Konjunktion, gleichordnend | und, aber, oder, jedoch|
| CD | Kardinalzahl | neun, 20, 1980, '96 |
| DT | Artikel |ein, der, eine, alle, beide, keine|
| EX | Existenzwort | dort |
| FW | Fremdwort | enfant terrible, hoi polloi, je ne sais quoi |
| IN | Präposition oder subordinierende Konjunktion| in, innerhalb, falls, bei, ob |
| JJ | Adjektiv oder Ordnungszahl | neunte, hübsch, scheußlich, multimodal |
| JJR | Adjektiv, Komparativ | besser, schneller, günstiger |
| JJS | Adjektiv, Superlativ | beste, schnellste, günstigste | 
| LS | Listenelementmarkierung | (a) (b), 1 2, A B, A. B. |
| MD | Modales Hilfsverb | können, sollen, werden, könnten, sollten |
| NN | Gattungsname, Singular oder Stoffname | Kartoffel, Geld, Schuh |
| NNP | Eigenname, Singular | Kennedy, Roosevelt, Chicago, Weehawken |
| NNPS | Eigenname, Plural | Springfields, Bushes |
| NNS | Gattungsname, Plural | Stücke, Mäuse, Felder |
| PDT | Predeterminer | alle, beide, halb, viele, ziemlich, solch, sicher, dies |
| POS | Genitivmarkierung | s |
| PRP | Pronomen, Personal | sie, er, es, ich, wir, sie, du |
| PRP$ | Pronomen, Possessiv | ihr, sein, mein, unser, deren, dein |
| RB | Adverb | klinisch, nur |
| RBR | Adverb, Komparativ | weitere, dunkler, größer, ernster, stärker, grimmiger, härter, rauer, gesünder, schwerer, höher, hingegen, später, hagerer, langwieriger, weniger perfekt, geringer, einsamer, länger, lauter, niedriger, mehr... |
| RBS | Adverb, Superlativ | am besten, am größten, am stumpfsten, frühestens, am weitesten, zuerst, am härtesten, am herzlichsten, am höchsten, am größten, am wenigsten, kleiner, am meisten, am nächsten, zweitens, am engsten, am schlechtesten |
| RP | Partikel | ein, aus, hoch, herunter, hinaus, über |
| SYM | Symbol | %, & |
| TO | „zu“ als Präposition oder Infinitivmarkierung | zu |
| UH | Ausruf | äh, hurra, hallo |
| VB | Verb, Grundform | geben, zuweisen, fliegen |
| VBD | Verb, Vergangenheitsform | gab, wies zu, flog |
| VBG | Verb, Präsenspartizip oder Gerundium | gebend, zuweisend, fliegend |
| VBN | Verb, Perfektpartizip | gegeben, zugewiesen, geflogen |
| VBP | Verb, Präsens, nicht 3. Person Singular | geben, zuweisen, fliegen |
| VBZ | Verb, Präsens, 3. Person Singular | gibt, weist zu, fliegt |
| WDT | W-Determinativ | welcher, welche, was |
| WP | W-Pronomen | wer, wem |
| WP$ | W-Possessivpronomen | wessen |
| WRB | W-Adverb | wie, wiederum, wann auch immer, wo |

## <a name="specification"></a>Spezifikation

Für die Tokenisierung wird die Spezifikation von der [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42) herangezogen.
