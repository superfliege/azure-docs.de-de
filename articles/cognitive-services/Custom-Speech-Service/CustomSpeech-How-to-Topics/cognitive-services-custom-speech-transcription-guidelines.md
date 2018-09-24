---
title: Richtlinien für die Transkription im Custom Speech Service unter Azure | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Ihre Daten für den Custom Speech Service in Cognitive Services vorbereiten.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 2772665107f94a273731d2147154e546225b22dd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46999731"
---
# <a name="transcription-guidelines"></a>Richtlinien für die Transkription
Halten Sie sich an die folgenden Richtlinien für die Transkription, um sicherzustellen, dass Ihre Textdaten für die Anpassung des Akustik- und Sprachmodells bestmöglich genutzt werden. Diese Richtlinien sind sprachspezifisch.

## <a name="text-normalization"></a>Textnormalisierung

Für die optimale Nutzung zur Anpassung des Akustik- oder Sprachmodells müssen die Textdaten normalisiert werden. Dies bedeutet, dass sie in eine unzweideutige Standardform transformiert werden müssen, damit sie für das System lesbar sind. In diesem Abschnitt wird die Textnormalisierung beschrieben, die vom Custom Speech Service beim Import von Daten durchgeführt wird, sowie die Textnormalisierung, die der Benutzer vor dem Datenimport durchführen muss.

## <a name="inverse-text-normalization"></a>Inverse Textnormalisierung

Der Prozess der Konvertierung eines unformatierten Texts in einen formatiertem Text, d.h. mit Groß-/Kleinschreibung und Interpunktion, wird als inverse Textnormalisierung (ITN) bezeichnet. Der ITN-Vorgang erfolgt basierend auf Ergebnissen, die von der Microsoft Cognitive Services-Speech-API zurückgegeben werden. Für einen benutzerdefinierten Endpunkt, der über den Custom Speech Service bereitgestellt wird, wird der gleiche ITN-Vorgang wie für die Microsoft Cognitive Services-Speech-API verwendet. Derzeit unterstützt dieser Dienst aber keine benutzerdefinierten ITN-Vorgänge, sodass neue Begriffe, die mit einem benutzerdefinierten Sprachmodell eingeführt werden, in den Erkennungsergebnissen nicht formatiert sind.

## <a name="transcription-guidelines-for-en-us"></a>Richtlinien für die Transkription für „en-US“

Die in diesen Dienst hochgeladenen Textdaten müssen als Nur-Text unter ausschließlicher Verwendung des druckbaren ASCII-Zeichensatzes geschrieben werden. Jede Zeile der Datei sollte nur den Text für eine einzelne Äußerung enthalten.

Es ist wichtig, dass Sie die Verwendung von Unicode-Interpunktionszeichen vermeiden. Dies kann versehentlich geschehen, wenn die Daten in einem Textverarbeitungsprogramm aufbereitet oder Daten von Webseiten genutzt werden. Ersetzen Sie diese Zeichen durch entsprechende ASCII-Zeichen. Beispiel: 

| Zu vermeidender Unicode | ASCII-Ersatzzeichen |
|----- | ----- |
| “Hello world” (öffnende und schließende doppelte Anführungszeichen) | "Hello world" (doppelte Anführungszeichen) |
| John’s day (einfaches Anführungszeichen rechts) | John's day (Apostroph) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Vom Custom Speech Service durchgeführte Textnormalisierung

Dieser Dienst führt die folgende Textnormalisierung für Daten durch, die als Sprachdataset oder als Transkriptionen für ein akustisches Dataset importiert wurden. Dies umfasst:

*   Kleinschreibung des gesamten Texts
*   Entfernen aller Interpunktionszeichen, mit Ausnahme von wortinternen Apostrophen
*   Ausweitung von Zahlen auf die gesprochene Form, einschließlich Dollarangaben

