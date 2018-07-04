---
title: Informationen zu Language Understanding (LUIS) in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Language Understanding (LUIS) die Leistungsfähigkeit von Machine Learning in Ihren Anwendungen nutzen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: bbd0a532e54f9b221739c8ae9ff097fe44fdc4df
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751594"
---
# <a name="what-is-language-understanding-luis"></a>Worum handelt es sich bei Language Understanding (LUIS)?
Language Understanding (LUIS) ist ein cloudbasierter Dienst, der benutzerdefiniertes Machine Learning auf natürliche Konversationssprachtexte eines Benutzers anwendet, um die allgemeine Bedeutung vorherzusagen sowie relevante und detaillierte Informationen abzurufen. 

Bei einer Clientanwendung für LUIS kann es sich um eine beliebige Konversationsanwendung handeln, die zum Durchführen einer Aufgabe mit einem Benutzer in natürlicher Sprache kommuniziert. Beispiele für Clientanwendungen sind Social Media Apps, Chatbots und sprachfähige Desktopanwendungen.  

![Konzeptionelle Darstellung der Informationseingabe aus 3 Anwendungen in LUIS](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>Was ist eine LUIS-App?
Eine LUIS-App enthält ein von Ihnen entworfenes, domänenspezifisches Modell für natürliche Sprache. Sie können Ihre LUIS-App mit einem vordefinierten Domänenmodell beginnen, ein eigenes Modell erstellen oder Teile einer vordefinierten Domäne mit Ihren eigenen benutzerdefinierten Informationen mischen.

[Vordefinierte Domänenmodelle](luis-how-to-use-prebuilt-domains.md) enthalten alle erforderlichen Teile und bieten eine hervorragende Möglichkeit für ein schnelle Verwendung von LUIS.

Die LUIS-App enthält auch Integrationseinstellungen, [Mitarbeiter](luis-concept-collaborator.md) und [Versionen](luis-concept-version.md).

## <a name="using-a-luis-app"></a>Verwenden einer LUIS-App
<a name="Accessing-LUIS"></a> Nachdem Ihre LUIS-App veröffentlicht wurde, sendet die Clientanwendung Äußerungen an die [LUIS-Endpunkt-API][endpoint-apis] und empfängt die Vorhersageergebnisse als JSON-Antworten.

Im folgenden Diagramm sendet der Client-Chatbot zuerst Benutzertext zu den Wünschen einer Person in deren eigenen Worten in einer HTTP-Anforderung an LUIS. Im zweiten Schritt wendet LUIS Ihr Lernmodell auf die natürliche Sprache an, damit die Benutzereingabe einen Sinn ergibt, und gibt dann eine Antwort im JSON-Format (JavaScript Object Notation) zurück. Als Drittes verwendet der Client-Chatbot die JSON-Antwort, um die Anforderungen des Benutzers zu erfüllen. 

![Konzeptionelle Darstellung der Arbeitsweise von LUIS mit Chatbot](./media/luis-overview/luis-overview-process-2.png)

### <a name="example-of-json-endpoint-response"></a>Beispiel einer JSON-Endpunktantwort

Die JSON-Endpunktantwort enthält mindestens die Abfrageäußerung und die Absicht mit der höchsten Bewertung. 

```JSON
{
  "query": "I want to call my HR rep.",
  "topScoringIntent": {
    "intent": "HRContact",
    "score": 0.921233
  },
  "entities": [
    {
      "entity": "call",
      "type": "Contact Type",
      "startIndex": 10,
      "endIndex": 13,
      "score": 0.7615982
    }
  ]
}
```

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>
## <a name="what-is-a-natural-language-model"></a>Was ist ein Modell für natürliche Sprache?
Ein Modell beginnt mit einer Liste allgemeiner _Absichten_ eines Benutzers, z.B. „Flug buchen“ oder „Helpdesk kontaktieren“. Sie geben Beispieltexte des Benutzers ein, die als _Beispieläußerungen_ für die Absichten bezeichnet werden. Dann markieren Sie wichtige Wörter oder Ausdrücke in der Äußerung. Diese werden als _Entitäten_ bezeichnet.


Ein Modell umfasst Folgendes:

* **[Absichten](#intents)**: Kategorien von Benutzerabsichten (beabsichtigte Aktion oder Ergebnis)
* **[Entitäten](#entities)**: bestimmte Datentypen in Äußerungen, z.B. Nummer, E-Mail oder Name
* **[Beispieläußerungen](#example-utterances)**: Beispieltext, den ein Benutzer in der Clientanwendung eingibt

### <a name="intents"></a>Absichten 
Eine [Absicht](luis-how-to-add-intents.md) ist_ein vom Benutzer_ in seiner Äußerung genannter Zweck oder ein Ziel, wie das Buchen eines Flugs, das Bezahlen einer Rechnung oder das Suchen nach einem Nachrichtenartikel. Sie erstellen eine Absicht für jede Aktion. In einer LUIS-Reise-App kann eine Absicht mit dem Namen „FlugBuchen“ definiert sein. Ihre Clientanwendung kann die Absicht mit der höchsten Bewertung zum Auslösen einer Aktion verwenden. Wenn beispielsweise die Absicht „FlugBuchen“ von LUIS zurückgegeben wird, kann die Clientanwendung einen API-Aufruf an einen externen Dienst zum Buchen eines Flugtickets auslösen.

### <a name="entities"></a>Entitäten
Eine [Entität](luis-how-to-add-entities.md) stellt detaillierte Informationen dar, die in der Äußerung gefunden werden und für die Anforderung des Benutzers relevant sind. Beispielsweise wird bei der Äußerung „Ein Ticket nach Paris buchen“ ein einzelnes Ticket angefordert, und „Paris“ ist ein Ort. Es werden zwei Entitäten gefunden: „ein Ticket“ gibt ein einzelnes Ticket an, und „Paris“ gibt das Ziel an. 

Nachdem LUIS die in der Äußerung des Benutzers gefundenen Entitäten zurückgegeben hat, kann die Clientanwendung die Liste der Entitäten als Parameter zum Auslösen einer Aktion verwenden. Für das Buchen eines Flugs sind beispielsweise Entitäten wie das Reiseziel, das Datum und die Fluggesellschaft erforderlich.

LUIS bietet verschiedene Methoden zum Identifizieren und Kategorisieren von Entitäten.

* **Vordefinierte Entitäten**: LUIS verfügt über eine Vielzahl vordefinierter Domänenmodelle, einschließlich Absichten, Äußerungen und [vordefinierter Entitäten](pre-builtentities.md). Sie können die vordefinierten Entitäten nutzen, ohne die Absichten und Äußerungen des vordefinierten Modells verwenden zu müssen. Die vordefinierten Entitäten ersparen Ihnen Zeit.

* **Benutzerdefinierte Entitäten**: LUIS bietet Ihnen mehrere Möglichkeiten zum Festlegen eigener benutzerdefinierter [Entitäten](luis-concept-entity-types.md), einschließlich auf maschinellem Lernen basierender Entitäten, bestimmter oder literaler Entitäten sowie einer Kombination von literalen und auf maschinellem Lernen basierenden Entitäten.

### <a name="example-utterances"></a>Beispiele für Äußerungen
Eine [Beispieläußerung](luis-how-to-add-example-utterances.md) ist eine Texteingabe des Benutzers, die von der Clientanwendung verstanden werden muss. Dabei kann es sich um einen Satz (z.B. „Ein Ticket nach Paris buchen“) oder einen Teil eines Satzes (z.B. „Buchen“ oder „Flug nach Paris“) handeln. Äußerungen sind nicht immer wohlgeformt, und es kann viele verschiedene Äußerungen für eine bestimmte Absicht geben. Fügen Sie 10 bis 20 Beispieläußerungen für jede Absicht hinzu, und markieren Sie die Entitäten in jeder Äußerung.

|Beispieläußerung eines Benutzers|Absicht|Entitäten|
|-----------|-----------|-----------|
|„Einen Flug nach __Seattle__ buchen?“|FlugBuchen|Seattle|
|„Wann __öffnet__ Ihr Geschäft?“|GeschäftszeitenUndStandorte|öffnet|
|„Eine Besprechung mit __Robert__ im Vertrieb um __13 Uhr__ planen“|BesprechungPlanen|13 Uhr, Robert|

## <a name="improve-prediction-accuracy"></a>Verbessern der Vorhersagegenauigkeit
Sobald die LUIS-App veröffentlicht wurde und echte Benutzeräußerungen empfängt, bietet LUIS mehrere Methoden zum Verbessern der Vorhersagegenauigkeit: [Aktives Lernen](#active-learning) von Endpunktäußerungen, [Begriffslisten](#phrase-lists) für die Einbeziehung von Domänenwörtern und [Muster](#patterns) zum Reduzieren der Anzahl erforderlicher Äußerungen.

### <a name="active-learning"></a>Aktives Lernen
Beim Prozess des [aktiven Lernens](label-suggested-utterances.md) ermöglicht Ihnen LUIS die Anpassung Ihrer LUIS-App an reale Äußerungen, indem Äußerungen, die am Endpunkt empfangen wurden, für Sie zur Überprüfung ausgewählt werden. Sie können die Endpunktvorhersage akzeptieren oder korrigieren, neu trainieren und erneut veröffentlichen. Dieser iterative Prozess ermöglicht LUIS ein schnelles Lernen und bedeutet für Sie minimalen Zeit- und Arbeitsaufwand. 

### <a name="phrase-lists"></a>Begriffslisten 
LUIS enthält [Begriffslisten](luis-concept-feature.md), mit denen Sie wichtige Wörter oder Ausdrücke für Ihre Modelldomäne angeben können. LUIS verwendet diese Listen, um den Wörtern und Ausdrücken, die andernfalls nicht im Modell gefunden würden, eine größere Bedeutung beizumessen.

### <a name="patterns"></a>Muster 
Mithilfe von Mustern können Sie die Sammlung von Äußerungen für eine Absicht in Form allgemeiner [Vorlagen](luis-concept-patterns.md) für Wortwahl und Wortfolge vereinfachen. Dies ermöglicht LUIS ein schnelleres Lernen, da weniger Beispieläußerungen für die Absichten benötigt werden. Muster sind ein Hybridsystem von regulären Ausdrücken und auf maschinellem Lernen basierenden Ausdrücken. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Erstellen und Zugreifen auf LUIS
Erstellen Sie Ihre LUIS-App über die LUIS-Website oder programmgesteuert mit den [Erstellungs](https://aka.ms/luis-authoring-apis)-APIs, oder verwenden Sie je nach Erstellungsanforderung beide Methoden. Greifen Sie über den [Endpunkt](https://aka.ms/luis-endpoint-apis) der Abfrage auf Ihre veröffentlichte LUIS-App zu. 

LUIS stellt je nach Erstellungsregion drei Websites weltweit bereit. Die Erstellungsregion bestimmt die Azure-Region, in der Sie Ihre LUIS-App veröffentlichen können.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Erfahren Sie [mehr](luis-reference-regions.md) über Erstellungs- und Veröffentlichungsregionen.

## <a name="what-technologies-work-with-luis"></a>Welche Technologien können mit LUIS verwendet werden?
Mehrere Microsoft-Technologien können mit LUIS verwendet werden:

* [Bing-Rechtschreibprüfungs-API](../bing-spell-check/proof-text.md) bietet Textkorrektur vor der Vorhersage. 
* [Bot Framework][bot-framework] ermöglicht einem Chatbot die Kommunikation mit einem Benutzer per Texteingabe. Wählen Sie [3.x](https://github.com/Microsoft/BotBuilder) oder [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK aus, um eine vollständige Botoberfläche zu erhalten.
* [QnA Maker][qnamaker] ermöglicht die Kombination mehrerer Texttypen in einer Wissensdatenbank basierend auf Fragen und Antworten.
* [Spracheingabe](../Speech/home.md) konvertiert Anforderungen in gesprochener Sprache in Text. Nach der Konvertierung in Text verarbeitet LUIS die Anforderungen. Weitere Informationen finden Sie unter [Sprach-SDK](https://aka.ms/csspeech).
* [Textanalyse](../text-analytics/overview.md) bietet eine Standpunktanalyse und Schlüsselbegriffserkennung.

## <a name="next-steps"></a>Nächste Schritte
Erstellen Sie eine neue LUIS-App mit einer [vordefinierten](luis-get-started-create-app.md) oder [benutzerdefinierten](luis-quickstart-intents-only.md) Domäne.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/