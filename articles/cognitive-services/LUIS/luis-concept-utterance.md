---
title: Gute Beispieläußerungen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Äußerungen sind Eingaben vom Benutzer, die Ihre App interpretieren muss. Sammeln Sie Ausdrücke, die Benutzer möglicherweise eingeben. Fügen Sie Äußerungen ein, die dieselbe Bedeutung haben, aber andere Wortlängen oder einen anderen Satzbau aufweisen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 2fd3416824189007bfdbe55d30907d9cb56f87ca
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895121"
---
# <a name="understand-what-good-utterances-are-for-your-luis-app"></a>Grundlegendes zu geeigneten Äußerungen für Ihre LUIS-App

**Äußerungen** sind Eingaben vom Benutzer, die Ihre App interpretieren muss. Es ist wichtig, eine Vielzahl verschiedener Beispieläußerungen für jede Absicht zu erfassen, um LUIS für die Extrahierung von Absichten und Entitäten zu trainieren. Das aktive Lernen bzw. der Prozess des kontinuierlichen Trainierens neuer Äußerungen ist ein grundlegender Bestandteil der Intelligenz durch Machine Learning, die LUIS bereitstellt.

Sammeln Sie Äußerungen, die Benutzer möglicherweise eingeben. Schließen Sie Äußerungen mit ein, die zwar die gleiche Bedeutung haben, aber unterschiedlich konstruiert sind:

* Äußerungslänge (kurz, mittel und lang für Ihre Clientanwendung)
* Wort- und Ausdruckslänge 
* Wortposition (Entität am Anfang, in der Mitte und am Ende der Äußerung)
* Grammatik 
* Pluralisierung
* Wortstammerkennung
* Nomen und Verb
* Interpunktion (Varianten mit korrekter und falscher Grammatik sowie ohne Grammatik)

## <a name="how-to-choose-varied-utterances"></a>Auswählen verschiedener Äußerungen

Wenn Sie mit Ihrem LUIS-Modell einsteigen, indem Sie [Beispieläußerungen hinzufügen](luis-how-to-add-example-utterances.md), finden Sie im Folgenden einige wichtige Prinzipien.

### <a name="utterances-arent-always-well-formed"></a>Äußerungen sind nicht immer ordnungsgemäß formatiert

Dabei kann es sich um einen Satz (z.B. „Ein Ticket nach Paris für mich buchen“) oder um einen Teil eines Satzes (z.B. „Buchen“ oder „Flug nach Paris“) handeln.  Benutzer machen oft Rechtschreibfehler. Überlegen Sie sich daher bei der Planung Ihrer App, ob Sie Benutzereingaben mithilfe der [Bing-Rechtschreibprüfung](luis-tutorial-bing-spellcheck.md) korrigieren möchten, bevor Sie sie an LUIS übergeben. 

Wenn Sie keine Rechtschreibprüfung auf die Äußerungen Ihrer Benutzer anwenden, sollten Sie LUIS auf Äußerungen trainieren, die Tipp- und Schreibfehler enthalten.

### <a name="use-the-representative-language-of-the-user"></a>Verwenden Sie Sprache, die den Benutzern entspricht

Beachten Sie bei der Wahl der Äußerungen, dass das, was Sie als allgemeinen Begriff oder Ausdruck kennen, unter Umständen für typische Benutzer Ihrer Clientanwendung nicht geeignet ist. Möglicherweise fehlt den Benutzern das Fachwissen. Seien Sie daher vorsichtig mit Begriffen oder Ausdrücken, die ein Benutzer nur verwenden würde, wenn er bestens mit der Thematik vertraut ist.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Auswählen variierender Terminologie und Ausdrücke

Auch wenn Sie sich bemühen, variierende Satzstrukturen zu verwenden, werden Sie feststellen, dass Sie einiges an Vokabular wiederholen müssen.

Sehen Sie sich diese Beispieläußerungen an:

|Beispiele für Äußerungen|
|--|
|Wie bekomme ich einen Computer?|
|Wo bekomme ich einen Computer?|
|Ich möchte einen Computer bekommen, wie gehe ich dazu vor?|
|Wann kann ich einen Computer bekommen?| 

Für den zentralen Begriff (Computer) sind keine Varianten vorhanden. Verwenden Sie Alternativen wie „Desktopcomputer“, „Laptop“, „Arbeitsstation“ oder sogar nur „Maschine“. LUIS kann diese Synonyme auf intelligente Weise vom Kontext ableiten. Wenn Sie jedoch Äußerungen zum Trainieren erstellen, ist es besser, diese zu variieren.

