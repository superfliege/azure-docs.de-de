---
title: 'Migrieren von Wissensdatenbanken: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Informationen zum Verschieben einer Wissensdatenbank mithilfe von QnA Maker in eine neue Wissensdatenbank.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/06/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 73f355a6e8c9373a5c31dd7cfebd4455aa324302
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55809741"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrieren einer Wissensdatenbank durch Exportieren und Importieren

Die Migration einer Wissensdatenbank erfordert den Export aus einer Wissensdatenbank und den Import in eine andere. 

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
* Richten Sie einen neuen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md) ein.

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Migrieren einer Wissensdatenbank aus QnA Maker
1. Melden Sie sich beim [QnA Maker-Portal](https://qnamaker.ai) an.
1. Wählen Sie die Wissensdatenbank aus, die Sie migrieren möchten.

1. Wählen Sie auf der Seite **Settings** (Einstellungen) die Option **Wissensdatenbank exportieren** (Export knowledge base), um eine TSV-Datei mit dem Inhalt der Wissensdatenbank herunterzuladen, d.h. Fragen, Antworten, Metadaten und die Namen der Datenquellen, aus denen sie extrahiert wurden.

1. Wählen Sie im oberen Menü die Option **Create a knowledge base** (Wissensdatenbank erstellen) aus, um eine leere Wissensdatenbank zu erstellen. 

    ![Festlegen von Datenquellen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Weisen Sie dem Dienst einen **Namen** zu. Doppelte Namen werden ebenso wie Sonderzeichen unterstützt.

1. Klicken Sie auf **Erstellen**.

    ![Erstellen der Wissensdatenbank](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Öffnen Sie in dieser neuen Wissensdatenbank die Registerkarte **Einstellungen**, und klicken Sie auf **Import knowledge base** (Wissensdatenbank importieren). Dadurch werden die Fragen, Antworten und Metadaten importiert und die Namen der Datenquellen beibehalten, aus denen sie extrahiert wurden.

   ![Importieren der Wissensdatenbank](../media/qnamaker-how-to-migrate-kb/Import.png)

1. **Testen** Sie die neue Wissensdatenbank im Testbereich. Erfahren Sie mehr über das [Testen Ihrer Wissensdatenbank](../How-To/test-knowledge-base.md).
1. **Veröffentlichen** Sie die Wissensdatenbank. Erfahren Sie mehr über das [Veröffentlichen Ihrer Wissensdatenbank](../How-To/publish-knowledge-base.md).
1. Sie können den Endpunkt in Ihrer Anwendung oder im Botcode verwenden. Informationen zum Erstellen eines QnA-Bots finden Sie [hier](../Tutorials/create-qna-bot.md).

    ![QnA Maker-Werte](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

    Nun wird der gesamte Inhalt der Wissensdatenbank – Fragen, Antworten und Metadaten sowie die Namen der Quelldateien und der URLs – in die neue Wissensdatenbank importiert. 

## <a name="chat-logs-and-alterations"></a>Chatlogs und Varianten
Varianten (Synonyme) werden nicht automatisch importiert. Verwenden Sie die [V2-APIs](https://aka.ms/qnamaker-v2-apis), um die Varianten aus der alten Wissensdatenbank zu exportieren, und die [V4-APIs](https://aka.ms/qnamaker-v4-apis), um die Varianten in die neue Wissensdatenbank zu verschieben.

Die Migration von Chatlogs ist nicht möglich, da die neue Wissensdatenbank Application Insights zum Speichern von Chatlogs verwendet. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bearbeiten einer Wissensdatenbank](../How-To/edit-knowledge-base.md)
