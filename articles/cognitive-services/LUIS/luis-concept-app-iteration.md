---
title: Grundlegendes zur iterativen Erstellung von LUIS-Apps – Azure | Microsoft-Dokumentation
description: LUIS-Apps erfordern den Entwurf von Iterationen zum Trainieren von LUIS für eine optimale Datenextraktion.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/12/2018
ms.author: v-geberr
ms.openlocfilehash: e0467e4c41209c937f548edc0c40c05cae588f4c
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888247"
---
# <a name="authoring-cycle"></a>Erstellungszyklus
LUIS lernt am besten in einem iterativen Zyklus aus Modelländerungen, Äußerungsbeispielen, Veröffentlichungen und dem Sammeln von Daten aus Endpunktabfragen. 

![Erstellungszyklus](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Erstellen eines LUIS-Modells
Mithilfe des Modells soll ermittelt werden, was der Benutzer fragt (Absicht) und welche Teile der Frage Details (Entitäten) bereitstellen, mit deren Hilfe die Antwort bestimmt werden kann. 

Das Modell muss spezifisch für die App-Domäne sein, um Wörter und Ausdrücke zu ermitteln, die relevant sind und in einer typischen Reihenfolge vorliegen. 

Das Modell enthält die Absicht und Entitäten. 

## <a name="add-training-examples"></a>Hinzufügen von Trainingsbeispielen
LUIS benötigt Beispieläußerungen in Absichten. Für die Beispiele sind ausreichend viele Variationen bei der Wortauswahl und Wortreihenfolge erforderlich, um die Absicht der Äußerung zu bestimmen. Bei sämtlichen Beispieläußerung müssen alle erforderlichen Daten als Entitäten bezeichnet sein. 

Sie weisen LUIS an, Äußerungen zu ignorieren, die für Ihre App-Domäne nicht relevant sind, indem Sie die Äußerung der Absicht **None** zuweisen. Alle Wörter oder Ausdrücke, die nicht aus einer Äußerung gepullt werden sollen, müssen nicht bezeichnet werden. Es sind keine Bezeichnungen für Wörter oder Ausdrücke erforderlich, die ignoriert werden sollen. 
<!--
## Not just yet
Do not add features such as a [phrase list](luis-concept-feature.md) feature in your first cycle. Phrase lists are phrases that would be specific to your app's subject area.  
-->
## <a name="train-and-publish-the-app"></a>Trainieren und Veröffentlichen der App
Wenn Sie zu jeder Absicht 10 bis 15 Äußerungen haben und die erforderlichen Entitäten mit Bezeichnungen versehen sind, trainieren Sie LUIS und führen dann die Veröffentlichung durch, um Ihre Endpunkte zu erhalten. Stellen Sie sicher, dass Ihre App erstellt und veröffentlicht wurde, damit sie in den [Endpunktregionen](luis-reference-regions.md) verfügbar ist. 

## <a name="https-endpoint-testing"></a>HTTPS-Tests an Endpunkten
Sie können Ihre LUIS-App von dem auf der Seite **[Veröffentlichen](luis-how-to-publish-app.md)** aufgeführten HTTPS-Endpunkt aus testen. Durch Tests vom Endpunkt kann LUIS alle Äußerungen mit niedrigen Zuverlässigkeitswerten zur Überprüfung auswählen.  

## <a name="recycle"></a>Recyceln
Wenn Sie einen Erstellungszyklus abgeschlossen haben, können Sie erneut beginnen. Starten Sie mit dem Überprüfen der Endpunktäußerungen, die LUIS mit niedrigen Zuverlässigkeitswerten gekennzeichnet hat. Überprüfen Sie diese Äußerungen auf Absichten und Entitäten. Nach dem Überprüfen der Äußerungen sollte die Prüfliste leer sein.  

## <a name="batch-testing"></a>Testen in Batches
Durch das Testen in Batches können Sie sehen, wie viele Beispieläußerungen von LUIS bewertet werden. Die Beispiele sollten für LUIS neu sein, und sie sollten richtig mit den Absichten und Entitäten bezeichnet sein, die LUIS finden soll. Die Testergebnisse zeigen an, wie gut LUIS diese Gruppe von Äußerungen auswertet. 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die Konzepte der [Zusammenarbeit](luis-concept-collaborator.md).