Einige Beispiele (in englischer Sprache):

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| Starbucks Coffee | starbucks coffee |
| “Holy cow!” said Batman. | holy cow said batman |
| “What?” said Batman’s sidekick, Robin. | what said batman's sidekick robin |
| Go get -em! | go get em |
| I’m double-jointed | i’m double jointed |
| 104 Main Street | one oh four main street |
| Tune to 102.7 | tune to one oh two point seven |
| Pi is about 3.14 | pi is about three point one four |
| It costs $3.14 | it costs three fourteen |

### <a name="text-normalization-required-by-users"></a>Erforderliche Textnormalisierung durch Benutzer

Damit Ihre Daten bestmöglich genutzt werden, sollten vor dem Import die folgenden Normalisierungsregeln angewendet werden.

*   Abkürzungen sollten in gesprochener Form wiedergegeben werden und müssen daher ausgeschrieben werden.
*   Nicht standardmäßige numerische Zeichenfolgen sollten ausgeschrieben werden.
*   Wörter mit nicht alphabetischen Zeichen oder gemischten alphanumerischen Zeichen müssen entsprechend ihrer Aussprache transkribiert werden.
*   Häufig verwendete Akronyme können als einzelne Entität ohne Punkte oder Leerzeichen zwischen den Buchstaben beibehalten werden, aber alle anderen Akronyme sollten als separate Buchstaben ausgeschrieben werden, wobei die Buchstaben jeweils durch ein Leerzeichen getrennt sind.

Einige Beispiele (in englischer Sprache):

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 14 NE 3rd Dr. | fourteen northeast third drive |
| Dr. Strangelove | Doctor Strangelove |
| James Bond 007 | james bond double oh seven |
| Ke$ha | Kesha |
| How long is the 2x4 | How long is the two by four |
| The meeting goes from 1-3pm | The meeting goes from one to three pm |
| my blood type is O+ | My blood type is O positive |
| water is H20 | water is H 2 O |
| play OU812 by Van Halen | play O U 8 1 2 by Van Halen |

## <a name="transcription-guidelines-for-zh-cn"></a>Richtlinien für die Transkription für „zh-CN“

Bei in Custom Speech Service hochgeladenen Textdaten muss die **UTF-8-Codierung (einschließlich Bytereihenfolge-Marke)** verwendet werden. Jede Zeile der Datei sollte nur den Text für eine einzelne Äußerung enthalten.

Die Verwendung von Interpunktionszeichen halber Breite ist zu vermeiden. Dies kann versehentlich geschehen, wenn die Daten in einem Textverarbeitungsprogramm aufbereitet oder Daten von Webseiten genutzt werden. Ersetzen Sie diese Zeichen durch entsprechende Zeichen mit voller Breite. Beispiel: 

| Zu vermeidender Unicode | ASCII-Ersatzzeichen |
|----- | ----- |
| “你好” (öffnende und schließende doppelte Anführungszeichen) | "你好" (doppelte Anführungszeichen) |
| 需要什么帮助? (Fragezeichen) | 需要什么帮助？ |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Vom Custom Speech Service durchgeführte Textnormalisierung

Dieser Spracherkennungsdienst führt die folgende Textnormalisierung für Daten durch, die als Sprachdataset oder als Transkriptionen für ein akustisches Dataset importiert wurden. Dies umfasst:

*   Entfernen aller Interpunktionszeichen
*   Erweiterung von Zahlen in die gesprochene Form
*   Konvertieren von Buchstaben normaler Breite in Buchstaben halber Breite
*   Großschreibung aller englischen Wörter

Hier einige Beispiele:

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗 ? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-required-by-users"></a>Erforderliche Textnormalisierung durch Benutzer

Damit Ihre Daten bestmöglich genutzt werden, sollten _vor_ dem Import die folgenden Normalisierungsregeln angewendet werden.

*   Abkürzungen sollten in gesprochener Form wiedergegeben werden und müssen daher ausgeschrieben werden.
*   Der Dienst deckt nicht alle numerischen Größen ab. Es ist zuverlässiger, numerische Zeichenfolgen in gesprochener Form auszuschreiben.

