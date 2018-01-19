---
title: Verwalten registrierter Server mit Azure File Sync (Vorschau) | Microsoft-Dokumentation
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
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: 10c8b708cad245f4ac0304489beb36dcf63cd4b1
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2018
---
# <a name="manage-registered-servers-with-azure-file-sync-preview"></a>Verwalten registrierter Server mit Azure File Sync (Vorschau)
Mit Azure File Sync (Vorschau) können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Dies erfolgt durch Umwandeln der Windows-Server in einen Schnellcache der Azure-Dateifreigabe. Sie können alle unter Windows Server verfügbaren Protokolle für den lokalen Zugriff auf Ihre Daten (einschließlich SMB, NFS und FTPS) sowie beliebig viele Caches weltweit verwenden.

Im folgenden Artikel wird beschrieben, wie Sie einen Server bei einem Speichersynchronisierungsdienst registrieren und verwalten. Informationen über die End-to-End-Bereitstellung der Azure-Dateisynchronisierung finden Sie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Registrieren eines Servers beim Speichersynchronisierungsdienst und Aufheben der Registrierung
Beim Registrieren eines Servers bei Azure File Sync wird eine Vertrauensstellung zwischen Windows Server und Azure eingerichtet. Mithilfe dieser Beziehung können dann *Serverendpunkte* auf dem Server erstellt werden, die bestimmte Ordner darstellen, die mit einer Azure-Dateifreigabe (auch als *Cloudendpunkt* bezeichnet) synchronisiert werden sollen. 

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie einen Server bei einem Speichersynchronisationsdienst registrieren können, müssen die erforderlichen Voraussetzungen auf dem Server erfüllt sein:

