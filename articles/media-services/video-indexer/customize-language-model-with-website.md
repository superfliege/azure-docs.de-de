---
title: 'Verwenden der Video Indexer-Website zum Anpassen eines Sprachmodells: Azure'
titlesuffix: Azure Media Services
description: In diesem Artikel wird gezeigt, wie ein Sprachmodell mit der Video Indexer-Website angepasst werden kann.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/03/2018
ms.author: anzaman
ms.openlocfilehash: 9bf48a994c3efeb433bcb99342f7a477d87858eb
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53283586"
---
# <a name="customize-a-language-model-with-the-video-indexer-website"></a>Anpassen eines Sprachmodells mit der Video Indexer-Website

Mit Video Indexer können Sie benutzerdefinierte Sprachmodelle erstellen, um die Spracherkennung durch Hochladen von Anpassungstexten anzupassen, insbesondere von Texten aus dem Bereich, an dessen Vokabular sich die Engine anpassen soll. Sobald Sie Ihr Modell trainiert haben, werden neue Wörter, die im Anpassungstext vorkommen, erkannt. 

Eine detaillierte Übersicht und bewährte Methoden für benutzerdefinierte Sprachmodelle finden Sie unter [Anpassen eines Sprachmodells mit Video Indexer](customize-language-model-overview.md).

Sie können die Video Indexer-Website verwenden, um benutzerdefinierte Sprachmodelle in Ihrem Konto zu erstellen und zu bearbeiten, wie in diesem Thema beschrieben. Sie können auch die API verwenden. Dies wird unter [Anpassen von Sprachmodellen mithilfe von APIs](customize-language-model-with-api.md) beschrieben.

## <a name="create-a-language-model"></a>Erstellen eines Sprachmodells

