---
title: Transkriptionsrichtlinien zur Spracherkennungsdienst-Schulung
description: Informationen zur Textvorbereitung zum Anpassen von Akustik- und Sprachmodellen und Voicefonts für den Spracherkennungsdienst.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/01/2018
ms.author: panosper
ms.openlocfilehash: f9cb205b5111e981ee70adca715139402c9e31a4
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721814"
---
# <a name="transcription-guidelines-for-using-the-speech-service"></a>Transkriptionsrichtlinien zur Verwendung des Spracherkennungsdiensts

Zur Anpassung der **Spracherkennung** oder **Sprachsynthese** müssen Sie Text zusammen mit Sprache angeben. Jede Zeile im Text entspricht einer einzelnen Äußerung. Der Text sollte mit der Sprache so genau wie möglich übereinstimmen. Der Text wird als *Transkript* bezeichnet und muss in einem bestimmten Format erstellt werden.

Der Spracherkennungsdienst normalisiert die Eingabe, damit der Text konsistent bleibt. 

In diesem Artikel werden beide Arten der Normalisierung beschrieben. Je nach Sprache weichen die Richtlinien geringfügig voneinander ab.

## <a name="us-english-en-us"></a>Englisch (USA) (en-us)

Die Textdaten müssen mit einer Äußerung pro Zeile als Nur-Text unter ausschließlicher Verwendung des ASCII-Zeichensatzes geschrieben werden.

Vermeiden Sie die Verwendung von erweiterten (Lateinisch-1) oder Unicode-Interpunktionszeichen. Diese Zeichen können unbeabsichtigt eingefügt werden, wenn die Daten in einem Textverarbeitungsprogramm vorbereitet oder aus Webseiten erfasst werden. Ersetzen Sie die Zeichen durch entsprechende ASCII-Zeichen. Beispiel: 

| Nicht zu verwendende Zeichen | Ersetzung |
|----- | ----- |
| „Hello World“ (öffnende und schließende doppelte Anführungszeichen) | "Hello world" (doppelte Anführungszeichen) |
| John’s day (einfaches Anführungszeichen rechts) | John's day (Apostroph) |
| it was good—no, it was great! (Geviertstrich) | it was good--no, it was great! (Bindestriche) |

### <a name="text-normalization-rules-for-english"></a>Regeln für die Textnormalisierung für Englisch

Der Spracherkennungsdienst verwendet die folgenden Normalisierungsregeln:

* Verwenden von Kleinbuchstaben für sämtlichen Text
* Entfernen aller Interpunktionszeichen, mit Ausnahme von wortinternen Apostrophen
* Ausweiten von Zahlen auf die gesprochene Form, einschließlich Dollarangaben

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| "Holy cow!" said Batman. | holy cow said batman |
| "What?" said Batman's sidekick, Robin. | what said batman's sidekick robin |
| Go get -em! | go get em |
| I'm double-jointed | I'm double jointed |
| 104 Elm Street | one oh four Elm street |
| Tune to 102.7 | tune to one oh two seven |
| Pi is about 3.14 | pi is about three point one four |
| It costs $3.14 | it costs three fourteen |

Wenden Sie die folgende Normalisierung auf Ihre Texttranskripte an:

* Abkürzungen sollten ausgeschrieben werden.
* Nicht standardmäßige numerische Zeichenfolgen (z. B. einige Datums- oder Rechnungsformen) müssen ausgeschrieben werden.
* Wörter mit nicht alphabetischen Zeichen oder gemischten alphanumerischen Zeichen müssen entsprechend ihrer Aussprache transkribiert werden.
* Lassen Sie Abkürzungen, die als Wörter ausgesprochen werden, unverändert (z. B. „Radar“, „Laser“, „RAM“ oder „NATO“).
* Schreiben Sie Abkürzungen, die als separate Buchstaben ausgesprochen werden, mit durch Leerzeichen getrennten Buchstaben (Beispiel: „IBM,“ „CPU,“ „FBI,“ „TBD,“ oder „NaN“). 

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 14 NE 3rd Dr. | fourteen northeast third drive |
| Dr. Bruce Banner | Doctor Bruce Banner |
| James Bond, 007 | James Bond, double oh seven |
| Ke$ha | Kesha |
| How long is the 2x4 | How long is the two by four |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| my blood type is O+ | My blood type is O positive |
| water is H20 | water is H 2 O |
| play OU812 by Van Halen | play O U 8 1 2 by Van Halen |
| UTF-8 with BOM | U T F 8 with BOM |

