---
title: Erfahren Sie mehr über das Verbessern der Vorhersagegenauigkeit mit Mustern | Microsoft Docs
titleSuffix: Azure
description: Erfahren Sie, wie Muster entworfen werden, um die Vorhersagegenauigkeit der Absichtsbewertung zu verbessern und Entitäten zu finden.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr
ms.openlocfilehash: 4a23ebddc29c6c519e68a00ef6cbff107061c341
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888278"
---
# <a name="patterns-improve-prediction-accuracy"></a>Verbessern der Vorhersagegenauigkeit mit Mustern
Muster werden entworfen, um die Genauigkeit zu erhöhen, wenn mehrere Äußerungen sehr ähnlich sind. Durch Bereitstellen eines Musters für die Äußerung kann LUIS große Vertrauenswürdigkeit bei der Vorhersage bieten. 

## <a name="patterns-solve-low-intent-confidence"></a>Muster als Weg bei wenig vertrauenswürdigen Absichten
Stellen Sie sich eine Personalwesen-App vor, die das Organigramm bezogen auf einen Mitarbeiter auswertet. Bei Angabe des Namens und der Beziehungen eines Mitarbeiters gibt LUIS die beteiligten Mitarbeiter zurück. Sehen wir uns einen Mitarbeiter Tom mit einer Vorgesetzten Alice und einem Team von ihm Unterstellten mit den Namen Michael, Rebekka und Karl an.

![Bild des Organigramms](./media/luis-concept-patterns/org-chart.png)

|Äußerungen|Vorhergesagte Absicht|Absichtsbewertung|
|--|--|--|
|Wer sind Toms Untergebene?|GetOrgChart|0,30|
|Wer ist der Untergebene von Tom?|GetOrgChart|0,30|

Wenn einer App zwischen 10 und 20 Äußerungen mit unterschiedlicher Satzlänge, verschiedener Wortstellung und sogar verschiedenen Wörtern (Synonymen von „Untergebener“, „Verwalten“, „Vorgesetzter“) vorliegen, gibt LUIS möglicherweise eine geringe Zuverlässigkeitsbewertung zurück. Um LUIS zu helfen, die Wichtigkeit der Wortstellung zu erfassen, erstellen Sie ein Muster. 

Muster lösen Probleme in den folgenden Situationen: 

* Bei geringer Zuverlässigkeitsbewertung
* Wenn die richtige Absicht nicht die höchste Punktzahl hat, der höchsten Punktzahl aber sehr nahe kommt 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Muster stellen eine Garantie für Absicht dar
Muster verwenden eine Kombination von Vorhersagetechnologien. Das Festlegen einer Absicht für eine Vorlagenäußerung in einem Muster stellt keine Garantie für die Absichtsvorhersage dar, liefert aber ein starkes Signal. 

## <a name="patterns-do-not-improve-entity-detection"></a>Muster verbessern nicht die Entitätserkennung
Zwar sind Entitäten Voraussetzung für Muster, Muster helfen beim Erkennen einer Entität aber nicht weiter. Ein Muster ist nur zur Unterstützung der Vorhersage bei Absichten und Rollen vorgesehen.  

## <a name="patterns-use-entity-roles"></a>Muster verwenden Entitätsrollen
Wenn zwei oder mehr Entitäten in einem Muster kontextverwandt sind, setzen Muster [Entitätsrollen](luis-concept-roles.md) ein, um Kontextinformationen zu den Entitäten zu extrahieren. Dies ist hierarchisch gleichbedeutend mit untergeordneten Entitäten, steht aber **nur** in Mustern zur Verfügung. 

## <a name="prediction-scores-with-and-without-patterns"></a>Vorhersagebewertungen mit und ohne Muster
Eine ausreichende Anzahl von Beispieläußerungen vorausgesetzt, wäre LUIS imstande, die Vorhersagezuverlässigkeit ohne Muster zu steigern. Mithilfe von Mustern lässt sich die Zuverlässigkeitsbewertung steigern, ohne so viele Äußerungen zur Verfügung stellen zu müssen.  

## <a name="pattern-matching"></a>Musterabgleich
Ein Musterabgleich erfolgt so, dass zuerst die Entitäten innerhalb des Musters erkannt und dann der Rest der Wörter und die Wortstellung bewertet werden. Entitäten sind im Muster erforderlich, damit ein Musterabgleich erfolgen kann. 

## <a name="pattern-syntax"></a>Mustersyntax
Die Mustersyntax ist eine Vorlage für eine Äußerung. Die Vorlage sollte sowohl Wörter und Entitäten enthalten, die abgeglichen werden sollen, als auch Wörter und Interpunktion, die ignoriert werden sollen. Sie ist **kein** regulärer Ausdruck. 

Entitäten in Mustern sind in geschweifte Klammern, `{}`, eingeschlossen. Muster können Entitäten und Entitäten mit Rollen enthalten. „Pattern.any“ ist eine Entität, die nur in Mustern verwendet wird. Die Syntax wird in den folgenden Abschnitten erläutert.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Syntax zum Hinzufügen einer Entität zu einer Mustervorlage
Um der Mustervorlage eine Entität hinzuzufügen, schließen Sie den Namen der Entität in geschweiften Klammern ein, wie hier: `Who does {Employee} manage?`. 

