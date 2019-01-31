---
title: Erstellen einer Wissensdatenbank
titleSuffix: QnA Maker - Azure Cognitive Services
description: Verwenden Sie das QnA Maker-Portal, um eine Wissensdatenbank mit Geplauder hinzuzufügen und zu erstellen. Dadurch wird Ihre App ansprechender. Fügen Sie einen vorkonfigurierten Satz beliebter Themen als Ausgangspunkt für das Geplauder Ihres Bots hinzu, und sparen Sie damit die Zeit und Kosten, diese Elemente von Grund auf neu zu schreiben.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/11/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: cb62be34f7f294bda83920f95c9f7c7bd63cfbda
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219684"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-portal"></a>Schnellstart: Erstellen einer Wissensdatenbank über das QnA Maker-Portal

Mit QnA Maker ist es einfach, Ihre vorhandenen Datenquellen beim Erstellen einer Wissensdatenbank hinzuzufügen. Aus den folgenden Dokumenttypen können Sie eine neue QnA Maker-Wissensdatenbank erstellen:

<!-- added for scanability -->
* FAQ-Seiten
* Produkthandbücher
* Strukturierte Dokumente

Fügen Sie eine Persönlichkeit für Geplauder ein, um Ihre Informationen für Ihre Benutzer ansprechender zu machen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="create-a-new-knowledge-base"></a>Erstellen einer neuen Wissensdatenbank

1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen am [QnA Maker-Portal](https://qnamaker.ai) an, und wählen Sie **Wissensdatenbank erstellen** aus.

1. Wenn Sie noch keinen QnA Maker-Dienst erstellt haben, wählen Sie **Create a QnA service** (QnA-Dienst erstellen) aus. 

1. Wählen Sie Ihren Azure-Mandanten, den Azure-Abonnementnamen und den dem QnA Maker-Dienst zugeordneten Azure-Ressourcennamen aus den Listen in **Schritt 2** im QnA Maker-Portal aus. Wählen Sie einen Azure QnA Maker-Dienst als Host für die Wissendatenbank aus.

    ![Einrichten eines QnA-Diensts](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

1. Geben Sie den Namen Ihrer Wissensdatenbank und die Datenquellen für die neue Wissensdatenbank ein.

    ![Festlegen von Datenquellen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Weisen Sie dem Dienst einen **Namen** zu. Doppelte Namen werden ebenso wie Sonderzeichen unterstützt.
    - Fügen Sie URLs für die Daten hinzu, die extrahiert werden sollen. Weitere Informationen zu den unterstützten Ausgangstypen finden Sie [hier](../Concepts/data-sources-supported.md).
    - Laden Sie Dateien für die Daten hoch, die extrahiert werden sollen. Unter [Preise](https://aka.ms/qnamaker-pricing) erfahren Sie, wie viele Dokumente Sie hinzufügen können.
    - Wenn Sie QnAs manuell hinzufügen möchten, können Sie **Schritt 4**, der in der vorhergehenden Abbildung gezeigt wird, überspringen.

1. Fügen Sie Ihrer Wissensdatenbank **Geplauder** hinzu. Fügen Sie Geplauderunterstützung für Ihren Bot hinzu, indem Sie eine der drei Persönlichkeiten auswählen. 

    ![Hinzufügen von Geplauder zu Wissensdatenbanken ](../media/qnamaker-how-to-create-kb/create-kb-chit-chat.png)

1. Wählen Sie **Wissensdatenbank erstellen** aus.

    ![Erstellen der Knowledge Base](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Das Extrahieren der Daten dauert einige Minuten.

    ![Extraktion](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Wenn Ihre Wissensdatenbank erfolgreich erstellt wurde, werden Sie auf die Seite **Wissensdatenbank** weitergeleitet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit der Wissensdatenbank fertig sind, entfernen Sie sie im QnA Maker-Portal.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von persönlichem Geplauder](./chit-chat-knowledge-base.md)
