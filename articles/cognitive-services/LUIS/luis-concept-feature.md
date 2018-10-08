---
title: Features in LUIS-Apps in Azure Cognitive Services
titleSuffix: Azure Cognitive Services
description: Fügen Sie einem Sprachmodell Features hinzu, um Hinweise zur Erkennung von Eingaben, die Sie bezeichnen oder klassifizieren möchten, bereitzustellen. Features helfen LUIS bei der Erkennung von Absichten und Entitäten.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 015679b6020e9d2a4d702f9d6e723ecd9499d8dc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034466"
---
# <a name="phrase-list-features-in-luis"></a>Features für Ausdruckslisten in LUIS

Beim maschinellen Lernen ist ein *Feature* ein eindeutiges Merkmal oder Attribut der Daten, die Ihr System untersucht. 

Fügen Sie einem Sprachmodell Features hinzu, um Hinweise zur Erkennung von Eingaben, die Sie bezeichnen oder klassifizieren möchten, bereitzustellen. Features helfen LUIS Absichten und Entitäten zu erkennen, Features sind jedoch nicht selbst Absichten oder Entitäten. Stattdessen können Features Beispiele für verwandte Begriffe liefern.  

## <a name="what-is-a-phrase-list-feature"></a>Was ist ein Feature für eine Ausdrucksliste?
Eine Ausdrucksliste enthält eine Gruppe von Werten (Wörter oder Ausdrücke), die derselben Klasse angehören und auf ähnliche Weise behandelt werden sollen (z.B. Städte- oder Produktnamen). Erkenntnisse, die LUIS zu einem davon sammelt, werden automatisch auch auf die anderen angewandt. Diese Liste ist keine geschlossene [Listenentität](luis-concept-entity-types.md#types-of-entities) (genaue Textübereinstimmungen) der übereinstimmenden Wörter.

Eine Ausdrucksliste ist Teil des Vokabulars der App-Domäne als zweiter Hinweis zu diesen Wörtern für LUIS.

## <a name="how-to-use-phrase-lists"></a>Verwenden von Ausdruckslisten
Im [Tutorial für einfache Entität](luis-quickstart-primary-and-secondary-data.md) der Human Resource-App verwendet die App eine Begriffsliste **Job** mit Jobtypen wie z.B. Programmierer, Dachdecker und Sekretärin. Wenn Sie einen dieser Werte als durch maschinelles Lernen erworbene Entität bezeichnen, lernt LUIS, die anderen zu erkennen. 

Eine Ausdrucksliste kann austauschbar oder nicht austauschbar sein. Eine *austauschbare* Ausdrucksliste wird für Werte verwendet, die synonym sind, und eine *nicht austauschbare* Ausdrucksliste ist für Werte vorgesehen, die keine Synonyme sind, aber dennoch ein zusätzliches Signal in der App benötigen. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>
## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Begriffslisten helfen bei der Erkennung einfach austauschbarer Entitäten
Austauschbare Begriffslisten sind eine gute Möglichkeit, die Leistung Ihrer LUIS-App zu optimieren. Wenn die App Probleme bei der Vorhersage von Äußerungen zu den richtigen Absichten oder der Erkennung von Entitäten hat, sollten Sie überlegen, ob die Äußerungen ungewöhnliche oder mehrdeutige Wörter enthalten. Diese Wörter sind gute Kandidaten für eine Ausdrucksliste.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Ausdruckslisten helfen durch ein besseres Kontextverständnis bei der Erkennung von Absichten
Eine Ausdrucksliste ist keine Anweisung an LUIS, einen strikten Abgleich auszuführen oder immer alle Begriffe in der Ausdrucksliste genau gleich zu bezeichnen. Sie stellt lediglich einen Hinweis dar. Angenommen, Sie verfügen über eine Begriffsliste, die angibt, dass „Patti“ und „Selma“ Namen sind, aber LUIS weiterhin Kontextinformationen verwenden kann, um zu erkennen, dass sie etwas anderes in „Reservierung für 2 Personen in Patti‘s Diner“ oder „zeige mir die Route nach Selma, Georgia“ bedeuten. 

Das Hinzufügen einer Ausdrucksliste stellt eine Alternative zum Hinzufügen zusätzlicher Beispieläußerungen zu einer Absicht dar. 

## <a name="an-interchangeable-phrase-list"></a>Eine austauschbare Begriffsliste
Verwenden Sie eine austauschbare Begriffsliste, wenn die Liste der Wörter oder Begriffe eine Klasse oder Gruppe bildet. Ein Beispiel ist eine Liste der Monate, z.B. „Januar“, „Februar“, „März“, oder Namen wie „John“, „Mary“, „Frank“.  Diese Listen sind insofern austauschbar, als die Äußerung mit der gleichen Absicht oder Entität bezeichnet würde, wenn ein anderes Wort in der Begriffsliste verwendet würde. Wenn „den Kalender für Januar anzeigen“ die gleiche Absicht hat wie „den Kalender für Februar anzeigen“, würden die Wörter auf einer austauschbaren Liste stehen. 

## <a name="a-non-interchangeable-phrase-list"></a>Eine nicht austauschbare Begriffsliste
Verwenden Sie eine nicht austauschbare Begriffsliste für nicht gleichbedeutende Wörter oder Begriffe, die in Ihrer Domäne gruppiert werden können. 

Verwenden Sie beispielsweise eine nicht austauschbare Begriffsliste für seltene, proprietäre und Fremdwörter. LUIS kann möglicherweise seltene und proprietäre Wörter sowie Fremdwörter (außerhalb der Kultur der App) nicht erkennen. Diese nicht austauschbare Einstellung gibt an, dass die seltenen Wortformen eine Klasse bilden, deren Erkennung LUIS erlernen sollte, die aber keine Synonyme oder untereinander austauschbar sind.

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Einsatzmöglichkeiten für Ausdruckslisten oder Listenentitäten
Während sowohl eine Ausdrucksliste als auch Listenentitäten Auswirkungen auf Äußerungen für alle Absichten haben können, unterscheiden sich diese Auswirkungen. Sie verwenden eine Ausdrucksliste, um die Vorhersage von Absichten zu beeinflussen. Sie verwenden eine Listenentität, um die Extraktion von Entitäten für eine genaue Textübereinstimmung zu beeinflussen. 

### <a name="use-a-phrase-list"></a>Wann Sie Ausdruckslisten verwenden
Mit einer Ausdrucksliste kann LUIS weiterhin den Kontext berücksichtigen und eine Generalisierung zum Identifizieren von ähnlichen Elementen ausführen, aber keine genaue Übereinstimmung wie bei Elementen in einer Liste. Wenn Sie Ihre LUIS-App in der Lage sein soll, zu generalisieren und neue Elemente in einer Kategorie zu identifizieren, verwenden Sie eine Ausdrucksliste. 

Wenn Sie neue Instanzen einer Entität erkennen möchten, z.B. einen Besprechungsplan, bei dem auch die Namen neuer Kontakte ermittelt werden, oder eine Inventur-App, bei der neue Produkte ermittelt werden sollen, verwenden Sie einen anderen Entitätstyp für das maschinelle Lernen, z.B. eine einfache oder hierarchische Entität. Erstellen Sie anschließend eine Ausdrucksliste der Wörter und Ausdrücke, die LUIS beim Identifizieren von Wörtern hilft, die der Entität ähneln. Diese Liste unterstützt LUIS beim Erkennen von Beispielen für die Entität durch das Hinzufügen einer zusätzlichen Signifikanz zum Wert dieser Wörter. 

Ausdruckslisten ähneln einem domänenspezifischen Vokabular, das zur Verbesserung des Verständnisses von Absichten und Entitäten beiträgt. Allgemein empfiehlt sich die Verwendung einer Ausdrucksliste mit Substantiven wie Ortsnamen. Der Name einer Stadt kann mehrere Wörter sowie Bindestriche oder Apostrophe enthalten.
 
### <a name="dont-use-a-phrase-list"></a>Wann Sie keine Ausdruckslisten verwenden 
Eine Listenentität definiert explizit jeden Wert, den eine Entität annehmen kann. Es werden außerdem ausschließlich exakte übereinstimmende Werte identifiziert. Eine Listenentität kann sich für eine App eignen, in der alle Instanzen einer Entität bekannt sind und sich nicht häufig ändern. Dies gilt z.B. für die Gerichte auf der Speisekarte eines Restaurants, die sich selten ändern. Wenn Sie eine genaue Textübereinstimmung einer Entität benötigen, verwenden Sie keine Ausdrucksliste. 

## <a name="best-practices"></a>Bewährte Methoden
Informationen zu [Best Practices](luis-concept-best-practices.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Hinzufügen von Features zu LUIS-Apps finden Sie unter [Hinzufügen von Features](luis-how-to-add-features.md).