```
Who does {Employee} manage?
```

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Syntax zum Hinzufügen einer Entität und einer Rolle zu einer Mustervorlage
Eine Entitätsrolle wird als `{entity:role}` notiert, wobei ein Doppelpunkt und dann der Name der Rolle auf den Namen der Entität folgen. Um der Mustervorlage eine Entität mit einer Rolle hinzuzufügen, schließen Sie den Namen der Entität und den Namen der Rolle in geschweiften Klammern ein, wie hier: `Book a ticket from {Location:Origin} to {Location:Destination}`. 

```
Book a ticket from {Location:Origin} to {Location:Destination}
```

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Syntax zum Hinzufügen eines „pattern.any“ zu einer Mustervorlage
Die Pattern.any-Entität ermöglicht Ihnen das Hinzufügen einer Entität mit variabler Länge zum Muster. Sofern die Mustervorlage befolgt wird, kann „pattern.any“ jede beliebige Länge aufweisen. 

Um der Mustervorlage eine **Pattern.any**-Entität hinzuzufügen, schließen Sie die Pattern.any-Entität in geschweifte Klammern ein, wie hier: `How much does {Booktitle} cost and what format is it available in?`.  

```
How much does {Booktitle} cost and what format is it available in?
```

|Buchtitel im Muster|
|--|
|Wie viel kostet **stiehl dieses Buch**, und in welchen Formaten ist es verfügbar?|
|Wie viel kostet **fragen**, und in welchen Formaten ist es verfügbar?|
|Wie viel kostet **Supergute Tage oder die sonderbare Welt des Christopher Boone**, und in welchen Formaten ist es verfügbar?| 

In diesen Beispielen für Buchtitel führen die Kontextwörter des Buchtitels bei LUIS nicht zu Verwirrung. LUIS weiß, wo der Buchtitel endet, weil er sich in einem Muster befindet und mit einer Pattern.any-Entität gekennzeichnet ist.

### <a name="explicit-lists"></a>Explizite Listen
Wenn Ihr Muster eine Pattern.any-Entität enthält und die Mustersyntax auf der Grundlage der Äußerung die Möglichkeit einer falschen Entitätsextraktion zulässt, erstellen Sie eine [explizite Liste](https://aka.ms/ExplicitList) mithilfe der Verfasser-API, um die Ausnahme zuzulassen. 

Nehmen wir beispielsweise an, Sie haben ein Muster, das sowohl optionale Syntax `[]` als auch Entitätssyntax `{}` in einer Kombination enthält, die zu einer fehlerhaften Extraktion der Daten führt.

Betrachten Sie das Muster ‚[Suche] E-Mail zu {Betreff} [von {Person}]‘. In den folgenden Äußerungen werden die Entitäten **Betreff** und **Person** richtig und falsch extrahiert:

|Äußerung|Entität|Richtige Extraktion|
|--|--|:--:|
|E-Mail über Hunde von Chris|Betreff=Hunde<br>Person=Chris|✔|
|E-Mail über den Mann aus La Mancha|Betreff=der Mann<br>Person=La Mancha|X|

In der vorstehenden Tabelle sollte in der Äußerung `email about the man from La Mancha` der Betreff `the man from La Mancha` (ein Buchtitel) sein, aber da der Betreff das optionale Wort `from` enthält, wird der Titel falsch vorhergesagt. 

Um diese Ausnahme im Muster zu beheben, fügen Sie mithilfe der [Verfasser-API für explizite Listen](https://aka.ms/ExplicitList) `the man from la mancha` als expliziten Listentreffer für die {Betreff}-Entität hinzu.

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Syntax zum Kennzeichnen von optionalem Text in einer Vorlagenäußerung
Kennzeichnen Sie optionalen Text in der Äußerung mithilfe der Syntax für eckige Klammern in regulären Ausdrücken, `[]`. In optionalem Text können eckige Klammern nur bis zu einer Tiefe von zwei Klammern verschachtelt werden.

```
[find] email about {subject} [from {person}]
```

Interpunktionszeichen wie `.`, `!` und `?` können mithilfe der eckigen Klammern ignoriert werden. Um diese Kennzeichnungen zu ignorieren, muss sich jede Kennzeichnung in einem separaten Muster befinden. Die optionale Syntax unterstützt derzeit das Ignorieren eines Elements in einer Liste aus mehreren Elementen nicht.

## <a name="patterns-only"></a>Nur Muster
LUIS lässt eine App ohne jegliche Beispieläußerungen in der Absicht zu. Diese Verwendung ist nur beim Einsatz von Mustern zulässig. Für Muster ist mindestens eine Entität in jedem Muster erforderlich. Bei Apps, die nur auf Mustern beruhen, sollte das Muster keine maschinell gelernten Entitäten enthalten, da für diese Beispieläußerungen erforderlich sind. 

## <a name="best-practices"></a>Bewährte Methoden
Informationen zu [Best Practices](luis-concept-best-practices.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [In diesem Tutorial ](luis-tutorial-pattern.md) erfahren Sie mehr über das Implementieren von Mustern.