---
title: Schnellstart für Microsoft Azure Data Box-Datenträger | Microsoft-Dokumentation
description: Verwenden Sie diese Schnellstartanleitung, um Ihren Azure Data Box-Datenträger schnell im Azure-Portal bereitzustellen.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to quickly deploy Data Box Disk so as to import data into Azure.
ms.openlocfilehash: 20dc414c5cdd309434ba53acf2d7f6716d3edfe5
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009925"
---
# <a name="quickstart-deploy-azure-data-box-disk-using-the-azure-portal-preview"></a>Schnellstart: Bereitstellen des Azure Data Box-Datenträgers über das Azure-Portal (Vorschauversion)

In dieser Schnellstartanleitung wird beschrieben, wie Sie den Azure Data Box-Datenträger über das Azure-Portal bereitstellen. In den Schritten erfahren Sie, wie Sie schnell einen Auftrag erstellen, Datenträger erhalten, diese auspacken und anschließen und Daten auf die Datenträger kopieren, damit diese in Azure hochgeladen werden können. 

Eine ausführliche Anleitung zur Schritt-für-Schritt-Bereitstellung und zur Nachverfolgung finden Sie unter [Tutorial: Bestellen eines Azure Data Box-Datenträgers (Vorschau)](data-box-disk-deploy-ordered.md). 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) erstellen.

> [!IMPORTANT]
> Data Box-Datenträger befinden sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Vorbereitungen

