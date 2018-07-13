---
title: Grundlegendes zum Vorhersageergebnis von LUIS – Azure | Microsoft-Dokumentation
description: Erfahren Sie, was das Vorhersageergebnis in LUIS bedeutet.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 31c101a23892df8599b8cdc0f67647fefb969490
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265987"
---
# <a name="prediction-score"></a>Vorhersageergebnis
Ein Vorhersageergebnis gibt den Grad der Zuverlässigkeit an, den LUIS in die Ergebnisse von Vorhersagen hat. 

Ein Vorhersageergebnis liegt in der Regel zwischen 0 (null) und 1 (eins). Ein Beispiel für eine hohe Zuverlässigkeitsbewertung von LUIS ist der Wert 0,99. Ein Beispiel für eine Bewertung mit niedriger Zuverlässigkeit ist 0,01. 

|Ergebniswert|Zuverlässigkeit|
|--|--|
|1|definitive Übereinstimmung|
|0,99|hohe Zuverlässigkeit|
|0.01|niedrige Zuverlässigkeit|
|0|definitiv keine Übereinstimmung|

Wenn eine Äußerung eine niedrige Zuverlässigkeitsbewertung liefert, hebt LUIS es auf der [LUIS][LUIS]-Website auf der Seite **Absicht** hervor und kennzeichnet die identifizierte **bezeichnete Absicht** rot. 

![Abweichungen bei der Bewertung](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Absicht mit der höchsten Bewertung
Zu jeder Vorhersage einer Äußerung wird die am höchsten bewertete Absicht zurückgegeben. Dies ist ein numerischer Vergleich der Vorhersageergebnisse. Zwischen den beiden höchsten Bewertungen kann nur ein sehr kleiner Unterschied bestehen. LUIS zeigt diesen geringen Unterschied nicht an, sondern gibt lediglich die Ergebnisse zurück.  

Wenn die Nähe der höchsten Bewertungen ein Problem darstellen könnte, sollten Sie das Ergebnis für alle Absichten zurückgeben. Sie können entweder Äußerungen für die beiden Absichten hinzufügen, um deren Unterschiede bei der Wortwahl und -anordnung hervorzuheben, oder Sie können in der App, die LUIS aufruft, also z.B. einem Chatbot, programmgesteuert angeben, wie die beiden am höchsten bewerteten Absichten zu behandeln sind. 

## <a name="return-prediction-score-for-all-intents"></a>Zurückgeben der Vorhersageergebnisse für alle Absichten
Ein Test- oder Endpunktergebnis kann alle Absichten enthalten. Diese Konfiguration wird am [Endpunkt](https://aka.ms/v1-endpoint-api-docs) mit dem Name-Wert-Paar `verbose=true` in der Abfragezeichenfolge festgelegt. 

## <a name="review-intents-with-similar-scores"></a>Überprüfen von Absichten mit ähnlichen Bewertungen
Durch das Überprüfen der Ergebnisse aller Absichten können Sie sicherstellen, dass nicht nur die richtige Absicht identifiziert wird, sondern auch die Bewertung der nächstniedrigeren Absicht bei allen Äußerungen deutlich niedriger ist. 

Wenn mehrere Absichten ähnliche Vorhersagebewertungen haben, kann LUIS basierend auf dem Kontext einer Äußerung möglicherweise zwischen diesen Absichten wechseln. Um dieses Problem zu beheben, fügen Sie jeder Absicht weitere Äußerungen mit einer größeren Vielfalt an kontextabhängigen Unterschieden hinzu.   

## <a name="e-exponent-notation"></a>E-Notation (Exponent)

Vorhersagebewertungen können exponentiell angegeben werden, um außerhalb des Intervalls von 0 bis 1 *angezeigt* zu werden, z.B. `9.910309E-07`. Dieses Ergebnis ist ein Hinweis auf eine sehr **kleine** Zahl.

|Bewertung in E-Notation |Tatsächliche Bewertung|
|--|--|
|9,910309E-07|0,0000009910309|

## <a name="differences-with-predictions"></a>Unterschiede zwischen Vorhersagen
Wenn Sie dasselbe Modell in einer anderen App trainieren und die Ergebnisse abweichen, enthält das Training ein Zufallselement. Darüber hinaus bedeutet eine Überlappung einer Äußerung über mehrere Absichten, dass die am höchsten bewertete Absicht für dieselbe Äußerung wechseln kann.

Wenn Ihr Chatbot eine bestimmte LUIS-Bewertung erfordert, um die Zuverlässigkeit einer Absicht anzuzeigen, sollten Sie stattdessen den Unterschied zwischen den Ergebnissen der beiden am höchsten bewerteten Absichten verwenden. Dies bietet Flexibilität für Varianten im Training. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Hinzufügen von Entitäten zu LUIS-Apps finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions