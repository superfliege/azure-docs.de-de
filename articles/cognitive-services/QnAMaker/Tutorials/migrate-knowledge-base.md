---
title: 'Migrieren von Vorschauwissensdatenbanken: Qna Maker'
titleSuffix: Azure Cognitive Services
description: Importieren einer Knowledge Base
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 6eaa4859ea6346390514325980835750d8e4b6d5
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365559"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrieren einer Knowledge Base durch Exportieren und Importieren
Auf der \\\Build\-Konferenz am 7. Mai 2018 wurde die allgemeine Verfügbarkeit von QnA Maker angekündigt. Für die allgemein verfügbare Version von QnA Maker wurde eine neue Architektur in Azure erstellt. Die mit der kostenlosen Vorschauversion von QnA Maker erstellten Knowledge Bases müssen zur allgemein verfügbaren Version von Maker migriert werden. Die Vorschauversion von QnA Maker wird im November 2018 eingestellt. Weitere Informationen zu den Änderungen in der allgemein verfügbaren Version von QnA Maker finden Sie im [Blogbeitrag](https://aka.ms/qnamakerga-blog) zur Ankündigung der allgemeinen Verfügbarkeit von QnA Maker.

QnA Maker verfügt jetzt über ein [Preismodell](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/).

Voraussetzungen
> [!div class="checklist"]
> * Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
> * Richten Sie einen neuen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md) ein.

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Migrieren einer Knowledge Base aus dem QnA Maker-Vorschauportal
1. Navigieren Sie zum [QnA Maker-Vorschauportal](https://aka.ms/qnamaker-old-portal
), und klicken Sie auf **Meine Dienste**.
2. Wählen Sie die zu migrierende Knowledge Base durch Klicken auf das Bearbeitungssymbol aus.

    ![Bearbeiten der Knowledge Base](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Klicken Sie auf **Download knowledge base** (Knowledge Base herunterladen), um eine TSV-Datei mit dem Inhalt der Knowledge Base herunterzuladen, d.h. Fragen, Antworten, Metadaten und die Namen der Datenquellen, aus denen sie extrahiert wurden.

    ![Herunterladen der Knowledge Base](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. Melden Sie sich mit Ihren Azure-Anmeldeinformationen am [QnA Maker-Portal](https://qnamaker.ai) an, und klicken Sie auf **Wissensdatenbank erstellen**.
    
5. Wenn Sie noch keinen QnA Maker-Dienst erstellt haben, wählen Sie **Create a QnA service** (QnA-Dienst erstellen) aus. Wählen Sie andernfalls einen QnA Maker-Dienst in den Dropdownlisten in Schritt 2 aus. Wählen Sie einen QnA Maker-Dienst als Host für die Knowledge Base aus.

    ![Einrichten eines QnA-Diensts](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Erstellen Sie eine leere Knowledge Base. 

    ![Festlegen von Datenquellen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Weisen Sie dem Dienst einen **Namen** zu. Doppelte Namen werden ebenso wie Sonderzeichen unterstützt.
    - Überspringen Sie den Schritt zum Hochladen von Dateien oder URLs, da Sie die Daten aus der Vorschau-Knowledge Base verwenden möchten. Sie erstellen zunächst eine leere Knowledge Base.

7. Klicken Sie auf **Erstellen**.

    ![Erstellen der Knowledge Base](../media/qnamaker-how-to-create-kb/create-kb.png)

8. Öffnen Sie in dieser neuen Knowledge Base die Registerkarte **Einstellungen**, und klicken Sie auf **Import knowledge base** (Knowledge Base importieren). Dadurch werden die Fragen, Antworten und Metadaten importiert und die Namen der Datenquellen beibehalten, aus denen sie extrahiert wurden.

   ![Importieren der Knowledge Base](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Testen** Sie die neue Knowledge Base im Testbereich. Erfahren Sie mehr über das [Testen Ihrer Knowledge Base](../How-To/test-knowledge-base.md).
10. **Veröffentlichen** Sie die Knowledge Base. Erfahren Sie mehr über das [Veröffentlichen Ihrer Knowledge Base](../How-To/publish-knowledge-base.md).
11. Verwenden Sie den folgenden Endpunkt in der Anwendung oder im Botcode. Informationen zum Erstellen eines QnA-Bots finden Sie [hier](../Tutorials/create-qna-bot.md).

    ![QnA Maker-Werte](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

Nun wird der gesamte Inhalt der Knowledge Base – Fragen, Antworten und Metadaten sowie die Namen der Quelldateien und der URLs – in die neue Knowledge Base importiert. 

## <a name="chatlogs-and-alterations"></a>Chatlogs und Varianten
Varianten (Synonyme) werden nicht automatisch importiert. Verwenden Sie die [V2-APIs](https://aka.ms/qnamaker-v2-apis), um die Varianten aus dem Vorschaustapel zu exportieren, und die [V4-APIs](https://aka.ms/qnamaker-v4-apis), um sie im neuen Stapel zu ersetzen.

Die Migration von Chatlogs ist nicht möglich, da der neue Stapel zum Speichern von Chatlogs Application Insights verwendet. Sie können die Chatlogs jedoch im [Vorschauportal](https://aka.ms/qnamaker-old-portal) herunterladen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bearbeiten einer Knowledge Base](../How-To/edit-knowledge-base.md)
