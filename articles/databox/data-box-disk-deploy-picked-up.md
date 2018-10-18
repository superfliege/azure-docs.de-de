---
title: Zurücksenden des Microsoft Azure Data Box-Datenträgers | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Ihren Azure Data Box-Datenträger an Microsoft zurücksenden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: f971a1bed0391e809e19ff5bb0508d153319faf4
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094002"
---
# <a name="tutorial-return-azure-data-box-disk-and-verify-data-upload-to-azure"></a>Tutorial: Zurücksenden des Azure Data Box-Datenträgers und Überprüfen des Datenuploads in Azure

Dies ist das letzte Tutorial der Reihe „Bereitstellen des Azure Data Box-Datenträgers“. In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Senden des Data Box-Datenträgers an Microsoft
> * Überprüfen des Datenuploads in Azure
> * Löschen von Daten vom Data Box-Datenträger

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie das [Tutorial: Kopieren von Daten auf den Azure Data Box-Datenträger und Durchführen der Überprüfung](data-box-disk-deploy-copy-data.md) durchgearbeitet haben.

## <a name="ship-data-box-disk-back"></a>Zurücksenden des Data Box-Datenträgers

1. Trennen Sie die Datenträger vom Computer, nachdem die Überprüfung der Daten abgeschlossen ist. Entfernen Sie die Verbindungskabel.
2. Verpacken Sie die Datenträger und die Verbindungskabel in Luftpolsterfolie, und legen Sie sie in das Versandpaket.
3. Verwenden Sie das Etikett für den Rückversand in der durchsichtigen Kunststoffhülle, die am Paket angebracht ist. Falls das Etikett beschädigt oder nicht mehr vorhanden ist, können Sie über das Azure-Portal ein neues Versandetikett herunterladen und anbringen. Navigieren Sie zu **Übersicht > Versandetikett herunterladen**. 

    ![Versandetikett herunterladen](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

    Mit dieser Aktion können Sie wie unten gezeigt ein Etikett für den Rückversand herunterladen.

    ![Beispiel für Versandetikett](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)

4. Verschließen Sie das Versandpaket, und stellen Sie sicher, dass das Etikett für den Rückversand sichtbar ist.
5. Planen Sie die Abholung durch UPS, falls Sie das Gerät in den USA zurücksenden. Wenn Sie das Gerät in Europa per DHL zurücksenden, können Sie die Abholung durch DHL anfordern, indem Sie auf der DHL-Website die Luftfrachtbrief-Nummer angeben. Greifen Sie auf die DHL Express-Website für Ihr Land zu, und wählen Sie **Abholung buchen > eReturn-Abholung buchen**.

    ![eReturn-Versand mit DHL](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
    Geben Sie die Luftfrachtbrief-Nummer an, und klicken Sie auf **Abholung beauftragen**, um die Abholung anzufordern.

      ![Abholung beauftragen](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

7. Nachdem die Datenträger vom Kurierdienst abgeholt wurden, wird der Auftragsstatus im Portal in **Picked up** (Abgeholt) geändert. Außerdem wird eine Nachverfolgungs-ID angezeigt.

    ![Datenträger abgeholt](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

## <a name="verify-data-upload-to-azure"></a>Überprüfen des Datenuploads in Azure

Nachdem der Datenträger bei Microsoft eingegangen ist und gescannt wurde, wird der Auftragsstatus in **Received** (Empfangen) geändert. 

![Datenträger empfangen](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Die Daten werden automatisch kopiert, nachdem die Datenträger im Azure-Rechenzentrum mit einem Server verbunden wurden. Je nach Datengröße kann der Kopiervorgang einige Stunden oder auch einige Tage dauern. Sie können den Status des Kopiervorgangs im Portal verfolgen.

Nachdem der Kopiervorgang abgeschlossen ist, wird der Auftragsstatus in **Completed** (Abgeschlossen) geändert.

![Kopieren der Daten abgeschlossen](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Stellen Sie sicher, dass sich Ihre Daten in den Speicherkonten befinden, bevor Sie sie aus der Quelle löschen. Führen Sie die folgenden Schritte aus, um sich zu vergewissern, dass die Daten in Azure hochgeladen wurden:

1. Navigieren Sie zu dem Speicherkonto, das Ihrem Datenträgerauftrag zugeordnet ist.
2. Navigieren Sie zu **Blob-Dienst > Blobs durchsuchen**. Die Liste mit den Containern wird angezeigt. Gemäß dem Unterordner, den Sie unter den Ordnern *BlockBlob* und *PageBlob* erstellt haben, werden unter Ihrem Speicherkonto Container mit demselben Namen erstellt.
    Falls die Ordnernamen nicht den Azure-Namenskonventionen entsprechen, tritt beim Hochladen der Daten nach Azure ein Fehler auf.

4. Verwenden Sie Microsoft Azure Storage-Explorer, um zu überprüfen, ob das gesamte Dataset geladen wurde. Fügen Sie das Speicherkonto an, das dem Mietauftrag für den Datenträger entspricht, und sehen Sie sich die Liste mit den Blobcontainern an. Wählen Sie einen Container aus, und klicken Sie auf **…Mehr** und dann auf **Folder statistics** (Ordnerstatistik). Im Bereich **Aktivitäten** wird die Statistik für diesen Ordner angezeigt, z.B. die Anzahl von Blobs und die Gesamtgröße der Blobs. Die Gesamtgröße der Blobs in Byte sollte mit der Größe des Datasets übereinstimmen.

    ![Ordnerstatistik in Storage-Explorer](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Löschen von Daten vom Data Box-Datenträger

Nachdem der Kopiervorgang abgeschlossen ist und Sie überprüft haben, dass sich die Daten unter dem Azure-Speicherkonto befinden, werden die Datenträger gemäß dem NIST-Standard auf sichere Weise gelöscht. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Datenträgern erhalten, z.B.:

> [!div class="checklist"]
> * Senden des Data Box-Datenträgers an Microsoft
> * Überprüfen des Datenuploads in Azure
> * Löschen von Daten vom Data Box-Datenträger


Fahren Sie mit dem nächsten Artikel zur Vorgehensweise fort, um zu erfahren, wie Sie den Data Box-Datenträger über das Azure-Portal verwalten.

> [!div class="nextstepaction"]
> [Use Azure portal to administer Azure Data Box Disk](./data-box-portal-ui-admin.md) (Verwenden des Azure-Portals zum Verwalten des Azure Data Box-Datenträgers)


