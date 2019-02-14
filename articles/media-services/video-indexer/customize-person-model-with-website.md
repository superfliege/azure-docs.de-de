---
title: 'Verwenden der Video Indexer-Website zum Anpassen eines Personenmodells: Azure'
titlesuffix: Azure Media Services
description: In diesem Artikel wird gezeigt, wie ein Personenmodell mit der Video Indexer-Website angepasst werden kann.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: 29c32126a5bbe792dd6fe88ae189e09c511d2a22
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997056"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Anpassen eines Personenmodells mit der Video Indexer-Website

Video Indexer unterstützt Gesichtserkennung und Erkennung bekannter Personen für Videoinhalte. Die Funktion zur Erkennung von Prominenten umfasst ungefähr eine Million Gesichter, die auf häufig angeforderten Datenquellen wie IMDB, Wikipedia und den wichtigsten LinkedIn-Influencern basieren. Gesichter, die von der Funktion zur Erkennung von Prominenten nicht erkannt werden, werden erfasst, bleiben aber unbenannt. Nachdem Sie Ihr Video in Video Indexer hochgeladen und die Ergebnisse zurückerhalten haben, können Sie die Gesichter benennen, die zwar erfasst, aber nicht erkannt wurden. Sobald Sie ein Gesicht mit einem Namen versehen haben, werden Gesicht und Name dem Personenmodell Ihres Kontos hinzugefügt. Video Indexer erkennt dieses Gesicht dann in Ihren zukünftigen und früheren Videos.

Sie können die Video Indexer-Website verwenden, um Gesichter zu bearbeiten, die in einem Video erkannt wurden. Dies wird in diesem Artikel beschrieben. Sie können auch die API verwenden. Dies wird unter [Anpassen von Personenmodellen mithilfe von APIs](customize-person-model-with-api.md) beschrieben.

## <a name="edit-a-face"></a>Bearbeiten eines Gesichts

> [!NOTE]
> Namen sind für Personenmodelle eindeutig. Wenn Sie zwei verschiedenen Gesichtern denselben Namen zuweisen, betrachtet Video Indexer die Gesichter als dieselbe Person und führt sie zusammen, nachdem Sie Ihr Video neu indizieren. Wenn Sie feststellen, dass Video Indexer mehrere verschiedene Vorkommen desselben Gesichts erkannt hat, weisen Sie ihnen denselben Namen zu, und indizieren Sie Ihr Video neu.
> Sie können ein Gesicht, das von Video Indexer als Prominenten erkannt wurde, mit einem neuen Namen aktualisieren. Der neue Name, den Sie vergeben, hat Vorrang vor der integrierten Erkennung von Prominenten.

1. Navigieren Sie zur [Video Indexer](https://www.videoindexer.ai/)-Website, und melden Sie sich an.
2. Suchen Sie nach einem Video, das Sie in Ihrem Konto anzeigen und bearbeiten möchten.
3. Um ein Gesicht in Ihrem Video zu bearbeiten, wechseln Sie zur Registerkarte **Erkenntnisse**, und klicken Sie oben rechts im Fenster auf das Stiftsymbol.

    ![Anpassen des Personenmodells](./media/customize-face-model/customize-face-model.png)

4. Klicken Sie auf eines der erkannten Gesichter, und ändern Sie deren Namen von "Unbekannt #X" (oder dem Namen, der dem Gesicht zuvor zugewiesen wurde).
5. Nachdem Sie den neuen Namen eingegeben haben, klicken Sie neben dem neuen Namen auf das Prüfsymbol. Dadurch wird der neue Name gespeichert und alle Vorkommen dieses Gesichts in Ihren anderen aktuellen Videos sowie in zukünftigen Videos, die Sie hochladen, erkannt und benannt. Die Erkennung des Gesichts in Ihren anderen aktuellen Videos kann einige Zeit in Anspruch nehmen, da es sich um einen Batchprozess handelt. 

    ![Speichern der Aktualisierung](./media/customize-face-model/save-update.png)

## <a name="delete-a-face"></a>Löschen eines Gesichts

Um ein erkanntes Gesicht in Ihrem Video zu löschen, wechseln Sie zur Registerkarte **Erkenntnisse**, und klicken Sie oben rechts im Bereich auf das Stiftsymbol. Klicken Sie unter dem Namen des Gesichts auf die Option **Löschen**. Dadurch wird das erkannte Gesicht aus dem Video entfernt. Das Gesicht wird in den anderen Videos, in denen es vorkommt, weiterhin erkannt. Sie können das Gesicht aber auch aus diesen Videos löschen, nachdem sie indiziert wurden. Das Gesicht bleibt auch im Personenmodell Ihres Kontos erhalten, wenn Sie es vor dem Löschen benannt haben.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Personenmodells mit APIs](customize-person-model-with-api.md)
