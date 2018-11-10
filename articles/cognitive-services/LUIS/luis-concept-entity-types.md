---
title: Entitätstypen in LUIS-Apps – Language Understanding
titleSuffix: Azure Cognitive Services
description: Hinzufügen von Entitäten (Schlüsseldaten in Ihrer Anwendungsdomäne) in LUIS-Apps (Language Understanding Intelligent Service)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: fdf81943a7bdbae80f4474915a72bb61f1123a30
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085854"
---
# <a name="entities-in-luis"></a>Entitäten in LUIS

Entitäten sind Wörter oder Ausdrücke in Äußerungen, die wichtige Daten in Ihrer Anwendungsdomäne darstellen.

## <a name="entity-compared-to-intent"></a>Entität im Vergleich zu Absichten
Die Entität stellt ein Wort oder einen Ausdruck innerhalb der Äußerung dar, die extrahiert werden soll. Eine Äußerung kann viele Entitäten oder auch überhaupt keine enthalten. Eine Entität stellt eine Klasse dar, einschließlich einer Sammlung ähnlicher Objekte (Orte, Dinge, Personen, Ereignisse oder Konzepte). Entitäten beschreiben Informationen mit Bezug zur Absicht. Manchmal sind sie auch für die Ausführung der Aufgabe Ihrer App wichtig. Eine App zur News-Suche kann beispielsweise Entitäten wie „Thema“, „Quelle“, „Schlüsselwort“ und „Veröffentlichungsdatum“ enthalten, die besonders wichtig für die Suche nach Nachrichten sind. In einer Reisebuchungs-App sind „Ort“, „Datum“, „Airline“, „Reiseklasse“ und „Tickets“ Schlüsselinformationen für die Flugbuchung (relevant für die Absicht „Flug buchen“).

Im Vergleich dazu stellt die Absicht die Vorhersage der gesamten Äußerung dar. 

## <a name="entities-represent-data"></a>Darstellen von Daten durch Entitäten
Entitäten sind Daten, die Sie aus der Äußerung pullen möchten. Dabei kann es sich um einen Namen, ein Datum, einen Produktnamen oder eine Wortgruppe handeln. 

|Äußerung|Entität|Daten|
|--|--|--|
|Kaufe 3 Tickets nach New York|Vordefinierte Anzahl<br>Location.Destination|3<br>New York|
|Kaufe ein Ticket von New York nach London am 5. März|Location.Origin<br>Location.Destination<br>Vordefinierte datetimeV2|New York<br>London<br>5. März 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entitäten sind optional, jedoch dringend empfohlen.
Im Gegensatz zu Absichten sind Entitäten optional. Sie müssen keine Entitäten für sämtliche Konzepte Ihrer App erstellen, sondern lediglich für die, die für Aktionen Ihrer App erforderlich sind. 

Wenn Ihre Äußerungen keine Details enthalten, die Ihr Bot für das Fortfahren benötigt, müssen Sie diese auch nicht hinzufügen. Sie können sie bei fortschreitender Entwicklung Ihrer App später hinzufügen. 

Wenn Sie nicht sicher sind, wie Sie die Informationen verwenden könnten, fügen Sie einige allgemeine vordefinierte Entitäten wie z.B. „datetimeV2“, „ordinal“, „email“ oder „phone number“ hinzu.

## <a name="label-for-word-meaning"></a>Bezeichnungen für Wortbedeutungen
Wenn die Wortauswahl oder die Anordnung der Wörter identisch ist, sie aber nicht dieselbe Bedeutung haben, sollten Sie sie nicht mit der Entität bezeichnen. 

In den folgenden Äußerungen ist das Wort `fair` ein Homograph. Die Schreibweise ist identisch, aber die Bedeutung ist eine andere:

|Äußerung|
|--|
|Welche Arten von Jahrmärkten finden in diesem Sommer in der Region Seattle statt?|
|Ist die aktuelle Bewertung für die Seattle-Rezension fair?|

Wenn Sie mit einer Veranstaltungsentität alle Veranstaltungsdaten suchen möchten, bezeichnen Sie das Wort `fair` in der ersten Äußerung, aber nicht in der zweiten.

