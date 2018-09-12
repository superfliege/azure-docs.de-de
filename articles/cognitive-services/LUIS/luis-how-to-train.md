---
title: Trainieren Ihrer LUIS-App
titleSuffix: Azure Cognitive Services
description: Sie trainieren Ihre LUIS-App, indem Sie ihr Fähigkeiten vermitteln, mit denen sie natürliche Sprache besser verstehen kann. Trainieren Sie Ihre LUIS-App, nachdem Sie Ihr Modell aktualisiert haben, indem Sie Entitäten, Absichten und Äußerungen hinzufügen, bearbeiten, bezeichnen oder löschen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 42cff3dd8237598da5aa71ed1a4d6462c5b4c25d
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44049130"
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

3. Wenn das Training abgeschlossen ist, wird im oberen Bereich des Browsers eine Benachrichtigungsleiste in grün angezeigt.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Wenn Ihre App mindestens eine Absicht aufweist, die keine Beispieläußerungen enthält, können Sie Ihre App nicht trainieren. Fügen Sie für sämtliche Absichten Äußerungen hinzu. Weitere Informationen finden Sie unter [Add example utterances (Hinzufügen von Beispieläußerungen)](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Nächste Schritte

* [Bezeichnen von vorgeschlagenen Äußerungen mit LUIS](luis-how-to-review-endoint-utt.md) 
* [Verwenden von Features zum Verbessern der Leistung Ihrer LUIS-App](luis-how-to-add-features.md) 