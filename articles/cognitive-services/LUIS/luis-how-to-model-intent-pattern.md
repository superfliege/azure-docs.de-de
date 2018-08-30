---
title: Hinzufügen von Mustervorlagen in LUIS-Apps | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie Mustervorlagen in LUIS-Anwendungen (Language Understanding Intelligent Service) hinzufügen, um die Vorhersagegenauigkeit zu verbessern.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry
ms.openlocfilehash: 8926ce48a8426aa667787f7bbe104e27e1a68669
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124683"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Hinzufügen von Mustern zum Verbessern der Vorhersagegenauigkeit
Nachdem eine LUIS-App Endpunktäußerungen empfängt, verbessern Sie mithilfe des [Konzepts](luis-concept-patterns.md) von Mustern die Vorhersagegenauigkeit für Äußerungen, die ein Muster in der Wortreihenfolge und Wortwahl zeigen. Muster verwenden [Entitäten](luis-concept-entity-types.md) und deren Rollen zum Extrahieren von Daten mithilfe einer spezifischen Mustersyntax. 

## <a name="add-template-utterance-to-create-pattern"></a>Hinzufügen von Vorlagenäußerungen zum Erstellen von Mustern
1. Öffnen Sie Ihre App, indem Sie den Namen auf der Seite **Meine Apps** auswählen, und wählen Sie dann **Muster** im linken Bereich unter **Improve app performance** (App-Leistung verbessern) aus.

    ![Screenshot der Musterliste](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Wählen Sie die richtige Absicht für das Muster aus. 

    ![Auswählen der Absicht](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Geben Sie im Textfeld der Vorlage die Vorlagenäußerung ein, und drücken Sie die EINGABETASTE. Wenn Sie den Namen der Entität eingeben möchten, verwenden Sie die richtige Musterentitätssyntax. Beginnen Sie die Entitätssyntax mit `{`. Die Liste der Entitäten wird angezeigt. Wählen Sie die richtige Entität aus, und drücken Sie die EINGABETASTE. 

    ![Screenshot der Entität für das Muster](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Wenn die Entität eine Rolle enthält, geben Sie die Rolle mit einem einzelnen Doppelpunkt `:` nach dem Entitätsnamen an, z.B. `{Location:Origin}`. Die Liste der Rollen für die Entitäten wird in einer Liste angezeigt. Wählen Sie die Rolle aus, und drücken Sie die EINGABETASTE. 

    ![Screenshot der Entität mit der Rolle](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Nach dem Auswählen der richtigen Entität beenden Sie die Eingabe des Musters und drücken die EINGABETASTE. Wenn Sie die Mustereingabe beendet haben, [trainieren](luis-how-to-train.md) Sie Ihre App.

    ![Screenshot eines eingegebenen Musters mit beiden Entitätstypen](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Suchmuster
Sie können Muster suchen, die einen bestimmten Text enthalten.  

1. Wählen Sie das Lupensymbol aus.

    ![Screenshot der Seite „Muster“ mit hervorgehobenem Symbol des Suchtools](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Geben Sie den Suchtext in das Suchfeld in der rechten oberen Ecke der Musterliste ein, und drücken Sie die EINGABETASTE. Die Musterliste wird aktualisiert, sodass nur die Muster angezeigt werden, die den Suchtext enthalten.

    ![Screenshot der Seite „Muster“ mit hervorgehobenem Suchtext im Suchfeld](./media/luis-how-to-model-intent-pattern/search-text.png)

    Um die Suche abzubrechen und die vollständige Liste der Muster wiederherzustellen, löschen Sie den eingegebenen Suchtext.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Bearbeiten eines Musters
1. Um ein Muster zu bearbeiten, wählen Sie die Auslassungspunkte (**...**) am rechten Ende der Zeile für dieses Muster aus, und wählen Sie dann ***Bearbeiten*** aus. 

    ![Screenshot des Menüelements „Bearbeiten“ in einer Musterzeile](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Geben Sie Änderungen in das Textfeld ein. Wenn Sie fertig sind, drücken Sie die EINGABETASTE. Wenn Sie das Bearbeiten von Mustern beendet haben, [trainieren](luis-how-to-train.md) Sie Ihre App.

    ![Screenshot der Bearbeitung eines Musters](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Neuzuweisen von einzelnen Muster zu einer anderen Absicht

Um ein einzelnes Muster einer anderen Absicht neu zuzuweisen, wählen Sie das Feld mit der Liste der Absichten rechts neben dem Mustertext und dann eine andere Absicht aus.

![Screenshot der Neuzuweisung eines einzelnen Musters zu einer anderen Absicht](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Zuweisen von mehreren Mustern zu unterschiedlichen Absichten

Um mehrere Muster einer anderen Absicht neu zuzuweisen, aktivieren Sie die Kontrollkästchen links neben den einzelnen Mustern, oder aktivieren Sie das oberste Kontrollkästchen. Die Option **Reassign intent** (Absicht neu zuweisen) wird auf der Symbolleiste angezeigt. Wählen Sie die richtige Absicht für die Muster aus. 

![Screenshot der Neuzuweisung mehrerer Muster zu einer anderen Absicht](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Löschen eines einzelnen Musters

1. Um ein Muster zu löschen, wählen Sie die Auslassungspunkte (**...**) am rechten Ende der Zeile für dieses Muster aus, und wählen Sie dann ***Löschen*** aus. 

    ![Screenshot des Löschens einer Äußerung](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Wählen Sie **OK** aus, um den Löschvorgang zu bestätigen.

    ![Screenshot der Löschbestätigung](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Löschen von mehreren Mustern

1. Um mehrere Muster zu löschen, aktivieren Sie die Kontrollkästchen links neben den einzelnen Mustern, oder aktivieren Sie das oberste Kontrollkästchen. Die Option **Delete pattern(s)** (Muster löschen) wird auf der Symbolleiste angezeigt. Wählen Sie **Delete pattern(s)** (Muster löschen) aus.  

    ![Screenshot des Löschens mehrerer Muster](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. Das Bestätigungsdialogfeld **Delete patterns** (Muster löschen) wird angezeigt. Wählen Sie **OK** aus, um den Löschvorgang abzuschließen.

    ![Screenshot des Löschens mehrerer Muster](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Filtern der Musterliste nach Entität

Wählen Sie zum Filtern der Musterliste nach einer bestimmten Entität auf der Symbolleiste über den Mustern die Option **Entity filters** (Entitätsfilter) aus. 

![Screenshot der Filterung von Mustern nach Entität](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Nach dem Anwenden des Filters wird der Entitätsname unter der Symbolleiste angezeigt. 

## <a name="filter-pattern-list-by-intent"></a>Filtern der Musterliste nach Absicht

Wählen Sie zum Filtern der Musterliste nach einer bestimmten Absicht auf der Symbolleiste über den Mustern **Intent filters** (Absichtsfilter) aus. 

![Screenshot der Filterung von Mustern nach Absicht](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Nach dem Anwenden des Filters wird der Absichtsname unter der Symbolleiste angezeigt. 

## <a name="remove-entity-or-intent-filter"></a>Entfernen eines Entitäts- oder Absichtsfilters
Wenn die Liste gefiltert wurde, wird der Name der Entität oder Absicht unter der Symbolleiste angezeigt. Um den Filter zu entfernen, wählen Sie den Namen aus.

![Screenshot von nach Entität gefilterten Mustern](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

Der Filter wird entfernt, und alle Muster werden angezeigt. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Hinzufügen von Mustern aus vorhandenen Äußerungen auf der Seite einer Absicht oder Entität
Sie können auf einer der Seiten **Intent** (Absicht) oder **Entity** (Entität) ein Muster aus einer vorhandenen Äußerung erstellen. Alle Äußerungen auf einer beliebigen Seite einer Absicht oder Entität werden in einer Liste angezeigt, wobei die rechte Spalte Zugriff auf Optionen auf Äußerungsebene bietet, z.B. **Bearbeiten**, **Löschen** oder **Add as pattern** (Als Muster hinzufügen).

1. Wählen Sie in der ausgewählten Zeile der Äußerung die Auslassungspunkte (**...**) rechts neben der Äußerung und dann ***Als Muster hinzufügen*** aus.

    [![](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "Screenshot der Äußerungstabelle mit hervorgehobener Option „Add as pattern“ (Als Muster hinzufügen) im Menü „Optionen“")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Ändern Sie das Muster gemäß den [Syntaxregeln](luis-concept-patterns.md#pattern-syntax). Wenn die ausgewählte Äußerung mit Entitäten bezeichnet ist, befinden sich diese Entitäten bereits mit der richtigen Syntax im Muster.

    ![Screenshot von nach Entität gefilterten Mustern](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Trainieren Ihrer App nach dem Ändern des Modells mit Mustern
Nachdem Sie ein Muster hinzufügen, bearbeiten, entfernen oder neu zuweisen, [trainieren](luis-how-to-train.md) und [veröffentlichen](luis-how-to-publish-app.md) Sie Ihre App, damit die Änderungen auf Endpunktabfragen angewandt werden. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie ein Muster mit Pattern.any und Rollen [hinzufügen](luis-tutorial-pattern.md).
* Erfahren Sie, wie Sie Ihre App [trainieren](luis-how-to-train.md).