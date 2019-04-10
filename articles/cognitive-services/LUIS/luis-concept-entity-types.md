---
title: Entitätstypen
titleSuffix: Language Understanding - Azure Cognitive Services
description: 'Entitäten extrahieren Daten aus der Äußerung. Entitätstypen ermöglichen eine vorhersagbare Extraktion von Daten. Es gibt zwei Arten von Entitäten: maschinell erlernte und nicht maschinell erlernte. Es ist wichtig zu wissen, mit welcher Art von Entität Sie in Äußerungen arbeiten.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: diberry
ms.openlocfilehash: 6e37466145af58a52a86a08a2a873e406c99b9e5
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895544"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Entitätstypen und ihr Zweck in LUIS

Entitäten extrahieren Daten aus der Äußerung. Entitätstypen ermöglichen eine vorhersagbare Extraktion von Daten. Es gibt zwei Arten von Entitäten: maschinell erlernte und nicht maschinell erlernte. Es ist wichtig zu wissen, mit welcher Art von Entität Sie in Äußerungen arbeiten. 

## <a name="entity-compared-to-intent"></a>Entität im Vergleich zu Absichten

Die Entität stellt ein Wort oder einen Ausdruck innerhalb der Äußerung dar, die extrahiert werden soll. Eine Äußerung kann viele Entitäten oder auch überhaupt keine enthalten. Eine Entität stellt eine Klasse dar, einschließlich einer Sammlung ähnlicher Objekte (Orte, Dinge, Personen, Ereignisse oder Konzepte). Entitäten beschreiben Informationen mit Bezug zur Absicht. Manchmal sind sie auch für die Ausführung der Aufgabe Ihrer App wichtig. Eine App zur News-Suche kann beispielsweise Entitäten wie „Thema“, „Quelle“, „Schlüsselwort“ und „Veröffentlichungsdatum“ enthalten, die besonders wichtig für die Suche nach Nachrichten sind. In einer Reisebuchungs-App sind „Ort“, „Datum“, „Airline“, „Reiseklasse“ und „Tickets“ Schlüsselinformationen für die Flugbuchung (relevant für die Absicht „Flug buchen“).

Im Vergleich dazu stellt die Absicht die Vorhersage der gesamten Äußerung dar. 

## <a name="entities-help-with-data-extraction-only"></a>Entitäten dienen nur als Hilfe bei der Datenextraktion

Sie beschriften bzw. kennzeichnen Entitäten nur für die Entitätsextraktion. Die Vorhersage von Absichten wird hierdurch nicht unterstützt.

## <a name="entities-represent-data"></a>Darstellen von Daten durch Entitäten

Entitäten sind Daten, die Sie aus der Äußerung pullen möchten. Dabei kann es sich um einen Namen, ein Datum, einen Produktnamen oder eine Wortgruppe handeln. 

|Äußerung|Entität|Daten|
|--|--|--|
|Kaufe 3 Tickets nach New York|Vordefinierte Anzahl<br>Location.Destination|3<br>New York|
|Kaufe ein Ticket von New York nach London am 5. März|Location.Origin<br>Location.Destination<br>Vordefinierte datetimeV2|New York<br>London<br>5. März 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entitäten sind optional, jedoch dringend empfohlen.

Im Gegensatz zu Absichten sind Entitäten optional. Sie müssen keine Entitäten für sämtliche Konzepte Ihrer App erstellen, sondern lediglich für die, die für Aktionen der Clientanwendung erforderlich sind. 

Wenn Ihre Äußerungen keine Details enthalten, die Ihr Bot für das Fortfahren benötigt, müssen Sie diese auch nicht hinzufügen. Sie können sie bei fortschreitender Entwicklung Ihrer App später hinzufügen. 

Gehen Sie wie folgt vor, wenn Sie nicht sicher sind, wie Sie die Informationen verwenden können: Fügen Sie einige allgemeine vordefinierte Entitäten, z.B. [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal](luis-reference-prebuilt-ordinal.md), [email](luis-reference-prebuilt-email.md) und [phonenumber](luis-reference-prebuilt-phonenumber.md) hinzu.

## <a name="label-for-word-meaning"></a>Bezeichnungen für Wortbedeutungen

