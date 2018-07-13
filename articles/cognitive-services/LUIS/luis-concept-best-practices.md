---
title: Grundlegendes zu bewährten Methoden bei LUIS – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit bewährten Methoden bei LUIS die besten Ergebnisse erzielen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: 6c4e4c7acb5083e758aa7c6b94e2464a85ae8e9e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951112"
---
# <a name="best-practices"></a>Bewährte Methoden
Verwenden Sie den App-Erstellungsprozess zur Erstellung der LUIS-App. 

* Erstellen des Sprachmodells
* Hinzufügen einiger Beispieläußerungen für das Training (10–15 pro Absicht)
* Veröffentlichen 
* Testen am Endpunkt 
* Hinzufügen von Features

Nachdem die App [veröffentlicht](luis-how-to-publish-app.md) wurde, können Sie über den Erstellungszyklus Features hinzufügen, die Veröffentlichung durchführen und Tests am Endpunkt durchführen. Beginnen Sie den nächsten Erstellungszyklus nicht durch das Hinzufügen weiterer Beispieläußerungen. Damit lernt LUIS Ihr Modell nicht durch reale Benutzeräußerungen kennen. 

Damit LUIS effizient lernen kann, erweitern Sie die Äußerungen erst, wenn der aktuelle Satz von Beispiel- und Endpunktäußerungen angemessene Zuverlässigkeits- und Vorhersagebewertungen aufweist. Verbessern Sie die Bewertungen durch aktives Lernen, [Muster](luis-concept-patterns.md) und [Ausdruckslisten](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Empfehlungen
Die folgende Liste enthält bewährte Methoden für LUIS-Apps:

|Empfohlene Vorgehensweise|Nicht empfohlene Vorgehensweise|
|--|--|
|[Definieren unterschiedlicher Absichten](#do-define-distinct-intents) |[Hinzufügen vieler Beispieläußerungen zu Absichten](#dont-add-many-example-utterances-to-intents) |
|[Anstreben einer Balance zwischen zu allgemeinen und zu spezifischen Absichten](#do-find-sweet-spot-for-intents)|[Verwenden von LUIS als Trainingsplattform](#dont-use-luis-as-a-training-platform)|
|[Iteratives Erstellen der App](#do-build-the-app-iteratively)|[Hinzufügen sehr vieler Beispieläußerungen im gleichen Format und Ignorieren anderer Formate](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Hinzufügen von Ausdruckslisten und Mustern in späteren Iterationen](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Kombinieren der Definition von Absichten und Entitäten](#dont-mix-the-definition-of-intents-and-entities)|
|[Hinzufügen von Beispieläußerungen zur Absicht „None“](#do-add-example-utterances-to-none-intent)|[Erstellen von Ausdruckslisten mit allen möglichen Werten](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Verwenden des Vorschlagfeatures für das aktive Lernen](#do-leverage-the-suggest-feature-for-active-learning)|[Hinzufügen sehr vieler Muster](#dont-add-many-patterns)|
|[Überwachen der Leistung Ihrer App](#do-monitor-the-performance-of-your-app)|[Trainieren und Veröffentlichen jeder einzelnen hinzugefügten Beispieläußerung](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Definieren Sie unterschiedliche Absichten
Stellen Sie sicher, dass sich das Vokabular der einzelnen Absichten nicht mit dem anderer Absichten überschneidet. Wenn Sie z.B. eine App planen, die Reisebuchungen wie Flüge oder Hotels verarbeiten soll, können Sie diese als separate Absichten oder als dieselbe Absicht mit separaten Entitäten für bestimmte Daten innerhalb der Äußerung planen.

Wenn das Vokabular bei zwei Absichten gleich ist, fassen Sie die Absichten zusammen, und verwenden Sie Entitäten. 

Erwägen Sie die folgenden Beispieläußerungen:

```
Book a flight
Book a hotel
```

„Buche einen Flug“ und „Buche ein Hotelzimmer“ verwenden das gleiche Vokabular „Buche“. Dies stellt eine Überlappung dar. Aus diesem Grund sollte nur eine Absicht verwendet werden. Extrahieren Sie die unterschiedlichen Wörter „einen Flug“ und „ein Hotelzimmer“ in Entitäten. 

## <a name="do-find-sweet-spot-for-intents"></a>Streben Sie eine Balance zwischen Absichten an
Verwenden Sie die Vorhersagedaten von LUIS, um zu bestimmen, ob die Absichten überlappen. Überlappende Absichten verwirren LUIS. Im Ergebnis ist die Absicht mit der höchsten Bewertung zu nah an einer anderen Absicht. Da LUIS nicht bei jedem Training genau denselben Pfad durch die Daten anwendet, kann eine überlappende Absicht beim Training den ersten oder zweiten Platz erreichen. Damit dieser Fall nicht eintritt, sollten Sie das Ergebnis der Äußerung für jede Absicht weiter voneinander trennen. Bei einer guten Unterscheidung der Absichten sollte das Ergebnis jedes Mal die erwartete bestbewertete Absicht sein. 
 
## <a name="do-build-the-app-iteratively"></a>Erstellen Sie die App iterativ
Behalten Sie einen separaten *blinden* Testsatz bei, der nicht für [Beispieläußerungen](luis-concept-utterance.md) oder Endpunktäußerungen verwendet wird. Optimieren Sie die App immer weiter mit Ihrem Testsatz. Passen Sie den Testsatz an die tatsächlichen Benutzeräußerungen an. Verwenden Sie diesen blinden Testsatz, um die einzelnen Iterationen zu bewerten. 

Entwickler sollten über drei Datensätze verfügen. Der erste enthält die Beispieläußerungen zum Erstellen des Modells. Der zweite dient zum Testen des Modells am Endpunkt. Der dritte enthält die blinden Testdaten für das [Testen in Batches](luis-how-to-batch-test.md). Der letzte Datensatz wird nicht zum Trainieren der Anwendung verwendet und auch nicht an den Endpunkt gesendet.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Fügen Sie Ausdruckslisten und Muster in späteren Iterationen hinzu
Mithilfe von [Ausdruckslisten](luis-concept-feature.md) können Sie Wörterbücher definieren, die Wörter im Zusammenhang mit Ihrer App-Domäne enthalten. Beginnen Sie die Ausdrucksliste mit wenigen Wörtern, und nutzen Sie dann das Vorschlagfeature, damit LUIS weitere Wörter im Vokabular kennt. Fügen Sie dem Vokabular nicht jedes Wort hinzu, da die Ausdrucksliste keine genaue Übereinstimmung darstellen soll. 

Äußerungen echter Benutzer des Endpunkts, die sich sehr stark ähneln, können auf Muster bei der Wortwahl und -anordnung hinweisen. Das [Musterfeature](luis-concept-patterns.md) übernimmt diese Wortwahl und -anordnung zusammen mit regulären Ausdrücken, um die Vorhersagegenauigkeit zu verbessern. Ein regulärer Ausdruck im Muster ermöglicht es Ihnen, Wörter und Satzzeichen zu ignorieren, während das Muster weiterhin übereinstimmt. 

Verwenden Sie die optionale Syntax des Musters für Satzzeichen, sodass diese ignoriert werden können.

Wenden Sie diese Methoden nicht an, bevor Ihre App Endpunktanforderungen empfangen hat, da sie die Zuverlässigkeit beeinflussen.  

## <a name="do-add-example-utterances-to-none-intent"></a>Fügen Sie der Absicht „None“ Beispieläußerungen hinzu
Dies ist die Fallbackabsicht, die alles außerhalb der Anwendung angibt. Fügen Sie der Absicht „None“ eine Beispieläußerung für jeweils 10 Beispieläußerungen im Rest der LUIS-App hinzu.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Verwenden Sie das Vorschlagfeature für das aktive Lernen
Verwenden Sie beim [aktiven Lernen](luis-how-to-review-endoint-utt.md) regelmäßig das **Überprüfen von Endpunktäußerungen**, anstatt Absichten zusätzliche Beispieläußerungen hinzuzufügen. Da die App ständig Endpunktäußerungen empfängt, nimmt diese Liste fortlaufend zu und ändert sich.

## <a name="do-monitor-the-performance-of-your-app"></a>Überwachen Sie die Leistung Ihrer App.
Überwachen Sie die Vorhersagegenauigkeit mithilfe eines Testsatzes. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Fügen Sie Absichten nur wenige Beispieläußerungen hinzu.
Nachdem die Anwendung veröffentlicht wurde, fügen Sie in nachfolgenden Iterationen nur Äußerungen aus dem aktiven Lernen hinzu. Wenn sich die Äußerungen zu ähnlich sind, fügen Sie ein Muster hinzu. 

## <a name="dont-use-luis-as-a-training-platform"></a>Verwenden Sie LUIS nicht als Trainingsplattform
LUIS ist für Sprachmodelle vorgesehen. Es ist nicht als allgemeine Trainingsplattform vorgesehen. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Fügen Sie nur wenige Beispieläußerungen im gleichen Format hinzu, und vernachlässigen Sie andere Format nicht
LUIS erwartet Variationen in den Äußerungen zu einer Absicht. Die Äußerungen können variieren, sollten aber eine einheitliche Bedeutung haben. Variationen können durch die Länge der Äußerung, die Wortwahl und die Wortanordnung entstehen. 

|Verwenden Sie nicht dasselbe Format.|Verwenden Sie unterschiedliche Formate.|
|--|--|
|Buy a ticket to Seattle<br>Buy a ticket to Paris<br>Buy a ticket to Orlando|Buy 1 ticket to Seattle<br>Reserve two seats on the red eye to Paris next Monday<br>I would like to book 3 tickets to Orlando for spring break|

In der zweiten Spalte werden verschiedene Verben (buy, reserve, book), verschiedene Mengenangaben (1, two, 3) und unterschiedliche Wortanordnungen verwendet, alle enthalten jedoch dieselbe Absicht: Flugtickets für eine Reise zu erwerben. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Trennen Sie die Definitionen von Absichten und Entitäten
Erstellen Sie eine Absicht für jede Aktion, die Ihr Bot akzeptieren soll. Verwenden Sie Entitäten als Parameter, die diese Aktionen möglich machen. 

Erstellen Sie für einen Chatbot, der Flüge buchen soll, die Absicht **BookFlight**. Sie sollten nicht für jede Fluggesellschaft oder jedes Ziel eine Absicht erstellen. Verwenden Sie diese Datenelemente als [Entitäten](luis-concept-entity-types.md), und kennzeichnen Sie sie in den Beispieläußerungen. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Erstellen Sie keine Ausdruckslisten mit allen möglichen Werten
Stellen Sie einige wenige Beispiele in den [Ausdruckslisten](luis-concept-feature.md) bereit, aber fügen Sie nicht jedes Wort hinzu. LUIS generalisiert und berücksichtigt dabei den Kontext. 

## <a name="dont-add-many-patterns"></a>Fügen Sie nicht viele Muster hinzu.
Fügen Sie nicht zu viele [Muster](luis-concept-patterns.md) hinzu. LUIS ist dafür vorgesehen, mit wenigen Beispielen schnell zu lernen. Überlasten Sie das System nicht unnötigerweise.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Führen Sie nicht für jede einzelne hinzugefügte Beispieläußerung Training und Veröffentlichung durch
Fügen Sie vor dem Training und der Veröffentlichung 10–15 Äußerungen hinzu. Damit können Sie die Auswirkungen auf die Vorhersagegenauigkeit überprüfen. Das Hinzufügen einer einzelnen Äußerung hat möglicherweise keine sichtbaren Auswirkung auf das Ergebnis. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie in der LUIS-App [Ihre App planen](luis-how-plan-your-app.md).