1. Navigieren Sie zur [Video Indexer](https://www.videoindexer.ai/)-Website, und melden Sie sich an.
2. Um ein Modell in Ihrem Konto anzupassen, klicken Sie in der oberen rechten Ecke der Seite auf die Schaltfläche **Content model customization** (Anpassung des Inhaltsmodells).

   ![Anpassen eines Inhaltsmodells](./media/content-model-customization/content-model-customization.png)

3. Wählen Sie die Registerkarte **Sprache** aus.

    Eine Liste der unterstützten Sprachen wird angezeigt. 

    ![Anpassen eines Sprachmodells](./media/customize-language-model/customize-language-model.png)

4. Nach dem Auswählen der gewünschten Sprache klicken Sie auf **Modell hinzufügen**.
5. Geben Sie den Namen für das Sprachmodell ein, und drücken Sie die Eingabetaste.

    Dadurch wird das Modell erstellt, und das Hochladen von Textdateien in das Modell wird möglich.
6. Um eine Textdatei hinzuzufügen, klicken Sie auf **Datei hinzufügen**. Der Datei-Explorer wird geöffnet.

7. Navigieren Sie zur Textdatei, und wählen Sie diese aus. Sie können mehrere Textdateien einem Sprachmodell hinzufügen.

    Sie können auch eine Textdatei hinzufügen, indem Sie auf die Schaltfläche **...** auf der rechten Seite des Sprachmodells klicken und **Add file** (Datei hinzufügen) auswählen.
8. Nachdem Sie die Textdateien hochgeladen haben, wählen Sie die grüne Option **Train** (Trainieren) aus.

    ![Trainieren eines Sprachmodells](./media/customize-language-model/train-model.png)

Der Trainingsprozess kann einige Minuten dauern. Wenn der Trainingsprozess abgeschlossen ist, wird **Trained** (Trainiert) neben dem Modell angezeigt. Sie können für die Datei eine Vorschau anzeigen, sie herunterladen und aus dem Modell löschen.

![Trainiertes Sprachmodell](./media/customize-language-model/preview-model.png)

### <a name="using-a-language-model-on-a-new-video"></a>Verwenden eines Sprachmodells für ein neues Video

Um Ihr Sprachmodell für ein neues Video zu verwenden, gehen Sie folgendermaßen vor:

* Klicken Sie auf die Schaltfläche **Upload** (Hochladen) oben auf der Seite. 

    ![Hochladen](./media/customize-language-model/upload.png)
* Legen Sie Ihre Audio- oder Videodatei in dem Kreis ab, oder suchen Sie nach Ihrer Datei.

    ![Hochladen](./media/customize-language-model/upload2.png)

So können Sie die Ausgangssprache des Videos über die Option **Video source language** (Ausgangssprache Video) auswählen. Klicken Sie auf die Dropdownliste, und wählen Sie daraus ein von Ihnen erstelltes Sprachmodell aus. Ihnen sollten dann die Sprache Ihres Sprachmodells sowie der von Ihnen in Klammern festgelegte Name angezeigt werden.

Wählen Sie unten auf der Seite die Option **Upload** (Hochladen) aus, und Ihr neues Video wird mithilfe des Sprachmodells indiziert.

### <a name="using-a-language-model-to-reindex"></a>Neuindizieren mithilfe eines Sprachmodells

Um Ihr Sprachmodell zum Neuindizieren eines Videos in Ihrer Sammlung zu verwenden, navigieren Sie zu Ihren **Account videos** (Videos in Ihrem Konto) auf der [Video Indexer](https://www.videoindexer.ai/)-Homepage, und zeigen Sie auf den Namen des Videos, welches Sie erneut indizieren möchten.

Sie sehen Optionen zum Bearbeiten, Löschen und Neuindizieren Ihres Videos. Wählen Sie die Option zum Neuindizieren Ihres Videos aus.

![Neuindizieren](./media/customize-language-model/reindex1.png)

So können Sie über die Option **Video source language** (Ausgangssprache Video) die Ausgangssprache des Videos auswählen, mit der Sie Ihr Video erneut indizieren möchten. Klicken Sie auf die Dropdownliste, und wählen Sie daraus ein von Ihnen erstelltes Sprachmodell aus. Ihnen sollten dann die Sprache Ihres Sprachmodells sowie der von Ihnen in Klammern festgelegte Name angezeigt werden.

![Neuindizieren](./media/customize-language-model/reindex.png)

Klicken Sie auf die Schaltfläche **Re-index** (Neuindizieren), und Ihr Video wird mithilfe Ihres Sprachmodells erneut indiziert.

## <a name="edit-a-language-model"></a>Bearbeiten eines Sprachmodells

Sie können ein Sprachmodell bearbeiten durch Ändern seines Namens oder durch Hinzufügen und Löschen von Dateien.

Wenn Sie dem Sprachmodell Dateien hinzufügen oder daraus löschen, müssen Sie das Modell neu trainieren, indem Sie die grüne Option **Train** (Trainieren) auswählen.

### <a name="rename-the-language-model"></a>Umbenennen eines Sprachmodells

Sie können den Namen des Sprachmodells ändern, indem Sie auf die Schaltfläche **...** auf der rechten Seite des Sprachmodells klicken und die Option **Rename** (Umbenennen) auswählen. 

Geben Sie den neuen Namen ein, und drücken Sie die Eingabetaste.

### <a name="add-files"></a>Dateien hinzufügen

Klicken Sie auf **Add file** (Datei hinzufügen), um eine Textdatei hinzuzufügen. Der Datei-Explorer wird geöffnet.

Navigieren Sie zur Textdatei, und wählen Sie diese aus. Sie können mehrere Textdateien einem Sprachmodell hinzufügen.

Sie können auch eine Textdatei hinzufügen, indem Sie auf die Schaltfläche **...** auf der rechten Seite des Sprachmodells klicken und **Add file** (Datei hinzufügen) auswählen.

### <a name="delete-files"></a>Löschen von Dateien

Um eine Datei aus dem Sprachmodell zu löschen, klicken Sie auf die Schaltfläche **...** auf der rechten Seite der Textdatei, und wählen Sie **Delete** (Löschen) aus. Dies öffnet ein neues Fenster mit der Mitteilung, dass die Löschung nicht rückgängig gemacht werden kann. Wählen Sie die Option **Delete** (Löschen) in dem neuen Fenster aus.

Durch diese Aktion wird die Datei vollständig aus dem Sprachmodell entfernt.

## <a name="delete-a-language-model"></a>Löschen eines Sprachmodells

Um ein Sprachmodell aus Ihrem Konto zu löschen, klicken Sie auf die Schaltfläche **...** auf der rechten Seite des Sprachmodells, und wählen Sie **Delete** (Löschen) aus.

Dies öffnet ein neues Fenster mit der Mitteilung, dass die Löschung nicht rückgängig gemacht werden kann. Wählen Sie die Option **Delete** (Löschen) in dem neuen Fenster aus.

Durch diese Aktion wird das Sprachmodell vollständig aus Ihrem Konto entfernt. Jedes Video, das das gelöschte Sprachmodell verwendet hat, behält den gleichen Index bei, bis Sie das Video erneut indizieren. Wenn Sie das Video erneut indizieren, können Sie dem Video ein neues Sprachmodell zuweisen. Andernfalls verwendet Video Indexer das Standardmodell, um das Video erneut zu indizieren. 

## <a name="next-steps"></a>Nächste Schritte

[Customize language model using APIs (Anpassen eines Sprachmodells mithilfe von APIs)](customize-language-model-with-api.md)