Wenn die Wortauswahl oder die Anordnung der Wörter identisch ist, sie aber nicht dieselbe Bedeutung haben, sollten Sie sie nicht mit der Entität bezeichnen. 

In den folgenden Äußerungen ist das Wort `fair` ein Homograph. Die Schreibweise ist identisch, aber die Bedeutung ist eine andere:

|Äußerung|
|--|
|Welche Arten von Jahrmärkten finden in diesem Sommer in der Region Seattle statt?|
|Ist die aktuelle Bewertung für die Seattle-Rezension fair?|

Wenn Sie mit einer Veranstaltungsentität alle Veranstaltungsdaten suchen möchten, bezeichnen Sie das Wort `fair` in der ersten Äußerung, aber nicht in der zweiten.

## <a name="entities-are-shared-across-intents"></a>Wiederverwenden von Entitäten für mehrere Absichten

Entitäten werden für mehrere Absichten verwendet. Sie gehören nicht zu einer bestimmten Absicht. Absichten und Entitäten können semantisch zugeordnet werden, aber es handelt sich nicht um eine exklusive Beziehung.

In der Äußerung „Buche mir ein Ticket nach Paris“ ist „Paris“ eine Entität, die auf den Ort verweist. Indem die in der Äußerung des Benutzers erwähnten Entitäten erkannt werden, unterstützt LUIS Ihre Clientanwendung beim Auswählen der jeweiligen Aktionen, die zum Erfüllen der Benutzeranforderung durchgeführt werden müssen.

## <a name="mark-entities-in-none-intent"></a>Kennzeichnen von Entitäten der Absicht „Keine“

Alle Absichten, auch **Keine**, sollten nach Möglichkeit über gekennzeichnete Entitäten verfügen. Auf diese Weise kann LUIS mehr darüber erfahren, wo sich die Entitäten in den Äußerungen befinden und welche Wörter sich um die Entitäten herum befinden. 

## <a name="entity-status-for-predictions"></a>Vorhersagen zum Entitätsstatus

Sie werden im LUIS-Portal darüber informiert, wenn die Entität in einer Beispieläußerung entweder von der gekennzeichneten Entität abweicht oder einer anderen Entität zu stark ähnelt und daher nicht eindeutig ist. Dies wird in der Beispieläußerung durch eine rote Unterstreichung angezeigt. 

