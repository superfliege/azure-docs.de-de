---
title: Transkriptionsrichtlinien für das Sprachtraining | Microsoft-Dokumentation
description: Informationen zur Textvorbereitung zum Anpassen von Akustik- und Sprachmodellen und Voicefonts für den Speech-Dienst.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 93ab7c81a773f692b2b970bb1901d82b7aceb5a2
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "35378861"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Transkriptionsrichtlinien zur Verwendung des Speech-Diensts

Zur Anpassung der **Spracherkennung** oder **Sprachsynthese** müssen Sie Text zusammen mit Sprache angeben. Jede Zeile im Text entspricht einer einzelnen Äußerung. Der Text wird als *Transkript* bezeichnet und muss in einem bestimmten Format erstellt werden.

Der Speech-Dienst führt für Sie einige Normalisierungen durch, damit der Text konsistent wird. Andere Normalisierungsaufgaben müssen vor dem Übermitteln des Texts zum Training ausgeführt werden. 

In diesem Artikel werden beide Arten der Normalisierung beschrieben. Je nach Sprache weichen die Richtlinien geringfügig voneinander ab.

## <a name="us-english-en-us"></a>Englisch (USA) (en-US)

Die in diesen Dienst hochgeladenen Textdaten müssen als Nur-Text unter ausschließlicher Verwendung des ASCII-Zeichensatzes geschrieben werden. Jede Zeile der Datei muss den Text für eine einzelne Äußerung enthalten.

Die Verwendung von Interpunktionszeichen des erweiterten (Lateinisch-1) oder Unicodeblocks ist zu vermeiden. Diese Zeichen können unbeabsichtigt eingefügt werden, wenn die Daten in einem Textverarbeitungsprogramm vorbereitet oder aus Webseiten erfasst werden. Ersetzen Sie diese Zeichen durch entsprechende ASCII-Zeichen. Beispiel: 

| Nicht zu verwendende Zeichen | Ersetzung |
|----- | ----- |
| “Hello world” (öffnendes und schließendes Anführungszeichen) | "Hello world" (doppelte Anführungszeichen) |
| John’s day (einfaches Anführungszeichen rechts) | John's day (Apostroph) |
| it was good—no, it was great! (Geviertstrich) | it was good--no, it was great! (Bindestriche) |

### <a name="text-normalization-performed-by-the-service"></a>Vom Dienst durchgeführte Textnormalisierung

Der Speech-Dienst führt die folgende Textnormalisierung in Texttranskripten durch.

*   Kleinschreibung des gesamten Texts
*   Entfernen aller Interpunktionszeichen, mit Ausnahme von wortinternen Apostrophen
*   Ausweitung von Zahlen auf die gesprochene Form, einschließlich Dollarangaben

Einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| Starbucks Coffee | starbucks coffee |
| "Holy cow!" said Batman. | holy cow said batman |
| "What?" said Batman's sidekick, Robin. | what said batman's sidekick robin |
| Go get -em! | go get em |
| I'm double-jointed | i'm double jointed |
| 104 Main Street | one oh four main street |
| Tune to 102.7 | tune to one oh two point seven |
| Pi is about 3.14 | pi is about three point one four |
| It costs $3.14 | it costs three fourteen |

### <a name="text-normalization-you-must-perform"></a>Von Ihnen durchzuführende Textnormalisierung

Wenden Sie die folgende Normalisierung auf Ihre Texttranskripte an.

*   Abkürzungen sind in gesprochener Form wiederzugeben und müssen daher ausgeschrieben werden.
*   Nicht standardmäßige numerische Zeichenfolgen (z.B. einige Datums- oder Rechnungsformen) müssen ausgeschrieben werden.
*   Wörter mit nicht alphabetischen Zeichen oder gemischten alphanumerischen Zeichen müssen entsprechend ihrer Aussprache transkribiert werden.
*   Behalten Sie Abkürzungen, die als Wörter ausgesprochen werden, unverändert bei, z.B. radar, laser, RAM, NATO und Mr.
*   Schreiben Sie Abkürzungen, die als separate Buchstaben ausgesprochen werden, mit durch Leerzeichen getrennten Buchstaben, z.B. IBM, CPU, FBI, TBD, NaN. 

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 14 NE 3rd Dr. | fourteen northeast third drive |
| Dr. Strangelove | Doctor Strangelove |
| James Bond 007 | James Bond double oh seven |
| Ke$ha | Kesha |
| How long is the 2x4 | How long is the two by four |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| my blood type is O+ | My blood type is O positive |
| water is H20 | water is H 2 O |
| play OU812 by Van Halen | play O U 8 1 2 by Van Halen |
| UTF-8 with BOM | U T F 8 with BOM |

## <a name="chinese-zh-cn"></a>Chinesisch (zh-CN)

Bei in Custom Speech Service hochgeladenen Textdaten muss die UTF-8-Codierung mit Bytereihenfolge-Marke verwendet werden. Jede Zeile der Datei muss den Text für eine einzelne Äußerung enthalten.

