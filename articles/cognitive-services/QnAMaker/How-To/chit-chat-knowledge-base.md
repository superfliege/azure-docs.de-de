---
title: Hinzufügen von Geplauder zu einer QnA Maker-Wissensdatenbank
titleSuffix: Azure Cognitive Services
description: Das Hinzufügen von persönlichem Geplauder zu Ihrem Bot macht ihn unterhaltsamer und interessanter, wenn Sie eine Wissensdatenbank erstellen. Mit QnA Maker können Sie auf einfache Weise ein vordefiniertes Dataset mit wichtigen Geplauderelementen in Ihrer Wissensdatenbank hinzufügen.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9c071d3f6ccdae13cc216fee4b1e5687b79ea5bc
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54261978"
---
# <a name="add-chit-chat-to-a-knowledge-base"></a>Hinzufügen von Geplauder zu einer Wissensdatenbank

Das Hinzufügen von Geplauder zu Ihrem Bot macht ihn unterhaltsamer und interessanter. Das Geplauderfeature von QnA Maker ermöglicht Ihnen, auf einfache Weise ein vordefiniertes Dataset mit wichtigen Geplauderelementen in Ihrer Wissensdatenbank (KB) hinzufügen. Dies kann ein Ausgangspunkt für die Persönlichkeit Ihres Bots sein und Ihnen die Zeit und die Kosten ersparen, diese Elemente von Grund auf neu zu schreiben.  

Dieses Dataset enthält etwa 100 Szenarien von Geplauder mit dem Sprachstil von drei Personas: dem Sachlichen, dem Freund und dem Komiker. Wählen Sie die Persona aus, die dem Sprachstil Ihres Bots am nächsten kommt. Bei einer Benutzerabfrage versucht QnA Maker, sie mit der nächsten bekannten Geplauder-QnA abzugleichen. 

Einige Beispiele für die verschiedenen Persönlichkeiten: <!-- added quotes so acrolinx doesn't score these sentences -->
|Benutzerabfrage|Der Sachliche|Der Freund|Der Komiker|
|--|--|--|--|
|`You are awesome`|`I aim to serve.`|`That's so nice of you!`|`Flattery. I like it.`|
|`Are you hungry?`|`I don't need to eat.`|`I only do food for thought.`|`Eating would require a lot of things I don't have. Like a digestive system. And silverware.`|
|`Sing a song`|`I'm afraid I'm not musically inclined.`|`La la la, tra la la. I'm awesome at this.`|`You can't handle my dulcet tones.`|
|`Will you marry me?`|`I think it's best if we stick to a professional relationship.`|`Aw, that's sweet.`|`Sure. Take me to city hall. See what happens.`|


> [!NOTE]
> Unterstützung für Geplauder ist zurzeit nur in englischer Sprache verfügbar. 

## <a name="add-chit-chat-during-kb-creation"></a>Hinzufügen von Geplauder während der Erstellung der Wissensdatenbank
Während der Erstellung der Wissensdatenbank besteht nach dem Hinzufügen Ihrer Quell-URLs und Dateien eine Option zum Hinzufügen von Geplauder. Wählen Sie die Persönlichkeit aus, die Sie als Grundlage für das Geplauder verwenden möchten. Wenn Sie kein Geplauder hinzufügen möchten oder bereits Unterstützung für Geplauder in Ihren Datenquellen verwenden, wählen Sie **Keine** aus. 
   
![Hinzufügen von Geplauder während der Erstellung](../media/qnamaker-how-to-chit-chat/create-kb-chit-chat.png)

## <a name="add-chit-chat-to-an-existing-kb"></a>Hinzufügen von Geplauder zu einer vorhandenen Wissensdatenbank
Wählen Sie Ihre Wissensdatenbank aus, und navigieren Sie zur Seite **Einstellungen**. Dort finden Sie einen Link zu allen Geplauderdatasets im entsprechenden **TSV**-Format. Laden Sie die gewünschte Persönlichkeit herunter, und laden Sie sie dann als Dateiquelle hoch. Achten Sie darauf, dass Sie das Format oder die Metadaten nicht bearbeiten, wenn Sie die Datei herunterladen und hochladen. 
  
![Hinzufügen von Geplauder zu einer vorhandenen Wissensdatenbank](../media/qnamaker-how-to-chit-chat/add-chit-chat-dataset.png)

## <a name="edit-your-chit-chat-questions-and-answers"></a>Bearbeiten von Geplauderfragen und -antworten
Beim Bearbeiten Ihrer Wissensdatenbank wird eine neue Quelle für Geplauder basierend auf der Persönlichkeit angezeigt, die Sie ausgewählt haben. Sie können jetzt geänderte Fragen hinzufügen oder die Antworten wie bei jeder anderen Quelle bearbeiten. 

![Bearbeiten von Geplauder-QnAs](../media/qnamaker-how-to-chit-chat/edit-chit-chat.png)

## <a name="add-additional-chit-chat-questions-and-answers"></a>Hinzufügen zusätzlicher Geplauderfragen und -antworten
Sie können neue Geplauder-QnAs hinzufügen, die nicht im vordefinierten Dataset enthalten sind. Stellen Sie sicher, dass Sie kein QnA-Paar duplizieren, das bereits im Geplauderdataset enthalten ist. Wenn Sie neue Geplauder-QnAs hinzufügen, werden diese Ihrer **redaktionellen** Quelle hinzugefügt. Um sicherzustellen, dass der Ranker versteht, dass es sich um Geplauder handelt, fügen Sie das Metadaten-Schlüssel-Wertpaar „Editorial: chit-chat“ hinzu, wie in der folgenden Abbildung gezeigt:
   
![Hinzufügen von Geplauder-QnAs](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png)

## <a name="delete-chit-chat-from-an-existing-kb"></a>Löschen von Geplauder aus einer vorhandenen Wissensdatenbank
Wählen Sie Ihre Wissensdatenbank aus, und navigieren Sie zur Seite **Einstellungen**. Ihre spezifische Geplauderquelle wird als Datei mit dem ausgewählten Persönlichkeitsnamen aufgelistet. Sie können diese als Quelldatei löschen.

![Löschen von Geplauder aus der Wissensdatenbank](../media/qnamaker-how-to-chit-chat/delete-chit-chat.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Importieren einer Knowledge Base](../Tutorials/migrate-knowledge-base.md)

## <a name="see-also"></a>Weitere Informationen 

[Übersicht über QnA Maker](../Overview/overview.md)
