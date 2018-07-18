---
title: Zuverlässigkeitsbewertung – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Erläutern der Zuverlässigkeitsbewertung
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: c97bdb7e57275ebd1893bc28248c4ecc6b35c153
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376307"
---
# <a name="confidence-score"></a>Zuverlässigkeitsbewertung

Eine Zuverlässigkeitsbewertung gibt den Grad der Übereinstimmung zwischen der Benutzerfrage und der zurückgegebenen Antwort an.

Wenn eine Benutzerabfrage mit dem Inhalt der Wissensdatenbank (Knowledge Base, KB) abgeglichen wird, kann mehr als eine Antwort zurückgegeben werden. Die Antworten werden in einer Rangfolge mit abnehmender Zuverlässigkeitsbewertung zurückgegeben.

Eine Zuverlässigkeitsbewertung liegt zwischen 0 und 100.

|Ergebniswert|Zuverlässigkeit|
|--|--|
|100|Eine genaue Übereinstimmung von Benutzerabfrage und KB-Frage|
|90|Hohe Zuverlässigkeit: Die meisten Wörter stimmen überein.|
|40 - 60|Relativ hohe Zuverlässigkeit: Wichtige Wörter stimmen überein.|
|10|Geringe Zuverlässigkeit: Wichtige Wörter stimmen nicht überein.|
|0|Keine Wortübereinstimmung|


## <a name="similar-confidence-scores"></a>Ähnliche Zuverlässigkeitsbewertungen
Wenn mehrere Antworten eine ähnliche Zuverlässigkeitsbewertung aufweisen, ist es wahrscheinlich, dass die Abfrage zu allgemein war und daher mit gleicher Wahrscheinlichkeit mit mehreren Antworten übereinstimmt. Versuchen Sie, Ihre Fragen und Antworten (Questions and Answers, QnAs) besser zu strukturieren, damit jede QnA-Entität eine eindeutige Absicht ausdrückt.


## <a name="improving-confidence-scores"></a>Verbessern der Zuverlässigkeitsbewertungen
Um die Zuverlässigkeitsbewertung einer bestimmten Antwort auf eine Benutzerabfrage zu verbessern, können Sie der Wissensdatenbank die Benutzerabfrage als alternative Frage für diese Antwort hinzufügen.
   
## <a name="confidence-score-differences"></a>Abweichungen zwischen Zuverlässigkeitsbewertungen
Die Zuverlässigkeitsbewertung einer Antwort kann zwischen der Testversion und der veröffentlichten Version der Wissensdatenbank geringfügig variieren, auch wenn der Inhalt identisch ist. Das liegt daran, dass sich die Inhalte der Test- und der veröffentlichten Wissensdatenbank in verschiedenen Azure Search-Indizes befinden.

Unter dem hier angegebenen Link erhalten Sie Informationen zur Funktionsweise des Vorgangs [Veröffentlichen](../How-To/publish-knowledge-base.md).


## <a name="no-match-found"></a>Keine Übereinstimmung gefunden
Wenn das Rangfolgemodul keine gute Übereinstimmung findet, wird die Zuverlässigkeitsbewertung „0,0“ oder „Keine“ zurückgegeben, und die Standardantwort lautet „Keine gute Übereinstimmung in der KB gefunden“. Sie können diese Standardantwort im Bot- oder Anwendungscode, der den Endpunkt aufruft, überschreiben. Alternativ können Sie die überschriebene Antwort auch in Azure festlegen, wodurch die Standardeinstellung für alle in einem bestimmten QnA Maker-Dienst bereitgestellten Wissensdatenbanken geändert wird.

1. Wechseln Sie zum [Azure-Portal](http://portal.azure.com), und navigieren Sie zu der Ressourcengruppe, die den von Ihnen erstellten QnA Maker-Dienst darstellt.

2. Klicken Sie zum Öffnen auf den **App Service**.

    ![Auf App Service zugreifen](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Klicken Sie auf **Anwendungseinstellungen**, und ändern Sie den Inhalt des Felds **DefaultAnswer** in die gewünschte Standardantwort. Klicken Sie auf **Speichern**.

    ![Standardantwort ändern](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Starten Sie Ihren App-Dienst neu.

    ![QnA Maker-App-Dienst neu starten](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Unterstützte Datenquellen](./data-sources-supported.md)

## <a name="see-also"></a>Weitere Informationen 

[Übersicht über QnA Maker](../Overview/overview.md)
