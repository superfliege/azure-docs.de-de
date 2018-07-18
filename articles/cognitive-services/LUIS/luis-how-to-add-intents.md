---
title: Hinzufügen von Absichten in LUIS-Anwendungen | Microsoft-Dokumentation
description: Verwenden Sie Language Understanding Intelligent Service (LUIS) zum Hinzufügen von Absichten, mit denen Apps Benutzeranforderungen besser verstehen und ordnungsgemäß auf sie reagieren können.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.service: cognitive-services
ms.openlocfilehash: 6e013e994a3bcb60c3104aa10cd7bad1535706f1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "35378526"
---
# <a name="manage-intents"></a>Verwalten von Absichten 
Fügen Sie Ihrer LUIS-App [Absichten](luis-concept-intent.md) hinzu, um Gruppen von Fragen oder Befehlen zu identifizieren, die den gleichen Zweck verfolgen. 

Sie verwalten Ihre Absichten auf der Seite **Intents** (Absichten), die über **Intents** im linken Bereich von LUIS verfügbar ist, und fügen sie auch dort hinzu. 

Das folgende Verfahren veranschaulicht, wie Sie die Absicht „BookFlight“ in der TravelAgent-App hinzufügen.

## <a name="add-intent"></a>Hinzufügen einer Absicht

1. Öffnen Sie die App (z.B. TravelAgent), indem Sie auf der Seite **Meine Apps** auf ihren Namen klicken, und klicken Sie dann im linken Bereich auf **Intents** (Absichten). 
2. Klicken Sie auf der Seite **Intents** (Absichten) auf **Create new Intent** (Neue Absicht erstellen).

    ![Liste der Absichten](./media/luis-how-to-add-intents/IntentsList.png)
