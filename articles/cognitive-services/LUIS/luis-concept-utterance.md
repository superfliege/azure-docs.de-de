---
title: Äußerungen in LUIS-Apps in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Äußerungen in LUIS-Apps (Language Understanding Intelligent Service) hinzufügen können.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/13/2018
ms.author: v-geberr
ms.openlocfilehash: 66a23876eebe177c767b20f60f86891c35da3385
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301861"
---
# <a name="utterances-in-luis"></a>Äußerungen in LUIS

**Äußerungen** sind Eingaben vom Benutzer, die Ihre App interpretieren muss. Es ist wichtig, dass eine Vielzahl verschiedener Eingaben für jede Absicht erfasst wird, um LUIS zum Extrahieren von Absichten und Entitäten zu trainieren. Das aktive Lernen bzw. der Prozess des kontinuierlichen Trainierens neuer Äußerungen ist ein grundlegender Bestandteil der Intelligenz durch Machine Learning, die LUIS bereitstellt.

Sammeln Sie Ausdrücke, die Benutzer möglicherweise eingeben. Fügen Sie Äußerungen ein, die dieselbe Bedeutung haben, aber andere Wortlängen oder einen anderen Satzbau aufweisen. 

## <a name="how-to-choose-varied-utterances"></a>Auswählen verschiedener Äußerungen
Wenn Sie mit Ihrem LUIS-Modell einsteigen, indem Sie [Beispieläußerungen hinzufügen][add-example-utterances], finden Sie im Folgenden einige wichtige Prinzipien.

### <a name="utterances-arent-always-well-formed"></a>Äußerungen sind nicht immer ordnungsgemäß formatiert
Es kann sich um einen Satz (z.B. „Ein Ticket nach Paris buchen“) oder einen Teil eines Satzes (z.B. „Buchen“ oder „Flug nach Paris“) handeln.  Benutzer machen oft Rechtschreibfehler. Deshalb sollten Sie sich bei der Planung Ihrer App überlegen, ob Sie eine Rechtschreibprüfung auf die Eingabe Ihrer Benutzer anwenden möchten, bevor diese an LUIS weitergegeben wird. Die [Bing-Rechtschreibprüfungs-API][BingSpellCheck] kann in LUIS integriert werden. Sie können Ihrer LUIS-App einen externen Schlüssel für die Bing-Rechtschreibprüfungs-API zuordnen, wenn Sie sie veröffentlichen. Wenn Sie keine Rechtschreibprüfung auf die Äußerungen Ihrer Benutzer anwenden, sollten Sie LUIS auf Äußerungen trainieren, die Tipp- und Schreibfehler enthalten.

### <a name="use-the-representative-language-of-the-user"></a>Verwenden Sie Sprache, die den Benutzern entspricht
Beachten Sie bei der Auswahl der Äußerungen, dass das, was Sie als allgemeinen Begriff oder Ausdruck kennen, möglicherweise für typische Benutzer Ihrer Clientanwendung fremd ist. Möglicherweise fehlt den Benutzern das Fachwissen. Sie sollten also vorsichtig mit Begriffen oder Ausdrücken sein, die ein Benutzer nur verwenden würde, wenn er ein Sachverständiger in diesem Feld wäre.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Auswählen variierender Terminologie und Ausdrücke
Auch wenn Sie sich bemühen, variierende Satzstrukturen zu verwenden, werden Sie feststellen, dass Sie einiges an Vokabular wiederholen müssen.

Sehen Sie sich diese Beispieläußerungen an:
```
how do I get a computer?
Where do I get a computer?
I want to get a computer, how do I go about it?
When can I have a computer? 
```
Der grundlegende Begriff ist „computer“. Dieser Begriff variiert nicht. Man könnte auch die Begriffe „desktop computer“, „laptop“, „workstation“ oder sogar „machine“ verwenden. LUIS kann diese Synonyme auf intelligente Weise vom Kontext ableiten. Wenn Sie jedoch Äußerungen zum Trainieren erstellen, ist es besser, diese zu variieren.

## <a name="example-utterances-in-each-intent"></a>Beispieläußerungen mit jeder Absicht
Jede Absicht benötigt mindestens 10 bis 15 Beispieläußerungen. Wenn Sie jedoch über eine Absicht verfügen, die keine Beispieläußerungen aufweist, können Sie LUIS nicht trainieren. Wenn Sie über eine Absicht mit nur einer oder sehr wenigen Beispieläußerungen verfügen, kann LUIS die Absicht nicht genau vorhersagen. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>Fügen Sie kleine Gruppen von 10 bis 15 Äußerungen für jede Erstellungsiteration hinzu
Fügen Sie nicht jeder Iteration des Modells große Mengen von Äußerungen hinzu. Fügen Sie Äußerungen in Mengen von zehn Äußerungen hinzu. [Trainieren](luis-how-to-train.md), [veröffentlichen](publishapp.md) und [testen](interactive-test.md) Sie anschließend erneut.  

Effektive LUIS-Modelle werden mit sorgfältig ausgewählten Äußerungen erstellt. Das Hinzufügen zu vieler Äußerungen ist nicht nützlich, da es zu Verwechslungen führen kann.  

Es ist besser, mit nur wenigen Äußerungen anzufangen und anschließend die [Endpunktäußerungen zu überprüfen](label-suggested-utterances.md), um die richtige Vorhersage der Absicht und Extraktion der Entität zu gewährleisten.

## <a name="ignoring-words-and-punctuation"></a>Ignorieren von Wörtern und Interpunktion
Wenn Sie bestimmte Wörter oder Satzzeichen in der Beispieläußerung ignorieren möchten, verwenden Sie ein [Muster](luis-concept-patterns.md#pattern-syntax) mit der Syntax zum _Ignorieren_. 

## <a name="training-utterances"></a>Trainieren von Äußerungen
Das Training ist nicht deterministisch: die Vorhersage von Äußerungen kann über verschiedene Versionen oder Apps hinweg variieren.

## <a name="testing-utterances"></a>Testen von Äußerungen 

Entwickler sollten damit beginnen, ihre LUIS-Anwendung mit echtem Datenverkehr zu testen, indem sie Äußerungen an den Endpunkt senden. Diese Äußerungen werden verwendet, um die Leistung der Absichten und Entitäten mit [Überprüfungsäußerungen](label-suggested-utterances.md) zu verbessern. Tests, die über den Testbereich der LUIS-Website eingesendet werden, werden nicht über den Endpunkt gesendet und tragen daher nicht zum aktiven Lernen bei. 

## <a name="review-utterances"></a>Überprüfen von Äußerungen
Nachdem Ihr Modell trainiert sowie veröffentlicht ist und [Endpunktabfragen](luis-glossary.md#endpoint) empfängt, [überprüfen Sie die Äußerungen](label-suggested-utterances.md), die LUIS vorschlägt. LUIS wählt Endpunktäußerungen aus, die niedrige Bewertungen für die Absicht oder Entität aufweisen. 

## <a name="best-practices"></a>Bewährte Methoden
Informieren Sie sich über die [bewährten Methoden](luis-concept-best-practices.md), um mehr zu erfahren.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Trainieren einer LUIS-App, damit sie Benutzeräußerungen versteht, finden Sie unter [Add example utterances (Hinzufügen von Beispieläußerungen)][add-example-utterances].

[add-example-utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-add-example-utterances
[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text