Einige Beispiele (in chinesischer Sprache):

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>Richtlinien für die Transkription für „de-DE“

Bei in Custom Speech Service hochgeladenen Textdaten sollte nur die **UTF-8-Codierung (einschließlich Bytereihenfolge-Marke)** verwendet werden. Jede Zeile der Datei sollte nur den Text für eine einzelne Äußerung enthalten.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Vom Custom Speech Service durchgeführte Textnormalisierung

Dieser Dienst führt die folgende Textnormalisierung für Daten durch, die als Sprachdataset oder als Transkriptionen für ein akustisches Dataset importiert wurden. Dies umfasst:

*   Kleinschreibung des gesamten Texts
*   Entfernen aller Interpunktionszeichen, einschließlich englischer bzw. deutscher Anführungszeichen ("test", 'test', “test„ und «test» sind zulässig)
*   Verwerfen aller Zeilen mit Sonderzeichen, z.B.: ^ ¢ £ ¤ ¥ ¦ § © ª ¬ ® ° ± ² µ × ÿ Ø¬¬
*   Erweiterung von Zahlen in die Wortform, einschließlich Dollar-/Euroangaben
*   Es werden nur die Umlaute für „a“, „o“ und „u“ akzeptiert. Andere Fälle werden durch „th“ ersetzt oder verworfen.

Einige Beispiele (in deutscher Sprache):

| Ursprünglicher Text | Nach der Normalisierung |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| "Hallo, Mama!" sagt die Tochter. | hallo mama sagt die tochter |
| ¡Eine Frage! | eine frage |
| wir, haben | wir haben |
| Das macht $10 | das macht zehn dollar |


### <a name="text-normalization-required-by-users"></a>Erforderliche Textnormalisierung durch Benutzer

Damit Ihre Daten bestmöglich genutzt werden, sollten vor dem Import die folgenden Normalisierungsregeln angewendet werden.

*   Als Dezimaltrennzeichen muss „,“ anstatt „.“ verwendet werden, z.B. 2,3% anstatt 2.3%.
*   Als Zeittrennzeichen zwischen Stunden und Minuten muss „:“ anstatt „.“ verwendet werden, z.B. 12:00 Uhr.
*   Abkürzungen wie „ca.“ und „bzw.“ werden nicht ersetzt. Wir empfehlen, die vollständige Schreibweise zu verwenden, damit die richtige Aussprache vorhanden ist.
*   Die vier wichtigsten mathematischen Operatoren werden entfernt: +, -, \*, /.
 Es wird empfohlen, sie durch die Wortformen „plus“, „minus“, „mal“ und „geteilt“ zu ersetzen.
*   Dasselbe gilt für die Vergleichsoperatoren (=, <, >): gleich, kleiner als, größer als.
*   Verwenden Sie für Brüche, z.B. 3/4, die Wortform „drei Viertel“ anstelle von „¾“.
*   Ersetzen Sie das €-Symbol durch die Wortform „Euro“.


Einige Beispiele (in deutscher Sprache):

| Ursprünglicher Text | Nach der Normalisierung durch den Benutzer | Nach der Normalisierung durch das System
|--------  | ----- | -------- |
| Es ist 12.23 Uhr | Es ist 12:23 Uhr | es ist zwölf uhr dreiundzwanzig |
| {12,45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als fünf |
| 2 + 3 - 4 | 2 plus 3 minus 4 | zwei plus drei minus vier|
| Das macht 12 € | Das macht 12 Euro | das macht zwölf euro |



## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines benutzerdefinierten Spracherkennungsendpunkts](cognitive-services-custom-speech-create-endpoint.md)
* Erhöhen Sie die Genauigkeit mit Ihrem [benutzerdefinierten Akustikmodell](cognitive-services-custom-speech-create-acoustic-model.md).
* Erhöhen Sie die Genauigkeit mit einem [benutzerdefinierten Sprachmodell](cognitive-services-custom-speech-create-language-model.md).
