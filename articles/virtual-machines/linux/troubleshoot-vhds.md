---
title: "Problembehandlung für angefügte VHDs auf virtuellen Azure Linux-Computern | Microsoft-Dokumentation"
description: "Behandeln von Problemen wie unerwarteten Neustarts von virtuellen Linux-Computern oder Problemen beim Löschen eines Speicherkontos mit angefügten VHDs"
keywords: SSH-Verbindung abgelehnt, SSH-Fehler, Azure SSH, SSH-Verbindungsfehler
services: virtual-machines-linux
author: iainfoulds
manager: jeconnoc
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/28/2018
ms.author: iainfou
ms.openlocfilehash: 10f5e2f4a48d335c6b7d05b5eb5b577765675c0e
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="troubleshoot-attached-vhds-on-azure-linux-virtual-machines"></a>Problembehandlung für angefügte VHDs auf virtuellen Azure Linux-Computern

Virtuelle Azure-Computer nutzen virtuelle Festplatten (VHDs) als Betriebssystemdatenträger und angefügte Datenträger. VHDs werden als Seitenblobs in einem oder mehreren Azure Storage-Konten gespeichert. Dieser Artikel beschreibt die Behandlung allgemeiner Probleme, die bei VHDs auftreten können. 

  * [Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs]
  * [Speicherbezogene Fehler beim Löschen in der Resource Manager-Bereitstellung]

## <a name="you-are-experiencing-unexpected-reboots"></a>Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs

Wenn ein virtueller Azure-Computer über eine große Anzahl von angeschlossenen VHDs verfügt, die sich im gleichen Speicherkonto befinden, können die Skalierbarkeitsziele für ein einzelnes Speicherkonto überschritten werden. Dies führt zu einem Ausfall des virtuellen Computers. Überprüfen Sie die Minutenmetriken für das Speicherkonto (**TotalRequests**/**TotalIngress**/**TotalEgress**) auf Spitzen, die die Skalierbarkeitsziele für ein Speicherkonto überschreiten. Unterstützung bei der Ermittlung, ob in Ihrem Speicherkonto eine Drosselung stattgefunden hat, erhalten Sie im Abschnitt „[Metriken zeigen Anstieg bei PercentThrottlingError an]“.

In der Regel wird jeder einzelne Eingabe- oder - oder Ausgabevorgang auf einer VHD von einem virtuellen Computer in **Get Page**- oder **Put Page**-Vorgänge des zugrunde liegenden Seitenblobs übersetzt. Daher können Sie die geschätzten IOPS für Ihre Umgebung verwenden, um zu bestimmen, über wie viele virtuelle Festplatten Sie in einem einzigen Speicherkonto auf Grundlage des spezifischen Anwendungsverhaltens verfügen können. Microsoft empfiehlt höchstens 40 Datenträger in einem Speicherkonto. Unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../../storage/common/storage-scalability-targets.md) finden Sie nähere Informationen über Skalierbarkeitsziele für Speicherkonten, insbesondere die Gesamtanforderungsrate und die Gesamtbandbreite für den Speicherkontotyp, den Sie verwenden.

Wenn Sie die Skalierbarkeitsziele für Ihr Speicherkonto überschreiten, platzieren Sie die virtuellen Festplatten in mehreren Speicherkonten, um die Aktivität in den einzelnen Konten zu reduzieren.

## <a name="storage-delete-errors-in-rm"></a>Speicherbezogene Fehler beim Löschen in der Resource Manager-Bereitstellung

Dieser Abschnitt enthält Anleitungen zur Behandlung der folgenden Fehler, die auftreten können, wenn Sie versuchen, ein Azure-Speicherkonto, einen Container oder ein Blob in einer Azure Resource Manager-Bereitstellung zu löschen.

>**Fehler beim Löschen von Speicherkonto 'StorageAccountName'. Fehler: Das Speicherkonto kann nicht gelöscht werden, da zugehörige Artefakte gerade verwendet werden.**

>**Fehler beim Löschen von # von # Containern:<br>vhds: Derzeit ist eine Lease für den Container vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.**

>**Fehler beim Löschen von # von # Blobs:<br>BlobName.vhd: Derzeit ist eine Lease für den Blob vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.**

Die auf virtuellen Azure-Computern verwendeten VHDs sind VHD-Dateien, die als Seitenblobs in einem Standard- oder Premium-Speicherkonto in Azure gespeichert sind.  Weitere Informationen zu Azure-Datenträgern finden Sie [hier](../../virtual-machines/windows/about-disks-and-vhds.md). Azure verhindert das Löschen eines an eine VM angefügten Datenträgers, um eine Beschädigung zu vermeiden. Zudem verhindert es das Löschen von Containern und Speicherkonten mit einem Seitenblob, das an einen virtuellen Computer angefügt ist. 