## <a name="entities-are-shared-across-intents"></a>Wiederverwenden von Entitäten für mehrere Absichten
Entitäten werden für mehrere Absichten verwendet. Sie gehören nicht zu einer bestimmten Absicht. Absichten und Entitäten sind semantisch zugeordnet, diese Beziehung ist aber keinesfalls exklusiv.

In der Äußerung „Buche mir ein Ticket nach Paris“ ist „Paris“ eine Entität vom Typ „Ort“. Durch das Erkennen der Entitäten in der Benutzereingabe unterstützt Sie LUIS bei der Auswahl der spezifischen Aktionen, die für eine Absicht ausgeführt werden sollen.

## <a name="assign-entities-in-none-intent"></a>Zuweisen von Entitäten zur Absicht „Keine“
In allen Absichten, einschließlich der Absicht **None**, sollten Entitäten eine Bezeichnung erhalten. Auf diese Weise kann LUIS mehr darüber erfahren, wo sich die Entitäten in den Äußerungen befinden und welche Wörter sich um die Entitäten herum befinden. 

## <a name="entity-status-for-predictions"></a>Vorhersagen zum Entitätsstatus

Weitere Informationen finden Sie unter [Vorhersagen zum Entitätsstatus](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Typen von Entitäten
LUIS bietet viele Typen von Entitäten: vordefinierte Entitäten, benutzerdefinierte Entitäten, die durch maschinelles Lernen erworben wurden, und Listenentitäten.

| NAME | Bezeichnung möglich | BESCHREIBUNG |
| -- |--|--|
| **Vordefiniert** <br/>[Benutzerdefiniert](#prebuilt)| |  **Definition**<br>Integrierte Typen, die allgemeine Konzepte darstellen <br><br>**Liste**<br/>key phrase number, ordinal, temperature, dimension, money, age, percentage, email, URL, phone number und key phrase. <br><br>Vordefinierte Entitätsnamen sind reserviert. <br><br>Alle vordefinierten Entitäten, die der Anwendung hinzugefügt wurden, werden in der Abfrage an den [Endpunkt](luis-glossary.md#endpoint) zurückgegeben. Weitere Informationen finden Sie unter [Vordefinierte Entitäten](./luis-prebuilt-entities.md). <br/><br/>[Beispielantwort für Entität](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Regulärer Ausdruck**<br/>[RegEx](#regex)||**Definition**<br>Benutzerdefinierter regulärer Ausdruck für die formatierte Nur-Text-Äußerung. Die Groß-/Kleinschreibung sowie die Kultur werden ignoriert.  <br><br>Diese Entität eignet sich für Wörter oder Ausdrücke, die konsistent mit einer Variante formatiert sind, die ebenfalls einheitlich ist.<br><br>Reguläre Ausdrücke werden nach Änderungen durch die Rechtschreibprüfung angewandt. <br><br>Wenn der reguläre Ausdruck zu komplex ist (z.B. mit zu vielen Klammern), können Sie den Ausdruck dem Modell nicht hinzufügen. <br><br>**Beispiel**<br>`kb[0-9]{6,}` entspricht kb123456.<br/><br/>[Schnellstart](luis-quickstart-intents-regex-entity.md)<br>[Beispielantwort für Entität](luis-concept-data-extraction.md)|
| **Einfach** <br/>[durch maschinelles Lernen erworben](#machine-learned) | ✔ | **Definition**<br>Eine einfache Entität ist eine generische Entität, die ein einzelnes Konzept beschreibt und im Kontext des maschinellen Lernen erworben wurde. Der Kontext umfasst die Wortwahl, die Wortanordnung und die Länge der Äußerung.<br/><br/>Diese Entität eignet sich gut für Wörter oder Ausdrücke, die nicht konsistent formatiert sind, jedoch dasselbe angeben. <br/><br/>[Schnellstart](luis-quickstart-primary-and-secondary-data.md)<br/>[Beispielantwort für Entität](luis-concept-data-extraction.md#simple-entity-data)|  
| **Liste** <br/>[Genaue Übereinstimmung](#exact-match)|| **Definition**<br>Listenentitäten stellen einen festen, abgeschlossenen Satz verwandter Wörter zusammen mit ihren Synonymen in Ihrem System dar. <br><br>Jede Listenentität kann eine oder mehrere Formen aufweisen. Sie werden am besten für einen bekannten Satz von Variationen angewandt, um dasselbe Konzept darzustellen.<br/><br/>LUIS ermittelt keine zusätzlichen Werte für Listenentitäten. Suchen Sie mithilfe des Features **Empfehlen** nach Vorschlägen für neue Wörter basierend auf der aktuellen Liste.<br/><br>Wenn mehr als eine Listenentität mit demselben Wert vorhanden ist, wird in der Endpunktabfrage jede Entität zurückgegeben. <br/><br/>[Schnellstart](luis-quickstart-intent-and-list-entity.md)<br>[Beispielantwort für Entität](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.any** <br/>[Gemischt](#mixed) | ✔|**Definition**<br>Pattern.any ist ein Platzhalter variabler Länge, der nur in der Vorlagenäußerung eines Musters verwendet wird, um zu kennzeichnen, wo die Entität beginnt und endet.  <br><br>**Beispiel**<br>Wenn Sie eine Suche zu einer Äußerung für Bücher basierend auf dem Titel durchführen, extrahiert Pattern.any den vollständigen Titel. Eine Vorlagenäußerung mit Pattern.any ist `Who wrote {BookTitle}[?]`.<br/><br/>[Tutorial](luis-tutorial-pattern.md)<br>[Beispielantwort für Entität](luis-concept-data-extraction.md#composite-entity-data)|  
| **Composite** <br/>[durch maschinelles Lernen erworben](#machine-learned) | ✔|**Definition**<br>Eine zusammengesetzte Entität besteht aus anderen Entitäten, z.B. vordefinierten, einfachen, „Regulärer Ausdruck“-, Listen- und hierarchischen Entitäten. Die einzelnen Entitäten bilden zusammen die gesamte Entität. <br><br>**Beispiel**<br>Der zusammengesetzten Entität PlaneTicketOrder könnten z.B. die vordefinierte Entität `number` sowie die Entität `ToLocation` untergeordnet sein. <br/><br/>[Tutorial](luis-tutorial-composite-entity.md)<br>[Beispielantwort für Entität](luis-concept-data-extraction.md#composite-entity-data)|  
| **Hierarchical** <br/>[durch maschinelles Lernen erworben](#machine-learned) |✔ | **Definition**<br>Eine hierarchische Entität ist eine Kategorie von kontextbezogen erlernten einfachen Entitäten.<br><br>**Beispiel**<br>Bei der hierarchischen Entität `Location` mit den untergeordneten Entitäten `ToLocation` und `FromLocation` können die einzelnen untergeordnete Entitäten basierend auf dem **Kontext** innerhalb der Äußerung bestimmt werden. In der Äußerung `Book 2 tickets from Seattle to New York` sind `ToLocation` und `FromLocation` kontextbezogen aufgrund der umgebenden Wörter unterschiedlich. <br/><br/>**Nicht verwenden, wenn**<br>Wenn Sie eine Entität mit genauen Textübereinstimmungen für die untergeordneten Entitäten benötigen, und zwar unabhängig vom Kontext, dann sollten Sie eine Listenentität verwenden. Wenn Sie eine Klassifizierung mit anderen Entitätstypen suchen, sollten Sie eine zusammengesetzte Entität verwenden.<br/><br/>[Schnellstart](luis-quickstart-intent-and-hier-entity.md)<br>[Beispielantwort für Entität](luis-concept-data-extraction.md#hierarchical-entity-data)|

<a name="prebuilt"></a>
**Vordefinierte** Entitäten sind benutzerdefinierte Entitäten, die von LUIS bereitgestellt werden. Einige dieser Entitäten werden im Open-Source-Projekt [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) definiert. Im Verzeichnis „/Specs“ der unterstützten Kulturen finden Sie viele [Beispiele](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs). Wenn Ihre Kultur oder Entität derzeit nicht unterstützt wird, können Sie sich am Projekt beteiligen. 

<a name="machine-learned"></a>
**Durch maschinelles Lernen erworbene** Entitäten funktionieren am besten, wenn sie mithilfe von [Endpunktabfragen](luis-concept-test.md#endpoint-testing) und durch [Überprüfen der Endpunktäußerungen](luis-how-to-review-endoint-utt.md) getestet werden. 

<a name="regex"></a>
**RegEx-Entitäten** werden durch einen regulären Ausdruck definiert, den der Benutzer bei der Definition der Entität angibt. 

<a name="exact-match"></a>
**Genaue Übereinstimmungsentitäten** verwenden den in der Entität bereitgestellten Text für eine genaue Textübereinstimmung.

<a name="mixed"></a>
**Gemischte** Entitäten verwenden eine Kombination von Methoden für die Entitätserkennung.

## <a name="entity-limits"></a>Grenzwerte für Entitäten
Informieren Sie sich über die [Grenzwerte](luis-boundaries.md#model-boundaries), um zu erfahren, wie viele Entitäten jedes Typs Sie einem Modell hinzufügen können.

## <a name="roles-versus-hierarchical-entities"></a>Rollen und hierarchische Entitäten

Weitere Informationen finden Sie unter [Rollen und hierarchische Entitäten](luis-concept-roles.md#roles-versus-hierarchical-entities).

## <a name="composite-vs-hierarchical-entities"></a>Hierarchische und zusammengesetzte Entitäten
Zusammengesetzte Entitäten und hierarchische Entitäten verfügen jeweils über eine Über- und Unterordnungsbeziehung, und sie wurden durch maschinelles Lernen erworben. Durch maschinelles Lernen kann LUIS die Entitäten basierend auf unterschiedlichen Kontexten (Anordnung von Wörtern) verstehen. Zusammengesetzte Entitäten sind flexibler, da sie unterschiedliche Entitätstypen als untergeordnete Elemente zulassen. Bei einer hierarchischen Entität sind die untergeordneten Entitäten immer einfache Entitäten. 

|Typ|Zweck|Beispiel|
|--|--|--|
|Hierarchical|Über-/untergeordnete Entität einfacher Entitäten|Location.Origin=New York<br>Location.Destination=London|
|Composite|Über-/untergeordnete Entitäten: vordefinierte, Listen-, einfache, hierarchische Entitäten| number=3<br>list=first class<br>prebuilt.datetimeV2=March 5|

## <a name="data-matching-multiple-entities"></a>Datenabgleich bei mehreren Entitäten
Wenn ein Wort oder ein Ausdruck mit mehreren Entitäten übereinstimmt, wird bei der Endpunktabfrage jede Entität zurückgegeben. Wenn Sie die vordefinierte number-Entität und die vordefinierte datetimeV2-Entität hinzufügen und als Äußerung `create meeting on 2018/03/12 for lunch with wayne` haben, erkennt LUIS alle Entitäten und gibt als Teil der JSON-Endpunktantwort ein Array von Entitäten zurück: 

```JSON
{
  "query": "create meeting on 2018/03/12 for lunch with wayne",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.9333419
  },
  "entities": [
    {
      "entity": "2018/03/12",
      "type": "builtin.datetimeV2.date",
      "startIndex": 18,
      "endIndex": 27,
      "resolution": {
        "values": [
          {
            "timex": "2018-03-12",
            "type": "date",
            "value": "2018-03-12"
          }
        ]
      }
    },
    {
      "entity": "2018",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 21,
      "resolution": {
        "value": "2018"
      }
    },
    {
      "entity": "03/12",
      "type": "builtin.number",
      "startIndex": 23,
      "endIndex": 27,
      "resolution": {
        "value": "0.25"
      }
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Datenabgleich bei mehreren Listenentitäten
Wenn ein Wort oder ein Ausdruck mit mehreren Listenentitäten übereinstimmt, wird bei der Endpunktabfrage jede Listenentität zurückgegeben.

Wenn die App bei der Abfrage `when is the best time to go to red rock?` das Wort `red` in mehreren Listenentitäten enthält, erkennt LUIS alle Entitäten und gibt als Teil der JSON-Endpunktantwort ein Array von Entitäten zurück: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Wenn mehr als die maximale Anzahl von Entitäten erforderlich ist 

Möglicherweise müssen Sie hierarchische und zusammengesetzte Entitäten verwenden. Hierarchische Entitäten spiegeln die Beziehung zwischen Entitäten wider, die gemeinsame Merkmale aufweisen oder derselben Kategorie angehören. Die untergeordneten Entitäten sind alle Mitglieder der Kategorie der übergeordneten Entität. Beispielsweise kann die hierarchische Entität PlaneTicketClass die untergeordneten Entitäten EconomyClass und FirstClass enthalten. Die Hierarchie umfasst nur eine untergeordnete Ebene.  

Zusammengesetzte Entitäten stellen Teile eines Ganzen dar. Die zusammengesetzte Entität PlaneTicketOrder könnte z.B. die untergeordneten Entitäten Airline, Destination, DepartureCity, DepartureDate und PlaneTicketClass enthalten. Sie erstellen eine zusammengesetzte Entität aus bereits vorhandenen einfachen Entitäten, untergeordnete Entitäten von hierarchischen Entitäten oder vordefinierten Entitäten.  

LUIS bietet auch den Listenentitätstyp, der nicht per maschinellem Lernen erworben wird, sondern es Ihrer LUIS-App ermöglicht, eine feste Liste mit Werten anzugeben. In der Referenz zu [LUIS-Grenzen](luis-boundaries.md) finden Sie Informationen zu den Grenzwerten des Listenentitätstyps. 

Wenn Sie bereits hierarchische, zusammengesetzte und Listenentitäten berücksichtigt haben und trotzdem mehr als den Grenzwert benötigen, wenden Sie sich an den Support. Sammeln Sie dazu ausführliche Informationen über Ihr System, navigieren Sie zur [LUIS](luis-reference-regions.md#luis-website)-Website, und wählen Sie dann **Support** aus. Wenn Ihr Azure-Abonnement Supportdienste umfasst, wenden Sie sich an den [technischen Support von Azure](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>Bewährte Methoden

Erstellen Sie eine [Entität](luis-concept-entity-types.md), wenn die aufrufende Anwendung oder der Bot Parameter oder Daten aus der Äußerung benötigt, um eine Aktion auszuführen. Eine Entität ist ein Wort oder Ausdruck in der Äußerung, die Sie extrahieren möchten – z.B. als Parameter für eine Funktion. 

Damit Sie den richtigen Entitätstyp, der Ihrer Anwendung hinzugefügt werden soll, auswählen können, müssen Sie wissen, wie diese Daten vom Benutzer eingegeben werden. Jeder Entitätstyp wird mithilfe eines anderen Mechanismus erworben, z.B. durch maschinelles Lernen, eine abgeschlossene Liste oder einen regulären Ausdruck. Wenn Sie unsicher sind, beginnen Sie mit einer einfachen Entität, und bezeichnen Sie das Wort oder den Ausdruck, das bzw. der diese Daten in allen Äußerungen und über alle Absichten (einschließlich der Absicht „None“) darstellt.  

Überprüfen Sie die Endpunktäußerungen in regelmäßigen Abständen, um häufige Verwendungen zu ermitteln, bei denen eine Entität als regulärer Ausdruck identifiziert oder durch eine genaue Textübereinstimmung gefunden werden kann.  

Planen Sie im Rahmen der Überprüfung auch das Hinzufügen einer Ausdrucksliste, um LUIS über Wörter oder Ausdrücke zu benachrichtigen, die für Ihre Domäne signifikant, jedoch keine genauen Übereinstimmungen sind und für die LUIS einen hohen Zuverlässigkeitswerten aufweist.  

Weitere Informationen finden Sie unter [Best Practices](luis-concept-best-practices.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte für gute [Äußerungen](luis-concept-utterance.md). 

Weitere Informationen zum Hinzufügen von Entitäten zu LUIS-Apps finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).