## <a name="example-utterances-in-each-intent"></a>Beispieläußerungen mit jeder Absicht

Jede Absicht benötigt mindestens 15 Beispieläußerungen. Wenn Sie jedoch über eine Absicht verfügen, die keine Beispieläußerungen aufweist, können Sie LUIS nicht trainieren. Wenn Sie über eine Absicht mit nur einer oder sehr wenigen Beispieläußerungen verfügen, kann LUIS die Absicht nicht genau vorhersagen. 

## <a name="add-small-groups-of-15-utterances-for-each-authoring-iteration"></a>Hinzufügen kleiner Gruppen mit 15 Äußerungen pro Erstellungsiteration

Fügen Sie nicht jeder Iteration des Modells große Mengen von Äußerungen hinzu. Fügen Sie Äußerungen in 15er Gruppen hinzu. [Trainieren](luis-how-to-train.md), [veröffentlichen](luis-how-to-publish-app.md) und [testen](luis-interactive-test.md) Sie anschließend erneut.  

LUIS erstellt effektive Modelle mit sorgfältig vom LUIS-Modellersteller ausgewählten Äußerungen. Das Hinzufügen zu vieler Äußerungen ist nicht nützlich, da es zu Verwechslungen führen kann.  

Es ist besser, mit nur wenigen Äußerungen anzufangen und anschließend die [Endpunktäußerungen zu überprüfen](luis-how-to-review-endpoint-utterances.md), um die richtige Vorhersage der Absicht und Extraktion der Entität zu gewährleisten.

## <a name="punctuation-marks"></a>Interpunktion

LUIS ignoriert Satzzeichen standardmäßig nicht, da einige Clientanwendungen diese unter Umständen als wichtig erachten. Stellen Sie sicher, dass Sie Beispieläußerungen mit und ohne Satzzeichen verwenden, damit beide Formate die gleichen relativen Ergebnisse zurückgeben. Wenn die Interpunktion in Ihrer Clientanwendung keine besondere Bedeutung hat, sollten Sie erwägen, [Satzzeichen zu ignorieren](#ignoring-words-and-punctuation), indem Sie Muster verwenden. 

## <a name="ignoring-words-and-punctuation"></a>Ignorieren von Wörtern und Interpunktion

Wenn Sie bestimmte Wörter oder Satzzeichen in der Beispieläußerung ignorieren möchten, verwenden Sie ein [Muster](luis-concept-patterns.md#pattern-syntax) mit der Syntax zum _Ignorieren_. 

## <a name="training-utterances"></a>Trainieren von Äußerungen

Das Training ist im Allgemeinen nicht deterministisch: Die Vorhersage von Äußerungen kann über verschiedene Versionen oder Apps hinweg variieren. Sie können nicht deterministisches Training entfernen, indem Sie die API für die [Versionseinstellungen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) mit dem `UseAllTrainingData`-Name-Wert-Paar aktualisieren, um alle Trainingsdaten zu verwenden.

## <a name="testing-utterances"></a>Testen von Äußerungen 

Entwickler sollten damit beginnen, ihre LUIS-Anwendung mit echtem Datenverkehr zu testen, indem sie Äußerungen an die URL des [Vorhersageendpunkts](luis-how-to-azure-subscription.md) senden. Diese Äußerungen werden verwendet, um die Leistung der Absichten und Entitäten mit [Überprüfungsäußerungen](luis-how-to-review-endpoint-utterances.md) zu verbessern. Tests, die über den Testbereich der LUIS-Website eingesendet werden, werden nicht über den Endpunkt gesendet und tragen daher nicht zum aktiven Lernen bei. 

## <a name="review-utterances"></a>Überprüfen von Äußerungen

Nachdem Ihr Modell trainiert sowie veröffentlicht ist und [Endpunktabfragen](luis-glossary.md#endpoint) empfängt, [überprüfen Sie die Äußerungen](luis-how-to-review-endpoint-utterances.md), die LUIS vorschlägt. LUIS wählt Endpunktäußerungen aus, die niedrige Bewertungen für die Absicht oder Entität aufweisen. 

## <a name="best-practices"></a>Bewährte Methoden

Machen Sie sich mit den [bewährten Methoden](luis-concept-best-practices.md) vertraut, und integrieren Sie sie in Ihren Erstellungszyklus.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Trainieren einer LUIS-App, damit sie Benutzeräußerungen versteht, finden Sie unter [Hinzufügen von Beispieläußerungen und -beschriftungen bei Entitäten](luis-how-to-add-example-utterances.md).