- Stellen Sie sicher, dass Ihr Abonnement für den Azure Data Box-Dienst aktiviert ist. [Sie müssen sich für den Dienst registrieren](http://aka.ms/azuredataboxfromdiskdocs), um das Abonnement für diesen Dienst zu aktivieren.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [http://aka.ms/azuredataboxfromdiskdocs](http://aka.ms/azuredataboxfromdiskdocs) beim Azure-Portal an.

## <a name="order"></a>Reihenfolge

Dieser Schritt dauert ungefähr fünf Minuten.

1. Erstellen Sie im Azure-Portal eine neue Azure Data Box-Ressource. 
2. Wählen Sie ein Abonnement aus, das für diesen Dienst aktiviert ist, und wählen Sie als Übertragungstyp **Import** aus. Geben Sie unter **Quellland/-region** an, wo sich die Daten befinden, und legen Sie die **Azure-Zielregion** für die Datenübertragung fest.
3. Wählen Sie **Data Box-Datenträger** aus. Die maximale Kapazität der Lösung beträgt 35 TB, aber Sie können auch mehrere Aufträge für Datenträger erstellen, um weitere Datenträger anzufordern und höhere Datenmengen abzudecken.  
4. Geben Sie die Auftragsdetails und Versandinformationen ein. Wenn der Dienst in Ihrer Region verfügbar ist, können Sie E-Mail-Adressen für Benachrichtigungen angeben, die Zusammenfassung prüfen und anschließend den Auftrag erstellen. 

Nachdem der Auftrag erstellt wurde, werden die Datenträger für den Versand vorbereitet. 


## <a name="unpack"></a>Auspacken

Dieser Schritt dauert ungefähr fünf Minuten.

Die Data Box-Datenträger werden per UPS Express-Paket verschickt. Öffnen Sie das Paket, und vergewissern Sie sich, dass darin Folgendes enthalten ist:

- 1 bis 5 USB-Datenträger in Luftpolsterfolie
- Ein Verbindungskabel pro Datenträger 
- Versandetikett für die Rücksendung
 

## <a name="connect-and-unlock"></a>Anschließen und Entsperren

Dieser Schritt dauert ungefähr fünf Minuten.

1. Verwenden Sie das beigefügte Kabel, um den Datenträger mit einem Windows-Computer zu verbinden, auf dem eine unterstützte Version ausgeführt wird. Weitere Informationen zu unterstützten Betriebssystemversionen finden Sie unter den [Systemanforderungen für Azure Data Box-Datenträger](data-box-disk-system-requirements.md). 
2. Entsperren Sie den Datenträger wie folgt:

    1. Wechseln Sie im Azure-Portal zu **Allgemein > Gerätedetails**, und rufen Sie den Hauptschlüssel ab.
    2. Laden Sie das Tool zum Entsperren von Data Box-Datenträgern herunter, und extrahieren Sie es auf dem Computer, den Sie zum Kopieren der Daten auf die Datenträger verwenden möchten. 
    3. Führen Sie *DataBoxDiskUnlock.exe* aus, und geben Sie den Hauptschlüssel ein. Wiederholen Sie diesen Schritt für alle zukünftigen Datenträgeraktivierungen dieser Art.
    4. Der Laufwerkbuchstabe, der dem Datenträger zugewiesen ist, wird vom Tool angezeigt. Notieren Sie sich den Laufwerkbuchstaben des Datenträgers. Er wird in den nachfolgenden Schritten verwendet.



## <a name="copy-data-and-verify"></a>Kopieren der Daten und Durchführen der Überprüfung

Der Zeitraum, der für die Durchführung dieses Vorgangs erforderlich ist, hängt von Ihrer Datengröße ab. 

1. Das Laufwerk enthält die Ordner *PageBlob*, *BlockBlob* und *AzureImportExport*. Kopieren Sie die zu importierenden Daten per Drag & Drop als Blockblobs in den Ordner *BlockBlob*. Ziehen Sie Daten, z.B. von VHD/VHDX, entsprechend per Drag & Drop in den Ordner *PageBlob*.

    Im Azure-Speicherkonto wird für jeden Unterordner der Ordner *BlockBlob* und *PageBlob* ein Container erstellt. Alle Dateien in den Ordnern *BlockBlob* und *PageBlob* werden in den Standardcontainer `$root` unter dem Azure Storage-Konto kopiert.

    > [!NOTE] 
    > - Für alle Container und Blobs sollten die [Azure-Namenskonventionen](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions) eingehalten werden. Wenn diese Regeln nicht befolgt werden, tritt beim Datenupload in Azure ein Fehler auf.
    > - Stellen Sie sicher, dass Dateien für Blockblobs eine Größe von ca. 4,7 TiB und für Seitenblobs eine Größe von ca. 8 TiB nicht überschreiten.

2. (Optional) Nachdem der Kopiervorgang abgeschlossen ist, ist es ratsam, die im Ordner *AzureImportExport* enthaltene Datei `AzureExpressDiskService.ps1` auszuführen, um Prüfsummen für die Validierung zu generieren. Je nach Datengröße kann dieser Schritt relativ lange dauern. 
3. Trennen Sie die Kabelverbindung des Laufwerks mit dem Computer. 


## <a name="ship-to-azure"></a>Senden an Azure

Dieser Schritt dauert ungefähr 5 bis 7 Minuten.

1. Legen Sie alle Datenträger zusammen in das Originalpaket. Verwenden Sie das beigefügte Versandetikett. Falls das Etikett beschädigt oder nicht mehr vorhanden ist, können Sie es über das Portal herunterladen. Wechseln Sie zu **Übersicht**, und klicken Sie in der Befehlsleiste auf **Versandetikett herunterladen**.
2. Geben Sie das sicher verschlossene Paket beim Paketdienst ab.  

Der Data Box-Dienst sendet eine E-Mail-Benachrichtigung und aktualisiert den Auftragsstatus im Azure-Portal.


## <a name="verify-your-data"></a>Überprüfen Ihrer Daten

Der Zeitraum, der für die Durchführung dieses Vorgangs erforderlich ist, hängt von Ihrer Datengröße ab.

1. Wenn der Data Box-Datenträger mit dem Netzwerk des Azure-Rechenzentrums verbunden wird, beginnt der Upload der Daten in Azure automatisch. 
2. Sie werden vom Azure Data Box-Dienst über das Azure-Portal benachrichtigt, dass der Kopiervorgang für die Daten abgeschlossen ist. 
    
    1. Prüfen Sie die Fehlerprotokolle auf Fehler, und ergreifen Sie ggf. entsprechende Maßnahmen.
    2. Stellen Sie sicher, dass sich Ihre Daten in den Speicherkonten befinden, bevor Sie sie aus der Quelle löschen.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Dieser Schritt dauert zwei bis drei Minuten.

Sie können den Data Box-Auftrag stornieren und dann löschen, um die Bereinigung durchzuführen.

- Im Azure-Portal können Sie den Data Box-Auftrag stornieren, bevor er verarbeitet wurde. Nachdem der Auftrag verarbeitet wurde, kann er nicht mehr storniert werden. Der Auftrag wird abgearbeitet, bis er den Status „Abgeschlossen“ erreicht hat. 

    Navigieren Sie zum Stornieren des Auftrags zu **Übersicht**, und klicken Sie in der Befehlsleiste auf **Stornieren**.  

- Sie können einen Auftrag löschen, wenn im Azure-Portal dafür der Status **Abgeschlossen** oder **Abgebrochen** angezeigt wird. 

    Navigieren Sie zum Löschen des Auftrags zu **Übersicht**, und klicken dann in der Befehlsleiste auf **Löschen**.

## <a name="next-step"></a>Nächster Schritt

In dieser Schnellstartanleitung haben Sie einen Azure Data Box-Datenträger bereitgestellt, über den Ihre Daten in Azure importiert werden. Fahren Sie mit dem folgenden Tutorial fort, um sich weiter über die Verwaltung von Azure Data Box-Datenträgern zu informieren: 

> [!div class="nextstepaction"]
> [Use the Azure portal to administer Data Box Disk](data-box-portal-ui-admin.md) (Verwenden des Azure-Portals zum Verwalten des Data Box-Datenträgers)


