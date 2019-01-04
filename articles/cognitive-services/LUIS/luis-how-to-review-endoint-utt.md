---
title: Überprüfen von Benutzeräußerungen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Das wesentliche Feature von LUIS ist das Konzept des aktiven Lernens. Sobald Ihr LUIS über Endpunktabfragen verfügt, verbessert aktives Lernen die Qualität der Ergebnisse, indem LUIS Äußerungen auswählt, bei denen es unsicher ist. Wenn Sie diese Äußerungen bezeichnen, trainieren und veröffentlichen, kann LUIS Äußerungen genauer identifizieren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 7175463001eac118d2939d3479e61a01b80ba86b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106548"
---
# <a name="how-to-review-endpoint-utterances-in-luis-portal"></a>Überprüfen von Endpunktäußerungen im LUIS-Portal

Das wesentliche Feature von LUIS ist das [Konzept](luis-concept-review-endpoint-utterances.md) des aktiven Lernens. Sobald Ihre LUIS-Instanz Endpunktabfragen aufweist, verwendet LUIS aktives Lernen, um die Qualität der Ergebnisse zu verbessern. Im Prozess des aktiven Lernens untersucht LUIS die Endpunktäußerungen und wählt Äußerungen aus, die der Dienst nicht eindeutig einordnen kann. Wenn Sie diese Äußerungen bezeichnen, trainieren und veröffentlichen, kann LUIS Äußerungen genauer identifizieren. 

## <a name="filter-utterances"></a>Filtern von Äußerungen
1. Öffnen Sie Ihre App (z.B. TravelAgent), indem Sie ihren Namen auf der Seite **Meine Apps** und dann auf der oberen Leiste **Erstellen** auswählen.

2. Wählen Sie unter **Improve app performance** (App-Leistung verbessern) die Option **Review endpoint utterances** (Endpunktäußerungen überprüfen) aus.

3. Wählen Sie auf der Seite **Review endpoint utterances** (Endpunktäußerungen überprüfen) das Textfeld **Filter list by intent or entity** (Liste nach Absicht oder Entität filtern) aus. Diese Dropdownliste enthält alle Absichten unter **INTENTS** und alle Entitäten unter **ENTITIES**.

    ![Äußerungsfilter](./media/label-suggested-utterances/filter.png)

4. Wählen Sie in der Dropdownliste eine Kategorie (Absichten oder Entitäten) aus, und überprüfen Sie die Äußerungen.

    ![Absichtsäußerungen](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Bezeichnen von Entitäten
LUIS ersetzt Entitätstoken (Wörter) durch Entitätsnamen, die blau hervorgehoben werden. Wenn eine Äußerung Entitäten ohne Beschriftung aufweist, bezeichnen Sie sie in der Äußerung. 

1. Wählen Sie die Wörter in der Äußerung aus. 

2. Wählen Sie eine Entität in der Liste aus.

    ![Bezeichnen der Entität](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Zuordnen einer einzelnen Äußerung

Für jede Äußerung wird eine vorgeschlagene Absicht in der Spalte **Aligned intent** (Zugeordnete Absicht) angezeigt. 

1. Wenn Sie diesem Vorschlag zustimmen, aktivieren Sie das Kontrollkästchen.

    ![Beibehalten der zugeordneten Absicht](./media/label-suggested-utterances/align-intent-check.png)

2. Wenn Sie mit dem Vorschlag nicht einverstanden sind, wählen Sie die richtige Absicht in der Dropdownliste der zugeordneten Absichten aus, und aktivieren Sie dann rechts neben der zugeordneten Absicht das Kontrollkästchen. 

    ![Zuordnen von Absichten](./media/label-suggested-utterances/align-intent.png)

3. Nachdem Sie das Kontrollkästchen aktiviert haben, wird die Äußerung aus der Liste entfernt. 

## <a name="align-several-utterances"></a>Zuordnen mehrerer Äußerungen

Um mehrere Äußerungen zuzuordnen, aktivieren Sie das Kontrollkästchen links neben den Äußerungen, und wählen Sie dann die Schaltfläche **Ausgewählte hinzufügen** aus. 

![Zuordnen mehrerer Elementen](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Überprüfen der zugeordneten Absicht
Sie können überprüfen, ob die Äußerung der richtigen Absicht zugeordnet wurde, indem Sie zu der Seite **Intents** (Absichten) navigieren, den Namen der Absicht auswählen und dann die Äußerungen überprüfen. Die Äußerung aus **Review endpoint utterances** (Endpunktäußerungen überprüfen) befindet sich in der Liste.

## <a name="delete-utterance"></a>Löschen einer Äußerung
Jede Äußerung kann aus der Überprüfungsliste gelöscht werden. Nach dem Löschen wird sie nicht mehr in der Liste angezeigt. Dies gilt auch dann, wenn der Benutzer die gleiche Äußerung am Endpunkt eingibt. 

Wenn Sie nicht wissen, ob Sie die Äußerung löschen sollten, verschieben Sie sie zur Absicht „None“, oder erstellen Sie eine neue Absicht, z.B. „Sonstige“, und verschieben Sie die Äußerung in diese Absicht. 

## <a name="delete-several-utterances"></a>Löschen mehrerer Äußerungen
Um mehrere Äußerungen zu löschen, wählen Sie jedes Element und dann den Papierkorb rechts neben der Schaltfläche **Ausgewählte hinzufügen** aus.

![Löschen von mehreren Elementen](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Nächste Schritte

Um zu testen, wie die Leistung nach dem Bezeichnen der vorgeschlagenen Äußerungen verbessert wird, können Sie auf die Testkonsole zugreifen, die Sie im oberen Bereich **Test** auswählen. Anleitungen zum Testen Ihrer App mit der Testkonsole finden Sie unter [Trainieren und Testen Ihrer App](luis-interactive-test.md).
