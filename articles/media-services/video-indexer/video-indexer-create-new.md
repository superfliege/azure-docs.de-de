---
title: Erstellen von Videoauswertungen aus vorhandenen Videos – Azure
titlesuffix: Azure Media Services
description: In diesem Thema wird veranschaulicht, wie Sie Erkenntnisse basierend auf vorhandenen Videodateien gewinnen und veröffentlichen.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 2f6ceeebd18a91472ee12f04c0ac8e602b05f269
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197550"
---
# <a name="create-highlights-from-existing-videos"></a>Erstellen von Highlights aus vorhandenen Videos

In diesem Thema wird veranschaulicht, wie Sie Videoinformationen (Erkenntnisse) basierend auf den Daten eines anderen Videos erstellen und veröffentlichen.

1. Navigieren Sie zur [Video Indexer](https://www.videoindexer.ai/)-Website, und melden Sie sich an.
2. Suchen Sie nach einem Video, für das Sie die Videoinformationen erstellen möchten.
3. Klicken Sie auf die Schaltfläche für die **Wiedergabe**.

    Auf der Seite werden die zusammengefassten Informationen des Videos dargestellt. 

    ![Einblicke](./media/video-indexer-create-new/video-indexer-summarized-insights.png)

3. Klicken Sie auf die Schaltfläche **Bearbeiten**.

    Auf dieser Seite wird eine vollständige Aufstellung der Daten eines Videos angezeigt. Die Aufstellung ist in einzelne Blöcke unterteilt. Die Blöcke sollen das Durchgehen der Daten vereinfachen. Die Unterteilung in Blöcke kann beispielsweise darauf basieren, dass sich der Sprecher ändert oder dass es zu einer längeren Pause kommt. Sie können Ihre eigene Wiedergabeliste erstellen, die nur die gewünschten Zeilen enthält. Wenn Sie nur bestimmte Teile des Quellvideos anzeigen möchten, können Sie nach Themen/Schlüsselwörtern, Stimmungen, Personen oder Sprechern filtern. Sie können auch angeben, dass nur das Transkript oder die OCR-Daten des Videos angezeigt werden sollen.    

    ![Einblicke](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)

4. Erstellen Sie Ihre Wiedergabeliste.

    Wählen Sie **+**/**-**, um Ihrer Wiedergabeliste Zeilen hinzuzufügen oder Zeilen daraus zu entfernen.

5. Zeigen Sie eine Vorschau Ihrer Wiedergabeliste an.

    Wählen Sie **Vorschau**, nachdem Sie die Wiedergabeliste erstellt haben.
6. Veröffentlichen Sie die Wiedergabeliste.

    Nachdem Sie die Wiedergabeliste als Vorschau angezeigt haben, können Sie sie veröffentlichen.

    Nach der Veröffentlichung der Wiedergabeliste wird sie der Liste mit Ihren Videoinformationen hinzugefügt.


## <a name="next-steps"></a>Nächste Schritte 

Nachdem Sie die neue Wiedergabeliste erstellt haben, können Sie mit ihrer Verarbeitung fortfahren, wie in einem der folgenden Themen beschrieben: 

- [Verarbeiten von Inhalten mit der Video Indexer-REST-API](video-indexer-use-apis.md)
- [Einbetten von visuellen Widgets in Ihre Anwendung](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Weitere Informationen

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion)) 