Weitere Informationen finden Sie unter [Vorhersagen zum Entitätsstatus](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Typen von Entitäten

LUIS verfügt über viele Arten von Entitäten. Wählen Sie die Entität basierend darauf aus, wie die Daten extrahiert und nach der Extraktion dargestellt werden sollen.

Entitäten können mit maschinellem Lernen extrahiert werden. So kann LUIS weiterhin lernen, wie die Entität in der Äußerung angezeigt wird. Entitäten können auch ohne maschinelles Lernen extrahiert werden, indem entweder ein Abgleich auf den genauen Text oder anhand eines regulären Ausdrucks durchgeführt wird. Entitäten in Mustern können mit einer gemischten Implementierung extrahiert werden. 

Nachdem die Entität extrahiert wurde, können die Entitätsdaten als einzelne Informationseinheit dargestellt oder mit anderen Entitäten kombiniert werden, damit sie eine Informationseinheit bilden, die von der Clientanwendung verwendet werden kann.

|Durch maschinelles Lernen erworben|Kennzeichnung möglich|Tutorial|Beispiel<br>response|Entitätstyp|Zweck|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Composite**](#composite-entity)|Gruppierung von Entitäten, unabhängig vom Entitätstyp.|
|✔|✔|[✔](luis-quickstart-intent-and-hier-entity.md)|[✔](luis-concept-data-extraction.md#hierarchical-entity-data)|[**Hierarchical**](#hierarchical-entity)|Gruppierung von einfachen Entitäten.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**Auflisten**](#list-entity)|Liste mit Elementen und den zugehörigen Synonymen, die per genauer Textübereinstimmung extrahiert werden.|
|Gemischt||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Pattern.any**](#patternany-entity)|Entität, bei der das Entitätsende schwierig zu ermitteln ist.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Vordefiniert**](#prebuilt-entity)|Bereits trainiert, um verschiedene Arten von Daten zu extrahieren.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Regulärer Ausdruck**](#regular-expression-entity)|Verwendet einen regulären Ausdruck zum Abgleichen von Text.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**Einfach**](#simple-entity)|Enthält ein einzelnes Konzept in einem Wort oder Ausdruck.|

Nur Entitäten, die durch maschinelles Lernen erworben wurden, müssen in den Beispieläußerungen für jede Absicht gekennzeichnet werden. Durch maschinelles Lernen erworbene Entitäten funktionieren am besten, wenn sie mithilfe von [Endpunktabfragen](luis-concept-test.md#endpoint-testing) und durch das [Überprüfen der Endpunktäußerungen](luis-how-to-review-endpoint-utterances.md) getestet werden. 

Pattern.any-Entitäten müssen in [Pattern](luis-how-to-model-intent-pattern.md)-Vorlagenbeispielen gekennzeichnet werden, nicht in den Absichtsbenutzerbeispielen. 

Für gemischte Entitäten wird eine Kombination aus Methoden für die Entitätserkennung verwendet.

## <a name="composite-entity"></a>Entität vom Typ „Composite“

Eine zusammengesetzte Entität besteht aus anderen Entitäten, z.B. vom Typ Vordefiniert, Einfach, Regulärer Ausdruck, Liste oder Hierarchisch. Die einzelnen Entitäten bilden zusammen die gesamte Entität. 

Diese Entität ist gut geeignet, wenn für die Daten Folgendes gilt:

* Sind aufeinander bezogen. 
* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Verwenden eine Vielzahl von Entitätstypen.
* Müssen gruppiert und von der Clientanwendung als eine Informationseinheit verarbeitet werden.
* Verfügen über viele verschiedene Benutzeräußerungen, für die maschinelles Lernen erforderlich ist.

![Entität vom Typ „Composite“](./media/luis-concept-entities/composite-entity.png)

[Tutorial](luis-tutorial-composite-entity.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#composite-entity-data)<br>

## <a name="hierarchical-entity"></a>Entität vom Typ „Hierarchical“

Eine hierarchische Entität ist eine Kategorie von kontextbezogen erlernten einfachen Entitäten, die als untergeordnete Elemente bezeichnet werden.

Diese Entität ist gut geeignet, wenn für die Daten Folgendes gilt:

* Es sind einfache Entitäten.
* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Es werden bestimmte Wörter verwendet, um die einzelnen untergeordneten Entitäten anzugeben. Beispiele für diese Wörter sind: von/nach, Verlassen/Ziel.
* Untergeordnete Elemente sind häufig Teil der gleichen Äußerung. 
* Müssen gruppiert und von einer Client-App als eine Informationseinheit verarbeitet werden.

Sie sollte in folgenden Fällen nicht verwendet werden:

* Sie benötigen eine Entität, die unabhängig vom Kontext Übereinstimmungen für untergeordnete Elemente enthält. Verwenden Sie stattdessen eine [Entität vom Typ „Liste“](#list-entity). 
* Sie benötigen eine Entität für die Beziehung „Übergeordnetes/Untergeordnetes Element“ mit anderen Entitätstypen. Verwenden Sie die [Entität vom Typ „Zusammengesetzt“](#composite-entity).

![Entität vom Typ „Hierarchical“](./media/luis-concept-entities/hierarchical-entity.png)

[Tutorial](luis-quickstart-intent-and-hier-entity.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#hierarchical-entity-data)<br>

### <a name="roles-versus-hierarchical-entities"></a>Rollen und hierarchische Entitäten

Mit den [Rollen](luis-concept-roles.md#roles-versus-hierarchical-entities) eines Musters wird das gleiche Problem wie mit hierarchischen Entitäten gelöst, aber der Vorgang gilt für alle Entitätstypen. Rollen sind derzeit nur in Mustern verfügbar. In Beispieläußerungen von Absichten sind Rollen nicht verfügbar.  

## <a name="list-entity"></a>Entität vom Typ „List“

Listenentitäten stellen einen festen, abgeschlossenen Satz verwandter Wörter zusammen mit ihren Synonymen dar. LUIS ermittelt keine zusätzlichen Werte für Listenentitäten. Suchen Sie mithilfe des Features **Empfehlen** nach Vorschlägen für neue Wörter basierend auf der aktuellen Liste. Wenn mehr als eine Listenentität mit demselben Wert vorhanden ist, wird in der Endpunktabfrage jede Entität zurückgegeben. 

Diese Entität ist gut geeignet, wenn für die Textdaten Folgendes gilt:

* Es handelt sich um einen bekannten Satz.
* Für den Satz werden die maximalen LUIS-[Grenzen](luis-boundaries.md) dieses Entitätstyps nicht überschritten.
* Der Text in der Äußerung ist eine exakte Übereinstimmung mit einem Synonym oder dem kanonischen Namen. LUIS verwendet die Liste über genaue Textübereinstimmungen hinaus nicht. Wortstammerkennung, Pluralformen und andere Varianten lassen sich mit einer Listenentität nicht auflösen. Um Varianten zu behandeln, sollten Sie die Verwendung eines [Musters](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) mit der optionalen Textsyntax in Erwägung ziehen.

![Entität vom Typ „List“](./media/luis-concept-entities/list-entity.png)

[Tutorial](luis-quickstart-intent-and-list-entity.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#list-entity-data)

## <a name="patternany-entity"></a>Entität „Pattern.any“

Pattern.any ist ein Platzhalter variabler Länge, der nur in der Vorlagenäußerung eines Musters verwendet wird, um zu kennzeichnen, wo die Entität beginnt und endet.  

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Endung der Entität kann leicht mit dem verbleibenden Text der Äußerung verwechselt werden. 
[Tutorial](luis-tutorial-pattern.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#patternany-entity-data)

**Beispiel**  
Bei einer Clientanwendung, mit der anhand des Titels nach Büchern gesucht wird, wird mit pattern.any der vollständige Titel extrahiert. Eine Vorlagenäußerung mit pattern.any für diese Buchsuche lautet `Was {BookTitle} written by an American this year[?]` (Wurde {Buchtitel} in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?). 

In der folgenden Tabelle enthält jede Zeile zwei Versionen der Äußerung. In der oberen Äußerung ist dargestellt, wie LUIS die Äußerung zuerst sieht. Hierbei ist unklar, wo der Buchtitel beginnt und endet. Die untere Äußerung gibt an, wie LUIS den Buchtitel erkennen kann, wenn ein Muster für die Extraktion vorhanden ist. 

|Äußerung|
|--|
|Wurde das Buch The Man Who Mistook His Wife for a Hat and Other Clinical Tales in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br>Wurde das Buch **The Man Who Mistook His Wife for a Hat and Other Clinical Tales** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|Wurde Half Asleep in Frog Pajamas in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br>Wurde **Half Asleep in Frog Pajamas** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|Wurde The Particular Sadness of Lemon Cake: A Novel in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br>Wurde **The Particular Sadness of Lemon Cake: A Novel** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|Wurde There's A Wocket In My Pocket! in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br>Wurde **There's A Wocket In My Pocket!** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|

## <a name="prebuilt-entity"></a>Vordefinierte Entität

Bei vordefinierten Entitäten handelt es sich um integrierte Typen, mit denen häufig verwendete Konzepte, z.B. E-Mail, URL und Telefonnummer, dargestellt werden. Vordefinierte Entitätsnamen sind reserviert. [Alle vordefinierten Entitäten](luis-prebuilt-entities.md), die der Anwendung hinzugefügt werden, werden in der Endpunktvorhersage-Abfrage zurückgegeben, wenn sie in der Äußerung enthalten sind. 

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Daten stimmen mit einem häufigen Anwendungsfall überein, der von vordefinierten Entitäten für Ihre Sprachkultur unterstützt wird. 

Vordefinierte Entitäten können jederzeit hinzugefügt und entfernt werden.

![Vordefinierte Entität „number“](./media/luis-concept-entities/number-entity.png)

[Tutorial](luis-tutorial-prebuilt-intents-entities.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#prebuilt-entity-data)

Einige dieser vordefinierten Entitäten werden im Open-Source-Projekt [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) definiert. Wenn Ihre Kultur oder Entität derzeit nicht unterstützt wird, können Sie sich am Projekt beteiligen. 

### <a name="troubleshooting-prebuilt-entities"></a>Problembehandlung bei vordefinierten Entitäten

Wenn anstelle Ihrer benutzerdefinierten Entität eine vordefinierte Entität markiert ist, haben Sie im LUIS-Portal mehrere Optionen zum Beheben dieses Problems.

Die der App hinzugefügten vordefinierten Entitäten werden _immer_ zurückgegeben, auch wenn aus der Äußerung benutzerdefinierte Entitäten für den gleichen Text extrahiert werden. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Ändern der markierten Entität in der Beispieläußerung

Wenn die vordefinierte Entität dem Text oder den Token der benutzerdefinierten Entität entspricht, wählen Sie den Text in der Beispieläußerung aus, und ändern Sie die markierte Äußerung. 

Wenn die vordefinierte Entität mit mehr Text oder Token markiert ist als die benutzerdefinierte Entität, haben Sie zwei Optionen, um dies zu korrigieren:

* [Entfernen der Beispieläußerung](#remove-example-utterance-to-fix-tagging)
* [Entfernen der vordefinierten Entität](#remove-prebuilt-entity-to-fix-tagging)

#### <a name="remove-example-utterance-to-fix-tagging"></a>Entfernen der Beispieläußerung zur Korrektur der Markierung 

Die erste Option ist das Entfernen der Beispieläußerung. 

1. Löschen Sie die Beispieläußerung.
1. Trainieren Sie die App erneut. 
1. Fügen Sie nur das Wort oder den Ausdruck, das bzw. den die vordefinierte Entität markierte Entität darstellt, als vollständige Beispieläußerung ein. Für das Wort oder den Ausdruck ist weiterhin die vordefinierte Entität markiert. 
1. Wählen Sie auf der Seite **Absicht** die Entität in der Beispieläußerung aus, ändern Sie sie in die benutzerdefinierte Entität, und trainieren Sie die App erneut. Dies sollte verhindern, dass in LUIS genau dieser Text als vordefinierte Entität in Beispieläußerungen markiert wird, in denen dieser Text verwendet wird. 
1. Fügen Sie die vollständige ursprüngliche Beispieläußerung wieder auf der Seite „Absicht“ hinzu. Die benutzerdefinierte Entität sollte weiterhin anstelle der vordefinierten Entität markiert werden. Wenn die benutzerdefinierte Entität nicht markiert wird, müssen Sie weitere Beispiele für diesen Text in Äußerungen hinzufügen.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Entfernen der vordefinierten Entität zur Korrektur der Markierung

1. Entfernen Sie die vordefinierte Entität aus der App. 
1. Markieren Sie auf der Seite **Absicht** die benutzerdefinierte Entität in der Beispieläußerung.
1. Trainieren Sie die App.
1. Fügen Sie die vordefinierte Entität wieder in die App ein, und trainieren Sie die App. Bei dieser Korrektur wird davon ausgegangen, dass die vordefinierte Entität kein Teil einer zusammengesetzten Entität ist.

## <a name="regular-expression-entity"></a>Entität vom Typ „RegEx“ 

Ein regulärer Ausdruck ist am besten für unformatierten Text von Äußerungen geeignet. Die Groß-/Kleinschreibung sowie die Kultur werden ignoriert.  Reguläre Ausdrücke werden nach Änderungen durch die Rechtschreibprüfung auf Zeichenebene (nicht auf Tokenebene) angewandt. Wenn der reguläre Ausdruck zu komplex ist (z.B. mit zu vielen Klammern), können Sie den Ausdruck dem Modell nicht hinzufügen. Es wird ein Teil der [.NET Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions)-Bibliothek verwendet, aber nicht die gesamte Bibliothek. 

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Daten sind einheitlich formatiert, und auch bei den Abweichungen herrscht Einheitlichkeit.
* Für den regulären Ausdruck sind nicht mehr als zwei Schachtelungsebenen erforderlich. 

![Entität vom Typ „RegEx“](./media/luis-concept-entities/regex-entity.png)

[Tutorial](luis-quickstart-intents-regex-entity.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#regular-expression-entity-data)<br>

## <a name="simple-entity"></a>Entität vom Typ „Simple“ 

Eine einfache Entität ist eine generische Entität, die ein einzelnes Konzept beschreibt und im Kontext des maschinellen Lernens erworben wurde. Da es sich bei einfachen Entitäten normalerweise um Namen handelt, z.B. Unternehmensnamen, Produktnamen oder andere Namen, sollten Sie wie folgt vorgehen: Fügen Sie eine [Liste mit Ausdrücken](luis-concept-feature.md) hinzu, wenn Sie eine einfache Entität verwenden, um das Signal für die verwendeten Namen zu verstärken. 

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Daten sind nicht einheitlich formatiert, aber weisen auf denselben Sachverhalt hin. 

![Entität vom Typ „Simple“](./media/luis-concept-entities/simple-entity.png)

[Tutorial](luis-quickstart-primary-and-secondary-data.md)<br/>
[Beispielantwort für Entität](luis-concept-data-extraction.md#simple-entity-data)<br/>

## <a name="entity-limits"></a>Grenzwerte für Entitäten

Informieren Sie sich über die [Grenzwerte](luis-boundaries.md#model-boundaries), um zu erfahren, wie viele Entitäten jedes Typs Sie einem Modell hinzufügen können.

## <a name="composite-vs-hierarchical-entities"></a>Hierarchische und zusammengesetzte Entitäten

Zusammengesetzte Entitäten und hierarchische Entitäten verfügen jeweils über eine Über- und Unterordnungsbeziehung, und sie wurden durch maschinelles Lernen erworben. Durch maschinelles Lernen kann LUIS die Entitäten basierend auf unterschiedlichen Kontexten (Anordnung von Wörtern) verstehen. Zusammengesetzte Entitäten sind flexibler, da sie unterschiedliche Entitätstypen als untergeordnete Elemente zulassen. Bei einer hierarchischen Entität sind die untergeordneten Entitäten immer einfache Entitäten. 

|Type|Zweck|Beispiel|
|--|--|--|
|Hierarchical|Über-/untergeordnete Entität einfacher Entitäten|Location.Origin=New York<br>Location.Destination=London|
|Composite|Über-/untergeordnete Entitäten: vordefinierte, Listen-, einfache, hierarchische Entitäten| number=3<br>list=first class<br>prebuilt.datetimeV2=March 5|

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Wenn mehr als die maximale Anzahl von Entitäten erforderlich ist 

Möglicherweise müssen Sie hierarchische und zusammengesetzte Entitäten verwenden. Hierarchische Entitäten spiegeln die Beziehung zwischen Entitäten wider, die gemeinsame Merkmale aufweisen oder derselben Kategorie angehören. Die untergeordneten Entitäten sind alle Mitglieder der Kategorie der übergeordneten Entität. Beispielsweise kann die hierarchische Entität PlaneTicketClass die untergeordneten Entitäten EconomyClass und FirstClass enthalten. Die Hierarchie umfasst nur eine untergeordnete Ebene.  

Zusammengesetzte Entitäten stellen Teile eines Ganzen dar. Die zusammengesetzte Entität PlaneTicketOrder könnte z.B. die untergeordneten Entitäten Airline, Destination, DepartureCity, DepartureDate und PlaneTicketClass enthalten. Sie erstellen eine zusammengesetzte Entität aus bereits vorhandenen einfachen Entitäten, untergeordnete Entitäten von hierarchischen Entitäten oder vordefinierten Entitäten.  

LUIS verfügt auch über den Listenentitätstyp, der nicht per maschinellem Lernen erworben wird, sondern es Ihrer LUIS-App ermöglicht, eine feste Liste mit Werten anzugeben. In der Referenz zu [LUIS-Grenzen](luis-boundaries.md) finden Sie Informationen zu den Grenzwerten des Listenentitätstyps. 

Wenn Sie bereits hierarchische, zusammengesetzte und Listenentitäten berücksichtigt haben und trotzdem mehr als den Grenzwert benötigen, wenden Sie sich an den Support. Sammeln Sie dazu ausführliche Informationen über Ihr System, navigieren Sie zur [LUIS](luis-reference-regions.md#luis-website)-Website, und wählen Sie dann **Support** aus. Wenn Ihr Azure-Abonnement Supportdienste umfasst, wenden Sie sich an den [technischen Support von Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte für gute [Äußerungen](luis-concept-utterance.md). 

Weitere Informationen zum Hinzufügen von Entitäten zu LUIS-Apps finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).
