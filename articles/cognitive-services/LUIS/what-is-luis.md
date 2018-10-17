---
title: Was ist Language Understanding (LUIS) – Azure Cognitive Services | Microsoft-Dokumentation
description: Language Understanding (LUIS) ist ein cloudbasierter API-Dienst, der benutzerdefinierte Machine Learning-Intelligenz auf natürliche Konversationssprachtexte eines Benutzers anwendet, um die allgemeine Bedeutung vorherzusagen sowie relevante und detaillierte Informationen abzurufen. Bei einer Clientanwendung für LUIS handelt es sich um eine beliebige Konversationsanwendung, die zum Durchführen einer Aufgabe mit einem Benutzer in natürlicher Sprache kommuniziert. Beispiele für Clientanwendungen sind Social Media Apps, Chatbots und sprachfähige Desktopanwendungen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: overview
ms.date: 10/06/2018
ms.author: diberry
ms.openlocfilehash: 0520c00ab20ca7210b3bb13567f9998e7231be43
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867665"
---
# <a name="what-is-language-understanding-luis"></a>Worum handelt es sich bei Language Understanding (LUIS)?

Language Understanding (LUIS) ist ein cloudbasierter API-Dienst, der benutzerdefinierte Machine Learning-Intelligenz auf natürliche Konversationssprachtexte eines Benutzers anwendet, um die allgemeine Bedeutung vorherzusagen sowie relevante und detaillierte Informationen abzurufen. 

Bei einer Clientanwendung für LUIS handelt es sich um eine beliebige Konversationsanwendung, die zum Durchführen einer Aufgabe mit einem Benutzer in natürlicher Sprache kommuniziert. Beispiele für Clientanwendungen sind Social Media Apps, Chatbots und sprachfähige Desktopanwendungen.  

![Schematische Darstellung von 3 Clientanwendungen, die mit Cognitive Services Language Understanding (LUIS) arbeiten](./media/luis-overview/luis-entry-point.png "Schematische Darstellung von 3 Clientanwendungen, die mit Cognitive Services Language Understanding (LUIS) arbeiten")

## <a name="use-luis-in-a-chat-bot"></a>Verwendung von LUIS in einen Chatbot

<a name="Accessing-LUIS"></a>

Sobald die LUIS-App veröffentlicht ist, sendet eine Clientanwendung Äußerungen (Text) an die LUIS-Endpunkt-[API][endpoint-apis] für die Verarbeitung von natürlicher Sprache und empfängt die Ergebnisse als JSON-Antworten. Eine gängige Clientanwendung für LUIS ist ein Chatbot.


![Schematische Darstellung von LUIS in Verwendung mit einem Chatbot zum Vorhersagen von Benutzertext mit Verstehen natürlicher Sprache (Natural Language Understanding, NLP)](./media/luis-overview/luis-overview-process-2.png "Schematische Darstellung von LUIS in Verwendung mit einem Chatbot zum Vorhersagen von Benutzertext mit Verstehen natürlicher Sprache (Natural Language Understanding, NLP)")

|Schritt|Aktion|
|:--|:--|
|1|Die Clientanwendung sendet die _Benutzeräußerung_ (Text in den Worten des Benutzers) „Ich möchte meinen Personalverantwortlichen anrufen“ als HTTP-Anforderung an den LUIS-Endpunkt.|
|2|LUIS wendet das erlernte Modell auf den Text in natürlicher Sprache an, um ein intelligentes Verständnis der Benutzereingabe bereitzustellen. LUIS gibt eine Antwort in JSON-Formatierung mit der am höchsten eingestuften Absicht „HRContact“ zurück. Die JSON-Endpunktantwort enthält mindestens die Abfrageäußerung und die Absicht mit der höchsten Bewertung. Sie kann auch Daten wie die Entität „Kontakttyp“ extrahieren.|
|3|Die Clientanwendung verwendet die JSON-Antwort für Entscheidungen darüber, wie die Anforderungen des Benutzer erfüllt werden. Diese Entscheidungen können eine Entscheidungsstruktur im Frameworkcode des Bots und Aufrufe von anderen Diensten umfassen. |

Die LUIS-App stellt Intelligenz bereit, damit die Clientanwendung intelligente Entscheidungen treffen kann. Die Auswahlmöglichkeiten werden nicht von LUIS bereitgestellt. 

<!--

### Example of JSON endpoint response

The minimum JSON endpoint response contains the query utterance, and the top scoring intent. It can also extract data such as the following **Contact Type** entity. 

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
-->

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Verarbeitung natürlicher Sprache

Eine LUIS-App enthält ein domänenspezifisches Modell für natürliche Sprache. Sie können die LUIS-App mit einem vordefinierten Domänenmodell beginnen, ein eigenes Modell erstellen oder Teile einer vordefinierten Domäne mit Ihren eigenen benutzerdefinierten Informationen mischen.

