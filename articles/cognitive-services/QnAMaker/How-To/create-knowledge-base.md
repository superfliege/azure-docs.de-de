---
title: Erstellen einer Knowledge Base – QnA Maker – Azure Cognitive Services | Microsoft-Dokumentation
description: Erstellen einer Knowledge Base
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 93b64948ecc52feeb0f862f2b76ea898dce2333a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "35378699"
---
# <a name="create-a-knowledge-base"></a>Erstellen einer Wissensdatenbank

Mit QnA Maker ist das Integrieren vorhandener Datenquellen zum Erstellen einer Knowledge Base sehr einfach. Sie können eine neue QnA Maker-Knowledge Base basierend auf Seiten mit häufig gestellten Fragen, Produkthandbüchern oder strukturierten Dokumenten erstellen oder sie manuell hinzufügen.

## <a name="steps"></a>Schritte

1. Melden Sie sich zunächst mit Ihren Azure-Anmeldeinformationen im [QnA Maker-Portal](https://qnamaker.ai) an, und klicken Sie auf **Neuen Dienst erstellen**.

    ![Erstellen einer Knowledge Base ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Wenn Sie noch keinen QnA Maker-Dienst erstellt haben, wählen Sie **Create a QnA service** (QnA-Dienst erstellen) aus. Wählen Sie andernfalls einen QnA Maker-Dienst aus den Dropdownlisten in Schritt 2 aus. Wählen Sie einen QnA Maker-Dienst als Host für die Knowledge Base aus.

    ![Einrichten eines QnA-Diensts](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Geben Sie die folgenden Informationen ein, um die Knowledge Base zu erstellen.

    ![Festlegen von Datenquellen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Weisen Sie dem Dienst einen **Namen** zu. Doppelte Namen werden ebenso wie Sonderzeichen unterstützt.
    - Fügen Sie die URLs ein, aus denen die Extraktion erfolgen soll. Weitere Informationen zu den unterstützten Ausgangstypen finden Sie [hier](../Concepts/data-sources-supported.md).
    - Alternativ können Sie Dateien hochladen, aus denen die Daten dann extrahiert werden. Unter [Preise](https://aka.ms/qnamaker-pricing
) erfahren Sie, wie viele Dokumente Sie hinzufügen können.
    - Wenn Sie die Fragen und Antworten manuell hinzufügen möchten, können Sie das Verknüpfen von Dateien überspringen.

4. Klicken Sie auf **Erstellen**.

    ![Erstellen einer Knowledge Base](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Das Extrahieren der Daten dauert einige Minuten.

    ![Extraktion](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Wenn die Knowledge Base erfolgreich erstellt wurde, werden Sie auf die Seite der **Knowledge Base** umgeleitet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Importieren einer Knowledge Base](../Tutorials/migrate-knowledge-base.md)
