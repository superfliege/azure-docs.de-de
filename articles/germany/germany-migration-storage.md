---
title: Migration von Azure Deutschland Storage-Ressourcen zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Storage-Ressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 43d5429134b834483e549b71162f63e110f4436e
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51974366"
---
# <a name="migration-from-azure-germany-storage-resources-to-global-azure"></a>Migration von Azure Deutschland Storage-Ressourcen zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure Storage-Ressourcen von Azure Deutschland zu Azure weltweit.

## <a name="blobs"></a>Blobs (in englischer Sprache)

AzCopy ist ein kostenloses Tool, mit dem Sie Blobs, Dateien und Tabellen kopieren können. AzCopy funktioniert innerhalb von Azure, vom lokalen Computer zu Azure und von Azure zum lokalen Computer. Verwenden Sie AzCopy für Ihre Migration zum direkten Kopieren von Blobs zwischen Azure Deutschland und Azure weltweit.

Wenn Sie nicht verwaltete Datenträger für den virtuellen Quellcomputer nutzen, verwenden Sie AzCopy zum Kopieren der `.vhd`-Dateien in die Zielumgebung. Andernfalls müssen Sie einige Schritte im Vorfeld ausführen, siehe [Empfehlungen für Managed Disks](#managed-disks) weiter unten.

Das folgende kurze Beispiel zeigt die Funktionsweise von AzCopy. Die vollständige Referenz finden Sie in der [AzCopy-Dokumentation](../storage/common/storage-use-azcopy.md).

AzCopy verwendet die Begriffe *Source* und *Dest*, ausgedrückt als URIs. URIs für Azure Deutschland weisen immer das folgende Format auf:

```http
https://<storageaccountname>.blob.core.cloudapi.de/<containername>/<blobname>
```

Für Azure weltweit:

```http
https://<storageaccountname>.blob.core.windows.net/<containername>/<blobname>
```

Sie erhalten die drei Teile (*storageaccountname*, *containername* und *blobname*) für den URI über das Portal mit PowerShell oder der CLI. Der Name des Blobs kann Teil des URI sein oder als ein Muster (z.B. *vm121314.vhd*) angegeben werden.

Außerdem benötigen Sie die Speicherkontoschlüssel, um auf das Speicherkonto zugreifen zu können. Rufen Sie sie über das Portal, PowerShell oder die CLI ab. Beispiel: 

```powershell
Get-AzureRmStorageAccountKey -Name <saname> -ResourceGroupName <rgname>
```

Wie immer benötigen Sie nur einen der beiden Schlüssel, die für jedes Speicherkonto verfügbar sind.

Beispiel:
URI-Teil | Beispielwert
-------- | --------------
Quell-storageAccount | `migratetest`
Quellcontainer | `vhds`
Quellblob | `vm-121314.vhd`
Ziel-storageAccount | `migratetarget`
Zielcontainer | `targetcontainer`

Dieser Befehl kopiert eine virtuelle Festplatte aus Azure Deutschland in Azure weltweit (die Schlüssel wurden zur besseren Lesbarkeit gekürzt):

```cmd
azcopy -v /source:https://migratetest.blob.core.cloudapi.de/vhds /sourcekey:"0LN...w==" /dest:https://migratetarget.blob.core.windows.net/targetcontainer /DestKey:"o//ucDi5TN...w==" /Pattern:vm-121314.vhd
```

Um eine konsistente Kopie der VHD zu erhalten, fahren Sie den virtuellen Computer vor dem Kopiervorgang herunter, und planen Sie eine Ausfallzeit ein. Nach dem Kopieren [befolgen Sie diese Anweisungen](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks), um Ihren virtuellen Computer in der Zielumgebung erneut zu erstellen.

### <a name="links"></a>Links

- [AZCopy-Dokumentation](../storage/common/storage-use-azcopy.md)
- [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks)















## <a name="disks"></a>Datenträger

Azure Managed Disks vereinfacht die Datenträgerverwaltung für Azure IaaS-VMs, indem die Speicherkonten verwaltet werden, die dem VM-Datenträger zugeordnet sind. Da Sie keinen direkten Zugriff auf die `.vhd`-Datei besitzen, können Sie Tools wie AzCopy nicht direkt zum Kopieren der Dateien verwenden (siehe [Speichermigration](#blobs)). Die Problemumgehung besteht darin, zunächst den verwalteten Datenträger zu exportieren, indem Sie einen temporären SAS-URI abrufen, und ihn dann mit diesen Informationen herunterzuladen oder zu kopieren. Das folgende kurze Beispiel zeigt das Abrufen des SAS-URI und die weitere Vorgehensweise:

### <a name="step-1-get-sas-uri"></a>Schritt 1: Abrufen des SAS-URI

- Navigieren Sie zum Portal, suchen Sie nach Ihrem verwalteten Datenträger (in der gleichen Ressourcengruppe wie Ihr virtueller Computer, der Ressourcentyp ist „Datenträger“).
- Suchen Sie in der `Overview` nach der Schaltfläche `Export` im oberen Menüband, und klicken Sie auf diese (Sie müssen Ihren virtuellen Computer zuerst herunterfahren und die Zuordnung aufheben oder ihn trennen).
- Definieren Sie eine Zeit, zu der der URI abläuft (der Standardwert ist 3.600 Sekunden).
- Generieren Sie die URL (dies sollte nur wenige Sekunden in Anspruch nehmen).
- Kopieren Sie die URL (sie wird nur ein Mal angezeigt).

### <a name="step-2-azcopy"></a>Schritt 2: AzCopy

Beispiele für die Verwendung von AzCopy finden Sie weiter oben in diesem Artikel unter [Blobmigration](#blobs). Verwenden Sie AzCopy (oder ein anderes Tool) zum Kopieren des Datenträgers direkt aus der Quellumgebung in die Zielumgebung. Für AzCopy müssen Sie den URI in den Basis-URI und den SAS-Bestandteil aufteilen und dabei mit dem Zeichen „?“ beginnen. Wenn das Portal den folgenden SAS-URI bereitstellt:

```http
https://md-kp4qvrzhj4j5.blob.core.cloudapi.de/r0pmw4z3vk1g/abcd?sv=2017-04-17&sr=b&si=22970153-4c56-47c0-8cbb-156a24b6e4b5&sig=5Hfu0qMw9rkZf6mCjuCE4VMV6W3IR8FXQSY1viji9bg%3D>
```

Lauten die Quellparameter für AzCopy folgendermaßen:

```cmd
/source:"https://md-kp4qvrzhj4j5.blob.core.cloudapi.de/r0pmw4z3vk1g/abcd"
```

and

```cmd
/sourceSAS:" ?sv=2017-04-17&sr=b&si=22970153-4c56-47c0-8cbb-156a24b6e4b5&sig=5Hfu0qMw9rkZf6mCjuCE4VMV6W3IR8FXQSY1viji9bg%3D"
```

Und die vollständige Befehlszeile:

```cmd
azcopy -v /source:"https://md-kp4qvrzhj4j5.blob.core.cloudapi.de/r0pmw4z3vk1g/abcd" /sourceSAS:"?sv=2017-04-17&sr=b&si=22970153-4c56-47c0-8cbb-156a24b6e4b5&sig=5Hfu0qMw9rkZf6mCjuCE4VMV6W3IR8FXQSY1viji9bg%3D" /dest:"https://migratetarget.blob.core.windows.net/targetcontainer/newdisk.vhd" /DestKey:"o//ucD... Kdpw=="
```

### <a name="step-3-create-a-new-managed-disk-in-the-target-environment"></a>Schritt 3: Erstellen eines neuen verwalteten Datenträgers in der Zielumgebung

Es gibt mehrere Möglichkeiten, einen neuen verwalteten Datenträger zu erstellen, z.B. über das Portal:

- Wählen Sie `New` > `Managed Disk` > `Create` im Portal aus.
- Benennen Sie den neuen Datenträger.
- Wählen Sie wie gewohnt eine Ressourcengruppe aus.
- Wählen Sie unter `Source type` die Option *Speicherblob* aus, und kopieren Sie den Ziel-URI aus den AzCopy-Befehl, oder durchsuchen Sie die Optionen, um eine Auswahl zu treffen.
- Wenn Sie einen Betriebssystem-Datenträger kopiert haben, wählen Sie den Betriebssystemtyp aus. Klicken Sie andernfalls auf `Create`.

### <a name="step-4-create-the-vm"></a>Schritt 4: Erstellen des virtuellen Computers

Auch hier stehen verschiedene Möglichkeiten zum Erstellen eines virtuellen Computers mit diesem neuen verwalteten Datenträger zur Verfügung.

Wählen Sie im Portal den Datenträger aus, und klicken Sie im oberen Menüband auf `Create VM`. Definieren Sie die anderen Parameter Ihres virtuellen Computers wie gewohnt.

Für PowerShell folgen Sie den Anweisungen [in diesem Artikel](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks), um einen virtuellen Computer von einem wiederhergestellten Datenträger zu erstellen.

### <a name="links"></a>Links

- Exportieren auf den Datenträger [über die API](/rest/api/compute/disks/grantaccess) durch Abrufen eines SAS-URI 
- Erstellen eines verwalteten Datenträgers [über die API](/rest/api/compute/disks/createorupdate#create_a_managed_disk_by_importing_an_unmanaged_blob_from_a_different_subscription.) aus einem nicht verwalteten Blob