* **Vordefiniertes Modell**: LUIS verfügt über eine Vielzahl vordefinierter Domänenmodelle, einschließlich Absichten, Äußerungen und vordefinierter Entitäten. Sie können die vordefinierten Entitäten nutzen, ohne die Absichten und Äußerungen des vordefinierten Modells verwenden zu müssen. [Vordefinierte Domänenmodelle](luis-how-to-use-prebuilt-domains.md) enthalten den gesamten Entwurf und bieten eine hervorragende Möglichkeit für ein schnelle Verwendung von LUIS.

* **Benutzerdefinierte Entitäten**: LUIS bietet Ihnen mehrere Möglichkeiten zum Festlegen eigener benutzerdefinierter Absichten und Entitäten, einschließlich auf maschinellem Lernen basierender Entitäten, bestimmter oder literaler Entitäten sowie einer Kombination von literalen und auf maschinellem Lernen basierenden Entitäten.

## <a name="build-the-luis-model"></a>Erstellen des LUIS-Modells
Erstellen Sie das Modell mit den [Dokumenterstellungs](https://aka.ms/luis-authoring-apis)-APIs oder über das LUIS-Portal.

Das LUIS-Modell beginnt mit Kategorien von Benutzerabsichten, die als **[Absichten](luis-concept-intent.md)** bezeichnet werden. Jede Absicht erfordert Beispiele für **[Äußerungen](luis-concept-utterance.md)** von Benutzern. Jede Äußerung kann eine Vielzahl von Daten enthalten, die mit **[Entitäten](luis-concept-entity-types.md)** extrahiert werden müssen. 

|Beispieläußerung eines Benutzers|Absicht|Entitäten|
|-----------|-----------|-----------|
|„Einen Flug nach __Seattle__ buchen?“|FlugBuchen|Seattle|
|„Wann __öffnet__ Ihr Geschäft?“|GeschäftszeitenUndStandorte|öffnet|
|„Eine Besprechung mit __Robert__ im Vertrieb um __13 Uhr__ planen“|BesprechungPlanen|13 Uhr, Robert|

<!--
## What is a natural language model?

A model begins with a list of general user intentions, called _intents_, such as "Book Flight" or "Contact Help Desk." You provide user's example text, called _example utterances_ for the intents. Then mark significant words or phrases in the utterance, called _entities_.


A model includes:

* **[intents](#intents)**: categories of user intentions (overall intended action or result)
* **[entities](#entities)**: specific types of data in utterances such as number, email, or name contained in text
* **[example utterances](#example-utterances)**: example text a user might enter in the client application

### Intents 

An [intent](luis-how-to-add-intents.md), short for _intention_, is a purpose or goal expressed in a user's utterance, such as booking a flight, paying a bill, or finding a news article. You create an intent for each action. A LUIS travel app may define an intent named "BookFlight." Each prediction query includes the top scored intent. 

The client application can use the top scoring intent to trigger an action. For example, when "BookFlight" intent is returned from LUIS, a client application could trigger an API call to an external service for booking a plane ticket.

### Entities

An [entity](luis-how-to-add-entities.md) represents detailed information found within the utterance that is relevant to the user's request. For example, in the utterance "Book a ticket to Paris",  a single ticket is requested, and "Paris" is a location. Two entities are found "a ticket" indicating a single ticket and "Paris" indicating the destination. 

After LUIS returns the entities found in the user’s utterance, the client application can use the list of entities as parameters to trigger an action. For example, booking a flight requires entities like the travel destination, date, and airline.
-->
<!--
### Example utterances

An example [utterance](luis-how-to-add-example-utterances.md) is text input from the user that the client application needs to understand. It may be a sentence, like "Book a ticket to Paris", or a fragment of a sentence, like "Booking" or "Paris flight." Utterances aren't always well-formed, and there can be many utterance variations for a particular intent. Add 10 to 20 example utterances to each intent and mark entities in every utterance.

|Example user utterance|Intent|Entities|
|-----------|-----------|-----------|
|"Book a flight to __Seattle__?"|BookFlight|Seattle|
|"When does your store __open__?"|StoreHoursAndLocation|open|
|"Schedule a meeting at __1pm__ with __Bob__ in Distribution"|ScheduleMeeting|1pm, Bob|
-->
## <a name="query-prediction-endpoint"></a>Abfragen eines Vorhersageendpunkts

Nachdem das Modell erstellt und am Endpunkt veröffentlicht wurde, sendet die Clientanwendung Äußerungen an die API des veröffentlichten [Vorhersageendpunkts](https://aka.ms/luis-endpoint-apis). Die API wendet das Modell zur Analyse auf den Text an. Die API gibt die Vorhersageergebnisse in einem JSON-Format zurück.  

Die JSON-Endpunktantwort enthält mindestens die Abfrageäußerung und die Absicht mit der höchsten Bewertung. Sie kann auch Daten wie die folgende **Kontakttyp**-Entität extrahieren. 

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

## <a name="improve-model-prediction"></a>Verbessern der Modellvorhersage

Sobald ein LUIS-Modell veröffentlicht wurde und echte Benutzeräußerungen empfängt, bietet LUIS mehrere Methoden zum Verbessern der Vorhersagegenauigkeit: [Aktives Lernen](luis-concept-review-endpoint-utterances.md) von Endpunktäußerungen, [Begriffslisten](luis-concept-feature.md) für die Einbeziehung von Domänenwörtern und [Muster](luis-concept-patterns.md) zum Reduzieren der Anzahl erforderlicher Äußerungen.
<!--
### Active learning

In the [active learning](luis-how-to-review-endoint-utt.md) process, LUIS allows you to adapt the LUIS app to real-world utterances by selecting utterances it received at the endpoint for your review. You can accept or correct the endpoint prediction, retrain, and republish. LUIS learns quickly with this iterative process, taking the minimum amount of your time and effort. 

### Phrase lists 

LUIS provides [phrases lists](luis-concept-feature.md) so you can indicate important words or phrases of the model. LUIS uses these lists to add additional significance to those words and phrases that would otherwise not be found in the model.

### Patterns 

Patterns allow you to simplify an intent's utterance collection into common [templates](luis-concept-patterns.md) of word choice and word order. This allows LUIS to learn quicker by needing fewer example utterances for the intents. Patterns are a hybrid system of regular expressions and machine-learned expressions. 
-->
<a name="using-luis"></a>
<!--
## Authoring and accessing models
Author LUIS from the [authoring](https://aka.ms/luis-authoring-apis) APIs or from the LUIS portal. Query the published prediction endpoint of the model from the [endpoint](https://aka.ms/luis-endpoint-apis) APIs.
-->

## <a name="development-lifecycle"></a>Lebenszyklus der Entwicklung
LUIS verfügt über Tools, Versionsverwaltung und Kollaboration mit anderen LUIS-Autoren, und diese Elemente werden auf Ebene der Clientanwendung und des Sprachmodells in den vollständigen Entwicklungslebenszyklus integriert. 

## <a name="implementing-luis"></a>Implementieren von LUIS
LUIS kann als REST-API mit allen Produkten, Diensten oder Frameworks eingesetzt werden, die HTTP-Anforderungen stellen. Die folgende Liste enthält die am häufigsten mit LUIS verwendeten Microsoft-Produkte und -Dienste.

Die am häufigsten verwendete Clientanwendung für LUIS ist:
* [Web-App-Bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-3.0) erstellt schnell einen LUIS-fähigen Chatbot für die Kommunikation mit einem Benutzer über die Texteingabe. Verwendet [Bot Framework][bot-framework] Version [3.x](https://github.com/Microsoft/BotBuilder) oder [4.x](https://github.com/Microsoft/botbuilder-dotnet) für eine vollständige Bot-Oberfläche.

Tools zum schnellen und einfachen Nutzen von LUIS mit einem Bot:
* [LUIS-CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) Mit dem NPM-Paket sind Erstellungen und Vorhersagen entweder mit einem eigenständigen Befehlszeilentool oder als Import möglich. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen ist ein Tool zum Generieren von stark typisiertem C#- und TypeScript-Quellcode aus einem exportierten LUIS-Modell.
* [Dispatch](https://aka.ms/dispatch-tool) ermöglicht die Verwendung mehrerer LUIS- und QnA Maker-Apps aus einer übergeordneten App mithilfe eines Verteilermodells.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown ist ein Befehlszeilentool, mit dem Sprachmodelle für Ihren Bot verwaltet werden können.

Weitere Cognitive Services, die mit LUIS verwendet werden:
* [QnA Maker][qnamaker] ermöglicht die Kombination mehrerer Texttypen in einer Wissensdatenbank basierend auf Fragen und Antworten.
* [Bing-Rechtschreibprüfungs-API](../bing-spell-check/proof-text.md) bietet Textkorrektur vor der Vorhersage. 
* Der [Spracherkennungsdienst](../Speech-Service/overview.md) konvertiert Anforderungen in gesprochener Sprache in Text. 
* Das [Unterhaltungslernmodul](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview) ermöglicht Ihnen mit LUIS ein schnelleres Erstellen von Bot-Konversationen.
* [Projekt: Chat mit Profil](https://docs.microsoft.com/azure/cognitive-services/project-personality-chat/overview) für Smalltalk mit dem Bot.
<!--
## Other ways of implementing LUIS

A client application for LUIS is:
* [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/) - learn more with this [Mixed reality course](https://docs.microsoft.com/windows/mixed-reality/mr-azure-303) with LUIS. 


Labs: 

-->
## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine neue LUIS-App mit einer [vordefinierten](luis-get-started-create-app.md) oder [benutzerdefinierten](luis-quickstart-intents-only.md) Domäne. [Fragen Sie den Endpunkt der Vorhersage](luis-get-started-cs-get-intent.md) einer öffentlichen IoT-App ab.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/