Der Prozess zum Löschen eines Speicherkontos, Containers oder Blobs bei einem dieser Fehler umfasst folgende Schritte: 
1. [Identifizieren von an eine VM angefügten Blobs](#step-1-identify-blobs-attached-to-a-vm)
2. [Löschen von VMs mit angefügtem **Betriebssystemdatenträger**](#step-2-delete-vm-to-detach-os-disk)
3. [Trennen aller **Datenträger** von verbleibenden VMs](#step-3-detach-data-disk-from-the-vm)

Wenn Sie diese Schritte durchgeführt haben, versuchen Sie erneut, ein Speicherkonto, einen Container bzw. ein Blob zu löschen.

### <a name="step-1-identify-blob-attached-to-a-vm"></a>Schritt 1: Identifizieren von an eine VM angefügten Blobs

#### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Szenario 1: Löschen eines Blobs – Identifizieren einer angefügten VM
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus. Wechseln Sie zum Speicherkonto, wählen Sie unter **Blobdienst** die Option **Container** aus, und navigieren Sie zum Blob, das gelöscht werden soll.
3. Wenn das Blob als **Leasezustand** den Zustand **Geleast** aufweist, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Metadaten bearbeiten**, um den Bereich „Blobmetadaten“ zu öffnen. 

    ![Screenshot des Portals, in dem die Speicherkontoblobs und die per Rechtsklick aufgerufene Option „Metadaten bearbeiten“ hervorgehoben sind](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Überprüfen und notieren Sie im Bereich „Blobmetadaten“ den Wert für **MicrosoftAzureCompute_VMName**. Dieser Wert ist der Name der VM, der die VHD angefügt ist. (Wenn dieses Feld nicht vorhanden ist, siehe **Wichtig**.)
5. Überprüfen und notieren Sie im Bereich „Blobmetadaten“ den Wert für **MicrosoftAzureCompute_DiskType**. Mit diesem Wert wird ermittelt, ob es sich beim angefügten Datenträger um einen Betriebssystemdatenträger oder Datenträger handelt (wenn dieses Feld nicht vorhanden ist. siehe **Wichtig**). 

     ![Screenshot des Portals, in dem der Speicherbereich „Blobmetadaten“ geöffnet ist](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Wenn der Blobdatenträger vom Typ **OSDisk** ist, führen Sie [Schritt 2: Löschen einer VM beim Trennen eines Betriebssystemdatenträgers](#step-2-delete-vm-to-detach-os-disk) durch. Wenn der Blobdatenträger hingegen vom Typ **DataDisk** ist, befolgen Sie die Anweisungen unter [Schritt 3: Trennen eines Datenträgers von der VM](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Wenn **MicrosoftAzureCompute_VMName** und **MicrosoftAzureCompute_DiskType** nicht in den Blobmetadaten angezeigt werden, bedeutet dies, dass das Blob explizit geleast und nicht an eine VM angefügt ist. Geleaste Blobs können erst gelöscht werden, wenn das Lease abgebrochen wurde. Um ein Lease abzubrechen, klicken Sie mit der rechten Maustaste auf das Blob, und wählen Sie **Lease abbrechen** aus. Geleaste Blobs, die nicht an einen virtuellen Computer angefügt sind, verhindern das Löschen des Blobs, jedoch nicht das Löschen eines Containers oder Speicherkontos.

#### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Szenario 2: Löschen eines Containers – Identifizieren aller Blobs in einem an VMs angefügten Container
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus. Wechseln Sie zum Speicherkonto, wählen Sie unter **Blobdienst** die Option **Container** aus, und machen Sie den Container ausfindig, der gelöscht werden soll.
3. Klicken Sie darauf, um den Container zu öffnen. Daraufhin wird die Liste der darin enthaltenen Blobs angezeigt. Machen Sie in dieser Liste alle Blobs vom Blobtyp **Seitenblob** und mit dem Leasezustand **Geleast** ausfindig. Führen Sie die Schritte von [Szenario 1](#step-1-identify-blobs-attached-to-a-vm) durch, um die mit diesen Blobs verknüpfte VM zu identifizieren.

    ![Screenshot des Portals, in dem die Speicherkontoblobs und die Option „Leasezustand“ mit dem Wert „Geleast“ hervorgehoben sind](./media/troubleshoot-vhds/utd-disks-sm.png)

4. Führen Sie [Schritt 2](#step-2-delete-vm-to-detach-os-disk) und [Schritt 3](#step-3-detach-data-disk-from-the-vm) durch, um VMs mit Datenträgern vom Typ **OSDisk** zu löschen und Datenträger vom Typ **DataDisk** zu trennen. 

#### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Szenario 3: Löschen eines Speicherkontos – Identifizieren aller Blobs in einem an VMs angefügten Speicherkonto
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus. Wechseln Sie zum Speicherkonto, und wählen Sie unter **Blobdienst** die Option **Container** aus.

    ![Screenshot des Portals, in dem die Speicherkontocontainer und die Option „Leasezustand“ mit dem Wert „Geleast“ hervorgehoben sind](./media/troubleshoot-vhds/utd-containers-sm.png)

3. Identifizieren Sie im Bereich **Container** alle Container, die als **Leasezustand** den Wert **Geleast** aufweisen, und führen Sie für jeden Container mit dem Zustand **Geleast** die Schritte von [Szenario 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) durch.
4. Führen Sie [Schritt 2](#step-2-delete-vm-to-detach-os-disk) und [Schritt 3](#step-3-detach-data-disk-from-the-vm) durch, um VMs mit Datenträgern vom Typ **OSDisk** zu löschen und Datenträger vom Typ **DataDisk** zu trennen. 

### <a name="step-2-delete-vm-to-detach-os-disk"></a>Schritt 2: Löschen einer VM beim Trennen eines Betriebssystemdatenträgers
Wenn es sich bei der VHD um einen Betriebssystemdatenträger handelt, müssen Sie zuerst die VM löschen, bevor Sie die angefügte VHD löschen können. Für Datenträger, die an dieselbe VM angefügt sind, sind keine weiteren Aktionen erforderlich, nachdem Sie die folgenden Schritte durchgeführt haben:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü „Hub“ die Option **Virtuelle Computer** aus.
3. Wählen Sie die VM aus, an die die VHD angefügt ist.
4. Stellen Sie sicher, dass der virtuelle Computer nicht aktiv verwendet wird und dass Sie den virtuellen Computer nicht mehr benötigen.
5. Wählen Sie oben im Bereich **Details zum virtuellen Computer** die Option **Löschen** aus, und klicken Sie zur Bestätigung auf **Ja**.
6. Die VM sollte nun gelöscht, doch die VHD noch vorhanden sein. Die VHD sollte nicht mehr an eine VM angefügt bzw. es sollte keine Lease mehr für diese vorhanden sein. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Um sich zu vergewissern, dass das Lease aufgehoben wurde, navigieren Sie zum Blobspeicherort, und prüfen Sie im Bereich **Blobeigenschaften**, ob der **Leasezustand** den Zustand **Verfügbar** aufweist.

### <a name="step-3-detach-data-disk-from-the-vm"></a>Schritt 3: Trennen eines Datenträgers von der VM
Wenn die VHD ein Datenträger ist, trennen Sie die VHD von der VM, um die Lease zu entfernen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü „Hub“ die Option **Virtuelle Computer** aus.
3. Wählen Sie die VM aus, an die die VHD angefügt ist.
4. Wählen Sie im Bereich **Details zum virtuellen Computer** die Option **Datenträger**.
5. Wählen Sie den zu löschenden Datenträger aus, der die VHD angefügt ist. Anhand der URL der VHD können Sie erkennen, welches Blob an den Datenträger angefügt wurde.
6. Sie können den Blobspeicherort überprüfen, indem Sie durch Klicken auf den Datenträger den Pfad im Feld **VHD-URI** überprüfen.
7. Wählen Sie oben im Bereich **Datenträger** die Option **Bearbeiten** aus.
8. Klicken Sie für den zu löschenden Datenträger auf das Symbol **Trennen**.

     ![Screenshot des Portals, in dem der Speicherbereich „Blobmetadaten“ geöffnet ist](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. Wählen Sie **Speichern**aus. Der Datenträger ist jetzt vom virtuellen Computer getrennt, und die VHD wird nicht mehr geleast. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Um sich zu vergewissern, dass das Lease aufgehoben wurde, navigieren Sie zum Blobspeicherort, und prüfen Sie im Bereich **Blobeigenschaften**, ob der **Leasezustand** den Zustand **Entsperrt** oder **Verfügbar** aufweist.



[Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs]: #you-are-experiencing-unexpected-reboots
[Speicherbezogene Fehler beim Löschen in der Resource Manager-Bereitstellung]: #storage-delete-errors-in-rm