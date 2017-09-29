---
title: Registrieren/Aufheben der Registrierung eines Servers bei der Azure-Dateisynchronisierung (Vorschau) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Windows Server bei einem Azure-Datei-/Speichersynchronisierungsdienst registrieren bzw. die Registrierung aufheben.
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: wgries
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 0acf183fbaea99e4316b668a3da28d79b20b7bef
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="registerunregister-a-server-with-azure-file-sync-preview"></a>Registrieren/Aufheben der Registrierung eines Servers bei der Azure-Dateisynchronisierung (Vorschau)
Mit der Azure-Dateisynchronisierung (Vorschau) können Freigaben lokal oder in Azure repliziert werden, und der Zugriff darauf kann über SMB- oder NFS-Freigaben in Windows Server erfolgen. Die Azure-Dateisynchronisierung ist besonders nützlich in Szenarien, in denen weit entfernt von einem Azure-Rechenzentrum gespeicherte Daten abgerufen und geändert werden müssen, z.B. in einer Zweigstelle. Daten können zwischen mehreren Windows Server-Endpunkten repliziert werden, z.B. zwischen mehreren Zweigstellen.

Im folgenden Artikel wird beschrieben, wie Sie einen Windows Server bei einem Speichersynchronisierungsdienst registrieren bzw. die Registrierung aufheben. Dies kann erforderlich sein, wenn ein Server außer Betrieb genommen oder ein neuer Serverendpunkt in einer Synchronisierungsgruppe hinzugefügt werden soll. Informationen über die End-to-End-Bereitstellung der Azure-Dateisynchronisierung finden Sie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie einen Windows Server bei einem Speichersynchronisationsdienst registrieren können, müssen Sie die erforderlichen Voraussetzungen auf einem Windows Server sicherstellen:

* Stellen Sie sicher, dass der Speichersynchronisierungsdienst bereitgestellt wurde. Weitere Informationen zum Bereitstellen eines Speichersynchronisierungsdiensts finden Sie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).
* Stellen Sie sicher, dass der Server mit dem Internet verbunden ist und dass der Zugriff auf Azure möglich ist.
* Deaktivieren Sie die verstärkte Sicherheitskonfiguration für IE über die Server-Manager-Benutzeroberfläche.
    
    ![Server-Manager-Benutzeroberfläche, auf der die Option „Verstärkte Sicherheitskonfiguration für IE“ hervorgehoben ist](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Stellen Sie sicher, dass das AzureRM-PowerShell-Modul auf dem Server installiert ist. Wenn der Server Mitglied eines Failoverclusters ist, ist das AzureRM-Modul für jeden Knoten im Cluster erforderlich. Weitere Informationen zur Installation des AzureRM-Moduls finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Es wird empfohlen, die neueste Version des AzureRM-PowerShell-Moduls zu verwenden, um einen Server zu registrieren bzw. die Registrierung aufzuheben. Wenn das AzureRM-Paket zuvor auf diesem Server installiert wurde (und die PowerShell-Version auf diesem Server 5.* oder höher ist), können Sie das Paket mit dem Cmdlet `Update-Module` aktualisieren. 

## <a name="register-a-server-with-storage-sync-service"></a>Registrieren eines Servers beim Speichersynchronisierungsdienst
Damit ein Windows Server als *Serverendpunkt* in einer *Synchronisierungsgruppe* für die Azure-Dateisynchronisierung verwendet werden kann, muss er bei einem *Speicher-Synchronisierungsdienst* registriert werden. Ein Server kann immer nur bei einem *Speichersynchronisierungsdienst* registriert sein.

### <a name="install-the-azure-file-sync-agent"></a>Installieren des Azure-Dateisynchronisierungs-Agents
1. [Laden Sie den Azure-Dateisynchronisierungs-Agent herunter](https://go.microsoft.com/fwlink/?linkid=858257).
2. Starten Sie den Installer für den Azure-Dateisynchronisierungs-Agent.
    
    ![Erste Seite des Installers für den Azure-Dateisynchronisierungs-Agent](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Sie müssen Updates für den Azure-Dateisynchronisierungs-Agent mit Microsoft Update aktivieren. Dies ist deshalb von Bedeutung, weil wichtige Sicherheitspatches und Verbesserungen an Funktionen des Serverpakets über Microsoft Update ausgeliefert werden.

    ![Stellen Sie sicher, dass Microsoft Update im Bereich „Microsoft Update“ des Installers für den Azure-Dateisynchronisierungs-Agent aktiviert ist](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Wenn der Server noch nicht registriert wurde, wird die Benutzeroberfläche für die Serverregistrierung nach Abschluss der Installation geöffnet.

> [!Important]  
> Wenn der Server Mitglied eines Failoverclusters ist, muss der Azure-Dateisynchronisierungs-Agent auf jedem Knoten im Cluster installiert werden.

### <a name="register-the-server-using-the-server-registration-ui"></a>Registrieren des Servers mit der Benutzeroberfläche für die Serverregistrierung
1. Wenn die Benutzeroberfläche für die Serverregistrierung nicht unmittelbar nach Abschluss der Installation des Azure-Dateisynchronsierungs-Agents gestartet wird, können Sie sie manuell starten, indem Sie `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe` ausführen.
2. Klicken Sie auf *Anmelden*, um auf Ihr Azure-Abonnement zuzugreifen. 

    ![Öffnen des Dialogfelds mit der Benutzeroberfläche für die Serverregistrierung](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Wählen Sie im Dialogfeld das richtige Abonnement, die Ressourcengruppe und den Speicher-Synchronisierungsdienst aus.

    ![Angaben zum Speichersynchronisierungsdienst](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. In der Vorschauversion ist eine weitere Anmeldung erforderlich, um den Vorgang abzuschließen. 

    ![Anmeldedialogfeld](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Wenn der Server Mitglied eines Failoverclusters ist, muss die Serverregistrierung für jeden einzelnen Server ausgeführt werden. Wenn Sie die registrierten Server im Azure-Portal anzeigen, werden alle Knoten von der Azure-Dateisynchronisierung automatisch als Mitglied des gleichen Failoverclusters erkannt und entsprechend gruppiert.

## <a name="unregister-the-server-with-storage-sync-service"></a>Aufheben der Registrierung des Servers beim Speichersynchronisierungsdienst
Zum Aufheben der Registrierung eines Servers bei einem Speichersynchronisierungsdienst sind mehrere Schritte erforderlich. Im Folgenden wird das ordnungsgemäße Aufheben der Serverregistrierung behandelt.

### <a name="optional-recall-all-tiered-data"></a>(Optional) Abrufen aller ausgelagerten Daten
Wenn für einen Serverendpunkt das Cloudtiering aktiviert ist, werden Dateien auf Ihre Azure-Dateifreigaben *ausgelagert*. Dadurch können lokale Dateifreigaben als Cache statt als vollständige Kopie des Datasets fungieren, um den Speicherplatz auf dem Dateiserver effizient zu nutzen. Wenn jedoch ein Serverendpunkt entfernt wird und ausgelagerte Dateien noch lokal auf dem Server vorhanden sind, kann auf diese Dateien nicht mehr zugegriffen werden. Aus diesem Grund müssen Sie vor dem Aufheben der Registrierung alle ausgelagerten Dateien aus Azure Files abrufen, wenn ein Zugriff auf die Dateien weiterhin möglich sein soll. 

Dies kann mithilfe des folgenden PowerShell-Cmdlets durchgeführt werden:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Wenn das lokale Volume, auf dem der Server gehostet wird, nicht genügend freien Speicherplatz aufweist, um alle ausgelagerten Daten abzurufen, schlägt das Cmdlet `Invoke-StorageSyncFileRecall` fehl.  

### <a name="remove-the-server-from-all-sync-groups"></a>Entfernen des Servers aus allen Synchronisierungsgruppen
Vor dem Aufheben der Registrierung des Servers beim Speichersynchronisierungsdienst müssen alle Serverendpunkte für diesen Server entfernt werden. Hierzu können Sie das Portal verwenden:

1. Navigieren Sie zu dem Speichersynchronisierungsdienst, bei dem der Server registriert ist.
2. Entfernen Sie alle Serverendpunkte für diesen Server in allen Synchronisierungsgruppen im Speichersynchronisierungsdienst. Klicken Sie hierfür im Bereich „Synchronisierungsgruppe“ mit der rechten Maustaste auf den entsprechenden Serverendpunkt.

    ![Entfernen eines Serverendpunkts aus einer Synchronisierungsgruppe](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Sie können hierzu auch ein einfaches PowerShell-Skript verwenden:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Login-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

### <a name="unregister-the-server"></a>Aufheben der Serverregistrierung
Nachdem alle Daten abgerufen wurden und der Server aus allen Synchronisierungsgruppen entfernt wurde, kann die Registrierung des Servers aufgehoben werden. 

1. Navigieren Sie im Azure-Portal zum Abschnitt „Registrierte Server“ des Speichersynchronisierungsdiensts.
2. Klicken Sie mit der rechten Maustaste auf den Server, dessen Registrierung Sie aufheben möchten, und klicken Sie auf „Serverregistrierung aufheben“.

    ![Serverregistrierung aufheben](media/storage-sync-files-server-registration/unregister-server-1.png)