## <a name="chinese-zh-cn"></a>Chinesisch (zh-cn)

Bei in Custom Speech Service hochgeladenen Textdaten muss die UTF-8-Codierung mit Bytereihenfolge-Marke verwendet werden. Die Datei sollte eine Äußerung pro Zeile enthalten.

Vermeiden Sie die Verwendung von Interpunktionszeichen halber Breite. Diese Zeichen können unbeabsichtigt eingefügt werden, wenn die Daten in einem Textverarbeitungsprogramm vorbereitet oder aus Webseiten erfasst werden. Ersetzen Sie sie durch entsprechende Zeichen normaler Breite. Beispiel: 

| Nicht zu verwendende Zeichen | Ersetzung |
|----- | ----- |
| „你好“ (öffnende und schließende doppelte Anführungszeichen) | "你好" (doppelte Anführungszeichen) |
| 需要什么帮助? (Fragezeichen) | 需要什么帮助？ |

### <a name="text-normalization-rules-for-chinese"></a>Regeln für die Textnormalisierung für Chinesisch

Der Spracherkennungsdienst verwendet die folgenden Normalisierungsregeln:

* Entfernen aller Interpunktionszeichen
* Ausweitung von Zahlen auf die gesprochene Form
* Konvertieren von Buchstaben normaler Breite in Buchstaben halber Breite
* Verwenden von Großbuchstaben für alle englischen Wörter

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

Bevor Sie Ihren Text importieren, wenden Sie die folgende Normalisierung an:

* Abkürzungen sollten ausgeschrieben werden (wie in gesprochener Form).
* Schreiben Sie numerische Zeichenfolgen wie in gesprochener Form aus.

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Weitere Sprachen

Bei in den Dienst zur **Spracherkennung** hochgeladenen Textdaten muss die UTF-8-Codierung mit Bytereihenfolge-Marke verwendet werden. Die Datei sollte eine Äußerung pro Zeile enthalten.

> [!NOTE]
> Die folgenden Beispiele verwenden Deutsch. Die Richtlinien gelten jedoch für alle Sprachen außer für Englisch (USA) oder Chinesisch.

### <a name="text-normalization-rules-for-german"></a>Regeln für die Textnormalisierung für Deutsch

Der Spracherkennungsdienst verwendet die folgenden Normalisierungsregeln:

* Verwenden von Kleinbuchstaben für sämtlichen Text
* Entfernen aller Interpunktionszeichen, einschließlich verschiedener Typen von Anführungszeichen ("test", 'test', „test“ oder «test» können verwendet werden)
* Verwerfen aller Zeilen mit Sonderzeichen des Zeichensatzes ¢ ¤ ¥ ¦ § © ª ¬ ® ° ± ² µ × ÿ Ø¬¬
* Erweitern von Zahlen in die Wortform, einschließlich Dollar-/Euroangaben
* Zulassen von Umlauten nur für a, o und u; andere werden durch „th“ ersetzt oder entfernt

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |

Bevor Sie Ihren Text importieren, wenden Sie die folgende Normalisierung an:

* Als Dezimaltrennzeichen muss „,“ statt „.“ verwendet werden.
* Als Zeittrennzeichen zwischen Stunden und Minuten muss „:“ anstatt „.“ verwendet werden (Beispiel: 12:00 Uhr).
* Abkürzungen wie „ca.“ werden nicht ersetzt. Es empfiehlt sich, die vollständige Form zu verwenden.
* Die vier wichtigsten mathematischen Operatoren (+, -, \* und /) werden entfernt. Es wird empfohlen, sie durch die jeweilige Literalform zu ersetzen: „plus“, „minus“, „mal“ und „geteilt“.
* Dieselbe Regel gilt für Vergleichsoperatoren (=, < und >). Wir empfehlen, sie durch „gleich“, „kleiner als“ und „grösser als“ zu ersetzen.
* Verwenden Sie Brüche wie 3/4 in Wortform (Beispiel: „drei viertel“ anstatt ¾).
* Ersetzen Sie das €-Symbol durch die Wortform „Euro“.

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung durch den Benutzer | Nach der Normalisierung durch das System
|--------  | ----- | -------- |
| Es ist 12.23 Uhr | Es ist 12:23 Uhr | es ist zwölf uhr dreiundzwanzig |
| {12,45} | {12,45} | zwölf komma vier fünf ||
| 2 + 3 - 4 | 2 plus 3 minus 4 | zwei plus drei minus vier|

## <a name="next-steps"></a>Nächste Schritte

- [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
