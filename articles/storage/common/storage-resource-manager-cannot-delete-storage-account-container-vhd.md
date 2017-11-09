---
title: "Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs | Microsoft-Dokumentation"
description: "Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs"
services: storage
documentationcenter: 
author: passaree
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 7a3c9422887592c29c2eae8bd75cf960865808fd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-storage-delete-errors-in-resource-manager-deployment"></a>Behandlung von speicherbezogenen Fehlern beim Löschen in der Resource Manager-Bereitstellung
Dieser Artikel enthält Informationen zur Behandlung von einem der folgenden Fehler, die auftreten, wenn versucht wird, ein Azure-Speicherkonto, einen Azure-Container und ein Azure-Blob in der Azure Resource Manager-Bereitstellung zu löschen.

>**Fehler beim Löschen von Speicherkonto 'StorageAccountName'. Fehler: Das Speicherkonto kann nicht gelöscht werden, da zugehörige Artefakte gerade verwendet werden.**

>**Fehler beim Löschen von # von # Containern:<br>vhds: Derzeit ist eine Lease für den Container vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.**

>**Fehler beim Löschen von # von # Blobs:<br>BlobName.vhd: Derzeit ist eine Lease für den Blob vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.**

Bei den in den Azure-VMs verwendeten virtuellen Festplatten handelt es sich um VHD-Dateien, die als Seitenblobs in einem Storage Standard- oder Premium-Konto in Azure gespeichert sind.  Weitere Informationen zu Azure-Datenträgern finden Sie [hier](../../virtual-machines/windows/about-disks-and-vhds.md). Azure verhindert das Löschen eines an eine VM angefügten Datenträgers, um eine Beschädigung zu vermeiden. Zudem verhindert es das Löschen von Containern und Speicherkonten, die ein an eine VM angefügten Seitenblob aufweisen. 

