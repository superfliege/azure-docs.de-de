---
title: 'Erstellen einer Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Das Hinzufügen von Geplauder zu Ihrem Bot macht ihn unterhaltsamer und interessanter. Mit QnA Maker können Sie auf einfache Weise ein vordefiniertes Dataset mit wichtigen Geplauderelementen in Ihrer Wissensdatenbank hinzufügen. Dies kann ein Ausgangspunkt für das Geplauder Ihres Bots sein und Ihnen die Zeit und die Kosten ersparen, diese Elemente von Grund auf neu zu schreiben.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: c9d36014bc364d8b016221e6b9ff380b0bd4b8ed
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854166"
---
# <a name="create-a-knowledge-base"></a>Erstellen einer Wissensdatenbank

Mit QnA Maker ist es einfach, Ihre vorhandenen Datenquellen beim Erstellen einer Wissensdatenbank hinzuzufügen. Aus den folgenden Dokumenttypen können Sie eine neue QnA Maker-Wissensdatenbank erstellen:

<!-- added for scanability -->
* FAQ-Seiten
* Produkthandbücher
* Strukturierte Dokumente

## <a name="steps"></a>Schritte

1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen am [QnA Maker-Portal](https://qnamaker.ai) an, und wählen Sie **Wissensdatenbank erstellen** aus.

2. Wenn Sie noch keinen QnA Maker-Dienst erstellt haben, wählen Sie **Create a QnA service** (QnA-Dienst erstellen) aus. 

3. Wählen Sie Ihren Azure-Mandanten, den Azure-Abonnementnamen und den dem QnA Maker-Dienst zugeordneten Azure-Ressourcennamen aus den Listen in **Schritt 2** im QnA Maker-Portal aus. Wählen Sie einen Azure QnA Maker-Dienst als Host für die Wissendatenbank aus.

    ![Einrichten eines QnA-Diensts](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

4. Geben Sie den Namen Ihrer Wissensdatenbank und die Datenquellen für die neue Wissensdatenbank ein.

    ![Festlegen von Datenquellen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Weisen Sie dem Dienst einen **Namen** zu. Doppelte Namen werden ebenso wie Sonderzeichen unterstützt.
    - Fügen Sie URLs für die Daten hinzu, die extrahiert werden sollen. Weitere Informationen zu den unterstützten Ausgangstypen finden Sie [hier](../Concepts/data-sources-supported.md).
    - Laden Sie Dateien für die Daten hoch, die extrahiert werden sollen. Unter [Preise](https://aka.ms/qnamaker-pricing) erfahren Sie, wie viele Dokumente Sie hinzufügen können.
    - Wenn Sie QnAs manuell hinzufügen möchten, können Sie **Schritt 4**, der in der vorhergehenden Abbildung gezeigt wird, überspringen.

5. Fügen Sie Ihrer Wissensdatenbank **Geplauder** hinzu. Fügen Sie Geplauderunterstützung für Ihren Bot hinzu, indem Sie eine der 3 vordefinierten Persönlichkeiten auswählen. 

    <!-- TBD: add back in when chit chat how-to is merged
    ![Add chit-chat to KB ](../media/qnamaker-how-to-chitchat/create-kb-chit-chat.png)
    -->

6. Wählen Sie **Wissensdatenbank erstellen** aus.

    ![Erstellen der Knowledge Base](../media/qnamaker-how-to-create-kb/create-kb.png)

7. Das Extrahieren der Daten dauert einige Minuten.

    ![Extraktion](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

8. Wenn Ihre Wissensdatenbank erfolgreich erstellt wurde, werden Sie auf die Seite **Wissensdatenbank** weitergeleitet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von persönlichem Geplauder](./chit-chat-knowledge-base.md)