3. Geben Sie im Dialogfeld **Create new Intent** (Neue Absicht erstellen) den Namen „BookFlight“ für die Absicht ein, und klicken Sie auf **Fertig**.

    ![Hinzufügen einer Absicht](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Auf der Seite der Absichtsdetails für die neu hinzugefügte Absicht können Sie [Äußerungen hinzufügen](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Umbenennen der Absicht

1. Klicken Sie auf der Seite **Intent** (Absicht) auf das Symbol ![Rename Intent](./media/luis-how-to-add-intents/Rename-Intent-btn.png) (Absicht umbenennen) neben dem Namen der Absicht. 

2. Auf der Seite **Intent** (Absicht) wird der Name der aktuellen Absicht in einem Dialogfeld angezeigt. Bearbeiten Sie den Namen der Absicht, und drücken Sie die EINGABETASTE. Der neue Name wird gespeichert und auf der Seite der Absicht angezeigt.

    ![Bearbeiten einer Absicht](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Löschen einer Absicht
Wenn Sie eine andere Absicht als „None“ löschen, können Sie angeben, dass alle Äußerungen der Absicht „None“ hinzugefügt werden sollen. Das ist hilfreich, wenn Sie die Äußerungen verschieben und nicht löschen müssen.   

1. Klicken Sie auf der Seite **Intent** (Absicht) auf die Schaltfläche **Delete Intent** (Absicht löschen) rechts neben dem Namen der Absicht. 

    ![Schaltfläche „Delete Intent“ (Absicht löschen)](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Klicken Sie im Bestätigungsdialogfeld auf die Schaltfläche „Ok“.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Hinzufügen einer Äußerung auf der Seite der Absicht

Geben Sie auf der Seite der Absicht im Textfeld unter ihrem Namen eine relevante Äußerung ein, die Sie von Ihren Benutzern erwarten, z.B. `book 2 adult business tickets to Paris tomorrow on Air France`, und drücken Sie dann die EINGABETASTE. 
 
>[!NOTE]
>LUIS konvertiert alle Äußerungen in Kleinbuchstaben.

![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit hervorgehobener Äußerung](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Die Äußerungen werden der Liste der Äußerungen für die aktuelle Absicht hinzugefügt. Nachdem eine Äußerung hinzugefügt wurde, [bezeichnen Sie alle Entitäten](luis-how-to-add-example-utterances.md) innerhalb der Äußerungen, und [trainieren](luis-how-to-train.md) Sie Ihre App. 

## <a name="create-a-pattern-from-an-utterance"></a>Erstellen eines Musters aus einer Äußerung
Weitere Informationen finden Sie unter [Hinzufügen eines Musters aus einer vorhandenen Äußerung auf der Seite der Absicht oder der Entität](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="edit-an-utterance-on-intent-page"></a>Bearbeiten einer Äußerung auf der Seite der Absicht

Um eine Äußerung zu bearbeiten, wählen Sie das Symbol mit den drei Punkten („...“) am rechten Ende der Zeile für diese Äußerung aus, und wählen Sie dann **Bearbeiten** aus. Ändern Sie den Text, und drücken Sie die EINGABETASTE.

![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit hervorgehobenem Symbol mit drei Punkten](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Neues Zuweisen von Äußerungen auf der Seite der Absicht
Sie können die Absicht von Äußerungen ändern, indem Sie diese einer anderen Absicht neu zuweisen. 

Um eine einzelne Äußerung einer anderen Absicht neu zuzuweisen, wählen Sie am rechten Ende der Zeile für die Äußerung den Namen der richtigen Absicht in der Spalte **Labeled Intent** (Bezeichnete Absicht) aus. Die Äußerung wird aus der Liste der Äußerungen für die aktuellen Absicht entfernt. 

![Screenshot der Seite der Absicht „BookFlight“ mit ausgewählter Absicht für eine Äußerung in der Spalte „Labeled Intent“ (Bezeichnete Absicht)](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Um die Absicht mehrerer Äußerungen zu ändern, aktivieren Sie die Kontrollkästchen links neben den Äußerungen, und wählen Sie dann **Reassign Intent** (Absicht neu zuweisen) aus. Wählen Sie die richtige Absicht in der Liste aus.

![Screenshot der Seite der Absicht „BookFlight“ mit einer aktivierten Äußerung und hervorgehobener Schaltfläche „Reassign Intent“ (Absicht neu zuweisen)](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Löschen von Äußerungen auf der Seite der Absicht

Um eine Äußerung zu löschen, wählen Sie das Symbol mit den drei Punkten („...“) am rechten Ende der Zeile für diese Äußerung aus, und wählen Sie dann **Löschen** aus. Die Äußerung wird aus der Liste und der LUIS-App entfernt.

![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit hervorgehobener Option „Löschen“](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

So löschen Sie mehrere Äußerungen

1. Aktivieren Sie die Kontrollkästchen links neben den Äußerungen, und wählen Sie dann **Delete utterances(s)** (Äußerungen löschen) aus. 

    ![Screenshot der Seite „Intents details“ (Details zu den Absichten) mit aktivierten Äußerungen und hervorgehobener Schaltfläche „Delete utterances(s)“ (Äußerungen löschen)](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Wählen Sie im Popupdialogfeld **Delete utterances?** (Äußerungen löschen?) die Option **Fertig** aus.

## <a name="search-in-utterances-on-intent-page"></a>Suchen in Äußerungen auf der Seite der Absicht
Sie können in einer Absicht nach Äußerungen suchen, die Text (Wörter oder Ausdrücke) enthalten. Beispielsweise fällt Ihnen möglicherweise ein Fehler im Zusammenhang mit einem bestimmten Wort auf, und Sie möchten alle Beispiele suchen, die dieses Wort enthalten. 

1. Wählen Sie auf der Symbolleiste das Lupensymbol aus.

    ![Screenshot der Seite „Intents“ (Absichten) mit hervorgehobenem Lupensymbol](./media/luis-how-to-add-intents/magnifying-glass.png)

2. Ein Suchtextfeld wird angezeigt. Geben Sie im Suchfeld in der rechten oberen Ecke der Liste der Äußerungen ein Wort oder einen Ausdruck ein. Die Liste der Äußerungen wird aktualisiert, sodass nur noch die Äußerungen angezeigt werden, die den Suchtext enthalten. 

    ![Screenshot der Seite „Intents“ (Absichten) mit hervorgehobenem Suchtextfeld](./media/luis-how-to-add-intents/search-textbox.png)

    Um die Suche abzubrechen und die vollständige Liste der Äußerungen wiederherzustellen, löschen Sie den eingegebenen Suchtext. Um das Suchtextfeld zu schließen, wählen Sie das Lupensymbol auf der Symbolleiste erneut aus.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Fehler durch Vorhersagediskrepanz auf der Seite der Absicht
Eine Äußerung in einer Absicht kann eine Diskrepanz zwischen der ausgewählten Absicht und dem Vorhersageergebnis aufweisen. LUIS gibt diese Diskrepanz durch ein rotes Feld um das Ergebnis an. 

![Screenshot der Seite der Absicht „BookFlight“ mit hervorgehobener Diskrepanz im Vorhersageergebnis](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Filtern nach Diskrepanzfehlern für die Absichtsvorhersage auf der Seite der Absicht
Um die Liste der Äußerung so zu filtern, dass nur Äußerungen mit einer Diskrepanz in der Absichtsvorhersage angezeigt werden, schalten Sie auf der Symbolleiste von **Alle anzeigen** zu **Nur Fehler** um. 

## <a name="filter-by-entity-type-on-intent-page"></a>Filtern nach Entitätstyp auf der Seite der Absicht
Verwenden Sie die Dropdownliste **Entity filters** (Entitätsfilter) auf der Symbolleiste, um die Äußerungen nach Entität zu filtern. 

![Screenshot der Seite „Intents“ (Absichten) mit hervorgehobenem Entitätstypfilter](./media/luis-how-to-add-intents/filter-by-entities.png) 

Um den Filter zu entfernen, wählen Sie das blaue Filterfeld mit diesem Wort oder diesem Ausdruck unter der Symbolleiste aus.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Wechseln zur Tokenansicht auf der Seite der Absicht
Wechseln Sie zur **Tokens View** (Tokenansicht), um die Token anstelle der Entitätstypnamen anzuzeigen. Sie können auch **STRG+E** drücken, um die Ansicht umzuschalten. 

![Screenshot der Seite der Absicht „BookFlight“ mit hervorgehobener „Token View“ (Tokenansicht)](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Trainieren der App nach dem Ändern des Modells mit Absichten
Nachdem Sie Absichten hinzufügen, bearbeiten oder entfernen, [trainieren](luis-how-to-train.md) und [veröffentlichen](PublishApp.md) Sie Ihre App, damit die Änderungen auf Endpunktabfragen angewandt werden. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihrer App Absichten hinzugefügt haben, besteht Ihre nächste Aufgabe darin, [Beispieläußerungen](luis-how-to-add-example-utterances.md) für die hinzugefügten Absichten hinzuzufügen. 