## <a name="solution"></a>Lösung
Der Prozess zum Löschen eines Speicherkontos, Containers oder Blobs bei einer der oben genannten Fehlermeldungen umfasst folgende Schritte: 
1. [Identifizieren von an eine VM angefügten Blobs](#step-1-identify-blobs-attached-to-a-vm)
2. [Löschen von VMs mit angefügtem **Betriebssystemdatenträger**](#step-2-delete-vm-to-detach-os-disk)
3. [Trennen aller **Datenträger** von verbleibenden VMs](#step-3-detach-data-disk-from-the-vm)

Wenn Sie die oben genannten Schritte durchgeführt haben, versuchen Sie erneut, ein Speicherkonto, einen Container bzw. ein Blob zu löschen.

### <a name="step-1-identify-blob-attached-to-a-vm"></a>Schritt 1: Identifizieren von an eine VM angefügten Blobs

#### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Szenario 1: Löschen eines Blobs – Identifizieren einer angefügten VM
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus. Wechseln Sie zum Speicherkonto, wählen Sie unter **Blobdienst** die Option **Container** aus, und navigieren Sie zum Blob, das gelöscht werden soll.
3. Wenn das Blob als **Leasezustand** den Zustand **Geleast** aufweist, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Metadaten bearbeiten**, um den Bereich „Blobmetadaten“ zu öffnen. 

    ![Screenshot des Portals, in dem die Speicherkontoblobs und die per Rechtsklick aufgerufene Option „Metadaten bearbeiten“ hervorgehoben sind](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-edit-metadata-sm.png)

4. Überprüfen und notieren Sie im Bereich „Blobmetadaten“ den Wert für **MicrosoftAzureCompute_VMName**. Dieser Wert ist der Name der VM, der die VHD angefügt ist. (Wenn dieses Feld nicht vorhanden ist, siehe **Wichtig**.)
5. Überprüfen und notieren Sie im Bereich „Blobmetadaten“ den Wert für **MicrosoftAzureCompute_DiskType**. Hierdurch wird ermittelt, ob es sich beim angefügten Datenträger um einen Betriebssystemdatenträger oder Datenträger handelt (wenn dieses Feld nicht vorhanden ist. siehe **Wichtig**). 

     ![Screenshot des Portals, in dem der Speicherbereich „Blobmetadaten“ geöffnet ist](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-blob-metadata-sm.png)

6. Wenn der Blobdatenträger vom Typ **OSDisk** ist, führen Sie [Schritt 2: Löschen einer VM beim Trennen eines Betriebssystemdatenträgers](#step-2-delete-vm-to-detach-os-disk) durch. Wenn der Blobdatenträger hingegen vom Typ **DataDisk** ist, befolgen Sie die Anweisungen unter [Schritt 3: Trennen eines Datenträgers von der VM](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Wenn **MicrosoftAzureCompute_VMName** und **MicrosoftAzureCompute_DiskType** nicht in den Blobmetadaten angezeigt werden, bedeutet dies, dass das Blob explizit geleast und nicht an eine VM angefügt ist. Geleaste Blobs können erst gelöscht werden, wenn das Lease abgebrochen wurde. Um ein Lease abzubrechen, klicken Sie mit der rechten Maustaste auf das Blob, und wählen Sie **Lease abbrechen** aus. Geleaste Blobs, die nicht an eine VM angefügt sind, verhindern das Löschen des Blobs, jedoch nicht das Löschen eines Containers oder Speicherkontos.

#### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Szenario 2: Löschen eines Containers – Identifizieren aller Blobs in einem an VMs angefügten Container
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus. Wechseln Sie zum Speicherkonto, wählen Sie unter **Blobdienst** die Option **Container** aus, und machen Sie den Container ausfindig, der gelöscht werden soll.
3. Klicken Sie darauf, um den Container zu öffnen. Daraufhin wird die Liste der darin enthaltenen Blobs angezeigt. Machen Sie in dieser Liste alle Blobs vom Blobtyp **Seitenblob** und mit dem Leasezustand **Geleast** ausfindig. Führen Sie die Schritte von [Szenario 1](#step-1-identify-blobs-attached-to-a-vm) durch, um die mit diesen Blobs verknüpfte VM zu identifizieren.

    ![Screenshot des Portals, in dem die Speicherkontoblobs und die Option „Leasezustand“ mit dem Wert „Geleast“ hervorgehoben sind](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-disks-sm.png)

4. Führen Sie [Schritt 2](#step-2-delete-vm-to-detach-os-disk) und [Schritt 3](#step-3-detach-data-disk-from-the-vm) durch, um VMs mit Datenträgern vom Typ **OSDisk** zu löschen und Datenträger vom Typ **DataDisk** zu trennen. 

#### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Szenario 3: Löschen eines Speicherkontos – Identifizieren aller Blobs in einem an VMs angefügten Speicherkonto
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus. Wechseln Sie zum Speicherkonto, und wählen Sie unter **Blobdienst** die Option **Container** aus.

    ![Screenshot des Portals, in dem die Speicherkontocontainer und die Option „Leasezustand“ mit dem Wert „Geleast“ hervorgehoben sind](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-containers-sm.png)

3. Identifizieren Sie auf dem Blatt **Container** alle Container, die als **Leasezustand** den Wert **Geleast** aufweisen, und führen Sie für jeden Container mit dem Zustand **Geleast** die Schritte von [Szenario 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) durch.
4. Führen Sie [Schritt 2](#step-2-delete-vm-to-detach-os-disk) und [Schritt 3](#step-3-detach-data-disk-from-the-vm) durch, um VMs mit Datenträgern vom Typ **OSDisk** zu löschen und Datenträger vom Typ **DataDisk** zu trennen. 

### <a name="step-2-delete-vm-to-detach-os-disk"></a>Schritt 2: Löschen einer VM beim Trennen eines Betriebssystemdatenträgers
Wenn es sich bei der VHD um einen Betriebssystemdatenträger handelt, müssen Sie zuerst die VM löschen, bevor Sie die angefügte VHD löschen können. Für Datenträger, die an dieselbe VM angefügt sind, sind keine weiteren Aktionen erforderlich, nachdem Sie die folgenden Schritte durchgeführt haben:

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü „Hub“ die Option **Virtuelle Computer** aus.
3. Wählen Sie die VM aus, an die die VHD angefügt ist.
4. Stellen Sie sicher, dass der virtuelle Computer nicht aktiv verwendet wird und dass Sie den virtuellen Computer nicht mehr benötigen.
5. Wählen Sie oben auf dem Blatt **Details zum virtuellen Computer** die Option **Löschen** aus, und klicken Sie zur Bestätigung auf **Ja**.
6. Die VM sollte nun gelöscht, doch die VHD noch vorhanden sein. Die VHD sollte nicht mehr an eine VM angefügt bzw. es sollte keine Lease mehr für diese vorhanden sein. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Um sich zu vergewissern, dass das Lease aufgehoben wurde, navigieren Sie zum Blobspeicherort, und prüfen Sie im Bereich **Blobeigenschaften**, ob der **Leasezustand** den Zustand **Verfügbar** aufweist.

### <a name="step-3-detach-data-disk-from-the-vm"></a>Schritt 3: Trennen eines Datenträgers von der VM
Wenn die VHD ein Datenträger ist, trennen Sie die VHD von der VM, um die Lease zu entfernen:

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü „Hub“ die Option **Virtuelle Computer** aus.
3. Wählen Sie die VM aus, an die die VHD angefügt ist.
4. Wählen Sie auf dem Blatt **Details zum virtuellen Computer** die Option **Datenträger**.
5. Wählen Sie den zu löschenden Datenträger aus, der die VHD angefügt ist. Anhand der URL der VHD können Sie erkennen, welches Blob an den Datenträger angefügt wurde.
6. Sie können den Blobspeicherort überprüfen, indem Sie durch Klicken auf den Datenträger den Pfad im Feld **VHD-URI** überprüfen.
7. Wählen Sie oben auf dem Blatt **Datenträger** die Option **Bearbeiten** aus.
8. Klicken Sie für den zu löschenden Datenträger auf das Symbol **Trennen**.

     ![Screenshot des Portals, in dem der Speicherbereich „Blobmetadaten“ geöffnet ist](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-vm-disks-edit.png)

9. Wählen Sie **Speichern** aus. Der Datenträger sollte nun vom virtuellen Computer getrennt sein, und die virtuelle Festplatte sollte keine Lease für Datenträger mehr aufweisen. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Um sich zu vergewissern, dass das Lease aufgehoben wurde, navigieren Sie zum Blobspeicherort, und prüfen Sie im Bereich **Blobeigenschaften**, ob der **Leasezustand** den Zustand **Entsperrt** oder **Verfügbar** aufweist.

## <a name="next-steps"></a>Nächste Schritte
Wiederholen Sie den Löschvorgang des Speicherobjekts, bei dem zuvor ein Fehler aufgetreten ist.