Die Verwendung von Interpunktionszeichen halber Breite ist zu vermeiden. Diese Zeichen können unbeabsichtigt eingefügt werden, wenn die Daten in einem Textverarbeitungsprogramm vorbereitet oder aus Webseiten erfasst werden. Ersetzen Sie sie durch entsprechende Zeichen normaler Breite. Beispiel: 

| Nicht zu verwendende Zeichen | Ersetzung |
|----- | ----- |
| “你好”(öffnendes und schließendes Anführungszeichen) | "你好" (doppelte Anführungszeichen) |
| 需要什么帮助? (Fragezeichen) | 需要什么帮助？ |

### <a name="text-normalization-performed-by-the-service"></a>Vom Dienst durchgeführte Textnormalisierung

Der Speech-Dienst führt die folgende Textnormalisierung in Texttranskripten durch.

*   Entfernen aller Interpunktionszeichen
*   Ausweitung von Zahlen auf die gesprochene Form
*   Konvertieren von Buchstaben normaler Breite in Buchstaben halber Breite
*   Großschreibung aller englischen Wörter

Hier einige Beispiele.

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗 ? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-you-must-perform"></a>Von Ihnen durchzuführende Textnormalisierung

Wenden Sie vor dem Import die folgende Normalisierung auf den Text an.

*   Abkürzungen sind in gesprochener Form wiederzugeben und müssen daher ausgeschrieben werden.
*   Der Dienst deckt nicht alle numerischen Größen ab. Es ist zuverlässiger, numerische Zeichenfolgen in gesprochener Form zu schreiben.

Hier einige Beispiele.

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Weitere Sprachen

Bei in den Dienst zur **Spracherkennung** hochgeladenen Textdaten muss die UTF-8-Codierung mit Bytereihenfolge-Marke verwendet werden. Jede Zeile der Datei muss den Text für eine einzelne Äußerung enthalten.

> [!NOTE]
> In diesen Beispielen wird Deutsch verwendet. Diese Richtlinien gelten jedoch für alle Sprachen außer für Englisch (USA) und Chinesisch.

### <a name="text-normalization-performed-by-the-service"></a>Vom Dienst durchgeführte Textnormalisierung

Der Speech-Dienst führt die folgende Textnormalisierung in Texttranskripten durch.

*   Kleinschreibung des gesamten Texts
*   Entfernen aller Interpunktionszeichen, einschließlich verschiedener Typen von Anführungszeichen ("test", 'test', „test“ oder «test» können verwendet werden)
*   Verwerfen aller Zeilen mit Sonderzeichen des Zeichensatzes ^ ¢ £ ¤ ¥ ¦ § © ª ¬ ® ° ± ² µ × ÿ Ø¬¬
*   Ausweitung von Zahlen auf die gesprochene Form, einschließlich Euroangaben
*   Umlaute sind nur für a, o und u zulässig; andere werden durch „th“ ersetzt oder entfernt

Einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| "Hallo, Mama!" sagt die Tochter. | hallo mama sagt die tochter |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |
| Das macht $10 | das macht zehn dollar |

### <a name="text-normalization-you-must-perform"></a>Von Ihnen durchzuführende Textnormalisierung

Wenden Sie vor dem Import die folgende Normalisierung auf den Text an.

*   Als Dezimaltrennzeichen muss „,“ anstelle von „.“ verwendet werden, z.B 2,3 % anstelle von 2.3 %.
*   Als Zeittrennzeichen zwischen Stunden und Minuten muss „:“ anstelle von „.“ verwendet werden, z.B. 12:00 Uhr.
*   Abkürzungen wie „ca.“ und „bzw.“ werden nicht ersetzt. Es empfiehlt sich, die vollständige Form zu verwenden.
*   Die vier wichtigsten mathematischen Operatoren werden entfernt: +, -, \*, /. Es wird empfohlen, sie durch die jeweilige Literalform zu ersetzen: plus, minus, mal, geteilt durch.
*   Dasselbe gilt für die Vergleichsoperatoren (=, <, >): gleich, kleiner als, größer als.
*   Verwenden Sie Brüche, z.B. 3/4, in Wortform, z.B. „drei viertel“ anstatt „¾“.
*   Ersetzen Sie das €-Symbol durch die Wortform „Euro“.

Hier einige Beispiele.

| Ursprünglicher Text | Nach der Normalisierung durch den Benutzer | Nach der Normalisierung durch das System
|--------  | ----- | -------- |
| Es ist 12.23 Uhr | Es ist 12:23 Uhr | es ist zwölf uhr drei und zwanzig |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als fünf |
| 2 + 3 - 4 | 2 plus 3 minus 4 | zwei plus drei minus vier|
| Das macht 12 € | Das macht 12 Euro | das macht zwölf euro |

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-windows.md)
