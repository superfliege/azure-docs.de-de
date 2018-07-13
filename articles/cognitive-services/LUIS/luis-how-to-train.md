---
title: Trainieren der LUIS-App – Azure | Microsoft-Dokumentation
description: Verwenden Sie Language Understanding Intelligent Service (LUIS), um Ihr Modell zu trainieren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 8ec731ee2110b21a35d76d53fae4a1a6756fb5b8
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951813"
---
# <a name="train-your-luis-app"></a>Trainieren Ihrer LUIS-App

Sie trainieren Ihre LUIS-App, indem Sie ihr Fähigkeiten vermitteln, mit denen sie natürliche Sprache besser verstehen kann. Trainieren Sie Ihre LUIS-App, nachdem Sie Ihr Modell aktualisiert haben, indem Sie Entitäten, Absichten und Äußerungen hinzufügen, bearbeiten, bezeichnen oder löschen. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Das Trainieren und [Testen](luis-concept-test.md) eine App ist ein iterativer Vorgang. Nachdem Sie Ihre LUIS-App trainiert haben, sollten Sie sie mit Beispieläußerungen testen, um festzustellen, ob die Absichten und Entitäten ordnungsgemäß erkannt werden. Ist dies nicht der Fall, aktualisieren Sie die LUIS-App, und trainieren und testen Sie sie erneut. 

## <a name="train-your-app"></a>Trainieren Ihrer App
Sie müssen Ihre LUIS-App mindestens einmal trainieren, damit Sie den iterativen Prozess starten können. Vergewissern Sie sich vor dem Training, dass jeder Absicht mindestens eine Äußerung zugeordnet ist.

1. Greifen Sie auf Ihre App zu, indem Sie ihren Namen auf der Seite **Meine Apps** auswählen. 

2. Klicken Sie in Ihrer App im oberen Bereich auf **Trainieren**. 

    ![Schaltfläche „Trainieren“](./media/luis-how-to-train/train-button.png)

3. Wenn das Training abgeschlossen ist, wird im oberen Bereich des Browsers eine Benachrichtigungsleiste in grün angezeigt.

    ![Seite „Train & Test App“ (App trainieren und testen)](./media/luis-how-to-train/train-success.png)

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Wenn Ihre App mindestens eine Absicht aufweist, die keine Beispieläußerungen enthält, können Sie Ihre App nicht trainieren. Fügen Sie für sämtliche Absichten Äußerungen hinzu. Weitere Informationen finden Sie unter [Add example utterances (Hinzufügen von Beispieläußerungen)](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Nächste Schritte

* [Label suggested utterances with LUIS (Bezeichnen von vorgeschlagenen Äußerungen mit LUIS)](luis-how-to-review-endoint-utt.md) 
* [Use features to improve your LUIS app's performance (Verwenden von Features zum Verbessern der Leistung Ihrer LUIS-App)](luis-how-to-add-features.md) 