* Ihr Server muss unter einer unterstützten Version von Windows Server ausgeführt werden. Weitere Informationen finden Sie unter [Unterstützte Windows Server-Versionen](storage-sync-files-planning.md#supported-versions-of-windows-server).
* Stellen Sie sicher, dass der Speichersynchronisierungsdienst bereitgestellt wurde. Weitere Informationen zum Bereitstellen eines Speichersynchronisierungsdiensts finden Sie unter [Bereitstellen der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-deployment-guide.md).
* Stellen Sie sicher, dass der Server mit dem Internet verbunden ist und dass der Zugriff auf Azure möglich ist.
* Deaktivieren Sie die verstärkte Sicherheitskonfiguration für IE über die Server-Manager-Benutzeroberfläche.
    
    ![Server-Manager-Benutzeroberfläche, auf der die Option „Verstärkte Sicherheitskonfiguration für IE“ hervorgehoben ist](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Stellen Sie sicher, dass das AzureRM-PowerShell-Modul auf dem Server installiert ist. Wenn der Server Mitglied eines Failoverclusters ist, ist das AzureRM-Modul für jeden Knoten im Cluster erforderlich. Weitere Informationen zur Installation des AzureRM-Moduls finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Es wird empfohlen, die neueste Version des AzureRM-PowerShell-Moduls zu verwenden, um einen Server zu registrieren bzw. die Registrierung aufzuheben. Wenn das AzureRM-Paket zuvor auf diesem Server installiert wurde (und die PowerShell-Version auf diesem Server 5.* oder höher ist), können Sie das Paket mit dem Cmdlet `Update-Module` aktualisieren. 

### <a name="register-a-server-with-storage-sync-service"></a>Registrieren eines Servers beim Speichersynchronisierungsdienst
Damit ein Server als *Serverendpunkt* in einer *Synchronisierungsgruppe* von Azure File Sync verwendet werden kann, muss er bei einem *Speichersynchronisierungsdienst* registriert werden. Ein Server kann immer nur bei einem Speichersynchronisierungsdienst registriert sein.

#### <a name="install-the-azure-file-sync-agent"></a>Installieren des Azure-Dateisynchronisierungs-Agents
1. [Laden Sie den Azure-Dateisynchronisierungs-Agent herunter](https://go.microsoft.com/fwlink/?linkid=858257).
2. Starten Sie den Installer für den Azure-Dateisynchronisierungs-Agent.
    
    ![Erste Seite des Installers für den Azure-Dateisynchronisierungs-Agent](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Sie müssen Updates für den Azure-Dateisynchronisierungs-Agent mit Microsoft Update aktivieren. Dies ist deshalb von Bedeutung, weil wichtige Sicherheitspatches und Verbesserungen an Funktionen des Serverpakets über Microsoft Update ausgeliefert werden.

    ![Stellen Sie sicher, dass Microsoft Update im Bereich „Microsoft Update“ des Installers für den Azure-Dateisynchronisierungs-Agent aktiviert ist](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Wenn der Server noch nicht registriert wurde, wird die Benutzeroberfläche für die Serverregistrierung nach Abschluss der Installation geöffnet.

> [!Important]  
> Wenn der Server Mitglied eines Failoverclusters ist, muss der Azure-Dateisynchronisierungs-Agent auf jedem Knoten im Cluster installiert werden.

#### <a name="register-the-server-using-the-server-registration-ui"></a>Registrieren des Servers mit der Benutzeroberfläche für die Serverregistrierung
> [!Important]  
> Cloud Solution Provider-Abonnements können die Benutzeroberfläche für die Serverregistrierung nicht verwenden. Verwenden Sie stattdessen PowerShell (unten diesem Abschnitt).

1. Wenn die Benutzeroberfläche für die Serverregistrierung nicht unmittelbar nach Abschluss der Installation des Azure File Sync-Agents gestartet wird, können Sie sie manuell starten, indem Sie `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe` ausführen.
2. Klicken Sie auf *Anmelden*, um auf Ihr Azure-Abonnement zuzugreifen. 

    ![Öffnen des Dialogfelds mit der Benutzeroberfläche für die Serverregistrierung](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Wählen Sie im Dialogfeld das richtige Abonnement, die Ressourcengruppe und den Speicher-Synchronisierungsdienst aus.

    ![Angaben zum Speichersynchronisierungsdienst](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. In der Vorschauversion ist eine weitere Anmeldung erforderlich, um den Vorgang abzuschließen. 

    ![Anmeldedialogfeld](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Wenn der Server Mitglied eines Failoverclusters ist, muss die Serverregistrierung für jeden einzelnen Server ausgeführt werden. Wenn Sie die registrierten Server im Azure-Portal anzeigen, werden alle Knoten von Azure File Sync automatisch als Mitglied des gleichen Failoverclusters erkannt und entsprechend gruppiert.

#### <a name="register-the-server-with-powershell"></a>Registrieren des Servers mit PowerShell
Sie können auch Serverregistrierung auch über PowerShell ausführen. Dies ist die einzige unterstützte Methode zur Serverregistrierung für Cloud Solution Provider-Abonnements:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Aufheben der Registrierung des Servers beim Speichersynchronisierungsdienst
Zum Aufheben der Registrierung eines Servers bei einem Speichersynchronisierungsdienst sind mehrere Schritte erforderlich. Im Folgenden wird das ordnungsgemäße Aufheben der Serverregistrierung behandelt.

#### <a name="optional-recall-all-tiered-data"></a>(Optional) Abrufen aller ausgelagerten Daten
Wenn für einen Serverendpunkt das Cloudtiering aktiviert ist, werden Dateien auf Ihre Azure-Dateifreigaben *ausgelagert*. Dadurch können lokale Dateifreigaben als Cache statt als vollständige Kopie des Datasets fungieren, um den Speicherplatz auf dem Dateiserver effizient zu nutzen. Wenn jedoch ein Serverendpunkt entfernt wird und ausgelagerte Dateien noch lokal auf dem Server vorhanden sind, kann auf diese Dateien nicht mehr zugegriffen werden. Aus diesem Grund müssen Sie vor dem Aufheben der Registrierung alle ausgelagerten Dateien aus Azure Files abrufen, wenn ein Zugriff auf die Dateien weiterhin möglich sein soll. 

Dies kann mithilfe des folgenden PowerShell-Cmdlets durchgeführt werden:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Wenn das lokale Volume, auf dem der Serverendpunkt gehostet wird, nicht genügend freien Speicherplatz aufweist, um alle ausgelagerten Daten abzurufen, führt das Cmdlet `Invoke-StorageSyncFileRecall` zu einem Fehler.  

#### <a name="remove-the-server-from-all-sync-groups"></a>Entfernen des Servers aus allen Synchronisierungsgruppen
Vor dem Aufheben der Registrierung des Servers beim Speichersynchronisierungsdienst müssen alle Serverendpunkte für diesen Server entfernt werden. Hierzu können Sie das Azure-Portal verwenden:

1. Navigieren Sie zu dem Speichersynchronisierungsdienst, bei dem der Server registriert ist.
2. Entfernen Sie alle Serverendpunkte für diesen Server aus allen Synchronisierungsgruppen im Speichersynchronisierungsdienst. Klicken Sie hierfür im Bereich „Synchronisierungsgruppe“ mit der rechten Maustaste auf den entsprechenden Serverendpunkt.

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

#### <a name="unregister-the-server"></a>Aufheben der Serverregistrierung
Nachdem alle Daten abgerufen wurden und der Server aus allen Synchronisierungsgruppen entfernt wurde, kann die Registrierung des Servers aufgehoben werden. 

1. Navigieren Sie im Azure-Portal zum Abschnitt *Registrierte Server* des Speichersynchronisierungsdiensts.
2. Klicken Sie mit der rechten Maustaste auf den Server, dessen Registrierung Sie aufheben möchten, und klicken Sie auf „Serverregistrierung aufheben“.

    ![Serverregistrierung aufheben](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Sicherstellen, dass Azure File Sync in Ihrem Rechenzentrum ein guter Nachbar ist 
Da Azure File Sync selten der einzige Dienst ist, der in Ihrem Rechenzentrum ausgeführt wird, empfiehlt es sich, die Netzwerk- und Speichernutzung von Azure File Sync zu beschränken.

> [!Important]  
> Das Festlegen von zu niedrigen Grenzwerten wirkt sich auf die Leistung der Synchronisierung durch Azure File Sync und auf Rückrufe aus.

### <a name="set-azure-file-sync-network-limits"></a>Festlegen von Netzwerklimits für Azure File Sync
Sie können die Netzwerknutzung von Azure File Sync mithilfe der `StorageSyncNetworkLimit`-Cmdlets drosseln. 

Sie können beispielsweise ein neues Drossellimit erstellen, um sicherzustellen, dass Azure File Sync an Werktagen zwischen 9:00 und 17:00 Uhr nicht mehr als 10 MBit/s verbraucht: 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

Mit dem folgenden Cmdlet können Sie Ihr Limit anzeigen:

```PowerShell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

Verwenden Sie zum Entfernen von Netzwerklimits `Remove-StorageSyncNetworkLimit`. Mit dem folgenden Befehl werden z.B. alle Netzwerklimits entfernt:

```PowerShell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>Verwenden von QoS für Speicher für Windows Server 
Wenn Azure File Sync auf einem virtuellen Computer auf einem Windows Server-Virtualisierungshost ausgeführt wird, können Sie Quality of Service für Speicher verwenden, um die Speicher-E/A-Nutzung zu steuern. Die Richtlinie für QoS für Speicher kann entweder als Maximum (oder Grenzwert, wie das StorageSyncNetwork-Limit oben erzwungen wird) oder als Minimum (bzw. Reservierung) festgelegt werden. Das Festlegen eines Minimums anstelle eines Maximums ermöglicht Azure File Sync die Nutzung der verfügbaren Speicherbandbreite, wenn diese nicht von anderen Workloads genutzt wird. Weitere Informationen finden Sie unter [Quality of Service für Speicher](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview).

## <a name="see-also"></a>Weitere Informationen
- [Planen einer Bereitstellung von Azure File Sync (Vorschau)](storage-sync-files-planning.md)
- [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md) 
- [Problembehandlung bei Azure File Sync (Vorschau)](storage-sync-files-troubleshoot.md)
