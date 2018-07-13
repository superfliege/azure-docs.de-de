---
title: Überprüfen von Endpunktäußerungen für die Verwendung des aktiven Lernens in Language Understanding (LUIS) – Azure | Microsoft-Dokumentation
description: Verwenden Sie das Feature zum Überprüfen von Endpunktäußerungen für das aktive Lernen zur Verbesserung der Leistung und Geschwindigkeit von Vorhersagen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: b9672e8e63fb601d4411a342b7f3c00e30f9e002
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35379131"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Aktivieren des aktiven Lernens durch Überprüfen von Endpunktäußerungen
Aktives Lernen ist eine von drei Strategien zur Verbesserung der Vorhersagegenauigkeit. Von diesen ist sie am einfachsten zu implementieren. 

## <a name="what-is-active-learning"></a>Was ist aktives Lernen?
Aktives Lernen ist ein zweistufiger Prozess. LUIS wählt zunächst die am App-Endpunkt empfangenen Äußerungen aus, die validiert werden müssen. Der zweite Schritt wird vom App Besitzer oder Projektmitarbeiter ausgeführt. Dabei werden die ausgewählten Äußerungen [überprüft](label-suggested-utterances.md), um auch z.B. die richtige Absicht und eventuelle Entitäten in die Absicht zu validieren. Nach dem Überprüfen der Äußerungen trainieren und veröffentlichen Sie die App erneut. 

## <a name="which-utterances-are-on-the-review-list"></a>Äußerungen in der Prüfliste
LUIS fügt der Prüfliste Äußerungen hinzu, wenn die wichtigste ausgelöste Absicht eine niedrigere Bewertung hat oder wenn die Bewertungen der beiden wichtigsten Absichten zu ähnlich sind. 

## <a name="where-are-the-utterances-from"></a>Ursprung von Äußerungen
Endpunktäußerungen stammen aus Endbenutzerabfragen am HTTP-Endpunkt der Anwendung. Wenn Ihre App noch nicht veröffentlicht wurde oder noch keine Treffer empfangen hat, stehen keine Äußerungen für die Überprüfung bereit. Wenn für eine bestimmte Absicht oder Entität keine Treffer am Endpunkt empfangen werden, stehen auch keine Äußerungen zum Überprüfen bereit. 

## <a name="schedule-review-periodically"></a>Planen regelmäßiger Überprüfungen
Das Überprüfen vorgeschlagener Äußerungen muss nicht täglich erfolgen, sollte jedoch Teil Ihrer regelmäßigen Wartung von LUIS sein. 

## <a name="delete-review-items-programmatically"></a>Programmgesteuertes Löschen überprüfter Elemente
Wenn Ihre App sehr groß ist, können Sie einige Äußerungen überprüfen und die übrigen programmgesteuert aus der Liste entfernen. Dazu [rufen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) Sie zunächst die Liste ab und [löschen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) dann die Äußerungen anhand ihrer ID.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie Endpunktäußerungen [überprüfen](Label-Suggested-Utterances.md).
