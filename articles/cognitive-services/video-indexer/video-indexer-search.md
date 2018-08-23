---
title: Verwenden von Azure Video Indexer zum Suchen nach bestimmten Momenten in Videos | Microsoft-Dokumentation
description: In diesem Thema wird veranschaulicht, wie Sie in Videos nach bestimmten Momenten suchen können.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 1cffa067d8028adab4dbcc82c529f77d980ce6be
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397542"
---
# <a name="find-exact-moments-within-videos"></a>Suchen bestimmter Momente in Videos

In diesem Thema werden die Suchoptionen veranschaulicht, mit denen Sie in Videos nach bestimmten Momenten suchen können.

1. Melden Sie sich an Ihrem [Video Indexer](https://api-portal.videoindexer.ai/)-Konto an.
2. Sie können alle Videos Ihres Kontos durchsuchen.

    Im folgenden Beispiel wird nach allen Videos mit Scott Hanselman gesucht, die für Channel 9 erstellt wurden.

    ![Suche](./media/video-indexer-search/video-indexer-search01.png)
    
3. Durchsuchen Sie die zusammengefassten Informationen des Videos.

    Anschließend können Sie in einem Video suchen, indem Sie für das Video auf die Schaltfläche für die **Wiedergabe** klicken. Wenn Sie dann die Registerkarte **Suche** wählen, können Sie im Video suchen. Wir haben beispielsweise nach allen Stellen gesucht, an denen der Text „Identity Protection“ verwendet wird. 

    ![Suche](./media/video-indexer-search/video-indexer-search02.png)

    Wenn Sie auf ein Ergebnis klicken, gelangen Sie im Player zum entsprechenden Moment im Video. Sie können die Player-Ansicht bzw. die Ansicht für Erkenntnisse und die Synchronisierung in Ihrer Anwendung einrichten. Weitere Informationen finden Sie unter [Einbetten von Video Indexer-Widgets in Ihre Anwendungen](video-indexer-embed-widgets.md). 

    
4. Durchsuchen Sie die ausführliche Aufstellung des Videos.

    Klicken Sie auf die Schaltfläche **Bearbeiten**, wenn Sie basierend auf dem gefundenen Video Ihre eigene Aufstellung erstellen möchten. Auf dieser Seite wird eine vollständige Aufstellung der Daten eines Videos angezeigt. Sie können in der Aufstellung suchen, um nur die Zeilen anzuzeigen, die für Sie interessant sind. Weitere Informationen finden Sie unter [Anzeigen und Bearbeiten von Video Indexer-Erkenntnissen](video-indexer-view-edit.md).

    In diesem Beispiel haben wir nach dem Text „Identity Protection“ gesucht. Außerdem haben wir zusätzliche Filter angewendet. Dies ist unten im Screenshot zu sehen.

    ![Suche](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>Nächste Schritte 

Nachdem Sie das gewünschte Video gefunden haben, können Sie mit dem Verarbeiten des Videos fortfahren. Dies ist in einem dieser Themen beschrieben: 

- [Erstellen von Highlights aus vorhandenen Videos](video-indexer-create-new.md)
- [Verarbeiten von Inhalten mit der Video Indexer-REST-API](video-indexer-use-apis.md)
- [Einbetten von visuellen Widgets in Ihre Anwendung](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Weitere Informationen

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))
