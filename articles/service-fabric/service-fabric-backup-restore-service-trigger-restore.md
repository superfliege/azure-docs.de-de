---
title: Wiederherstellen von Sicherungsdaten in Azure Service Fabric | Microsoft-Dokumentation
description: Verwenden Sie das Service Fabric-Feature zum regelmäßigen Sichern und Wiederherstellen, um Sicherungsdaten Ihrer Anwendungsdaten wiederherzustellen.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 802F55B6-6575-4AE1-8A8E-C9B03512FF88
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 69604decab354368f336b85bfa1497671f0c3101
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730601"
---
#  <a name="restoring-backup-in-azure-service-fabric"></a>Wiederherstellen von Sicherungsdaten in Azure Service Fabric


Zuverlässige zustandsbehaftete Dienste und Reliable Actors in Service Fabric können einen änderbaren, autoritativen Zustand über die Anforderung und die Antwort oder eine vollständige Transaktion hinaus beibehalten. Wenn ein zustandsbehafteter Dienst für längere Zeit ausfällt oder Informationen aufgrund eines Notfalls verloren gehen, muss der Dienst möglicherweise mit einer akzeptablen aktuellen Sicherung des Zustands wiederhergestellt werden, damit er wieder verfügbar ist, nachdem er erneut gestartet wurde.

Ein Dienst könnte Daten beispielsweise zum Schutz vor folgenden Szenarien sichern:

- Im Fall des dauerhaften Verlusts eines gesamten Service Fabric-Clusters **(Case of Disaster Recovery - DR)** (Fall einer Notfallwiederherstellung – DR)
- Dauerhafter Verlust eines Großteils der Replikate einer Dienstpartition. **(Case of data loss)** (Fall eines Datenverlusts)
- Administrative Fehler, durch die der Zustand versehentlich gelöscht oder beschädigt wird. Ein Administrator mit ausreichenden Berechtigungen löscht z. B. versehentlich den Dienst.**(Case of data loss)** (Fall eines Datenverlusts)
- Fehler im Dienst, die zu einer Beschädigung von Daten führen. Eine Datenbeschädigung kann beispielsweise bei einem Dienstcodeupgrade auftreten, wenn fehlerhafte Daten in eine „Reliable Collection“ (zuverlässige Sammlung) geschrieben werden. In diesem Fall müssen unter Umständen der Code und die Daten in einen früheren Zustand zurückversetzt werden. **(Case of data corruption) (Fall einer Datenbeschädigung)**


## <a name="prerequisites"></a>Voraussetzungen
* Zum Auslösen sollte die Wiederherstellungsoption für den _Fault Analysis Service (FAS)_ (Fehleranalysedienst) für den Cluster deaktiviert sein.
* Die wiederherzustellenden Sicherungsdaten sollten vom _Backup Restore Service (BRS)_ (Sicherungswiederherstellungsdienst) abgerufen worden sein.
* Die Wiederherstellung kann nur auf einer Partition ausgelöst werden.

## <a name="triggering-restore"></a>Auslösen der Wiederherstellung

Die Wiederherstellung kann für jedes der folgenden Szenarios erfolgen: 
* Datenwiederherstellung im Fall einer _Notfallwiederherstellung_ (DR)
* Datenwiederherstellung im Fall von _Datenbeschädigung / Datenverlust_



### <a name="data-restore-in-the-event-of-disaster-recovery-dr"></a>Datenwiederherstellung im Fall einer _Notfallwiederherstellung_ (DR)
Im Fall des Verlusts eines gesamten Service Fabric-Clusters, können die Daten für die Partitionen des zuverlässigen zustandsbehafteten Diensts und der Reliable Actors in einem alternativen Cluster wiederhergestellt werden. Die gewünschte Sicherung kann aus den aufgelisteten [Informationen zur GetBackup-API mit Sicherungsspeicher](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation) ausgewählt werden. Die Sicherungen können für eine Anwendung, einen Dienst oder eine Partition aufgelistet werden.

Nehmen wir an, bei dem verlorenen Cluster handle es sich um den im Abschnitt [Aktivieren der regelmäßigen Sicherung für den zuverlässigen zustandsbehafteten Dienst und Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors) genannten Cluster, welcher `SampleApp` bereitgestellt hat, während für die Partition eine Sicherungsrichtlinie aktiviert war und Sicherungen in Azure Storage durchgeführt wurden. 


Führen Sie das folgende PowerShell-Skript zum Aufrufen der REST-API aus, um die für alle Partitionen in der Anwendung `SampleApp` im verlorenen Service Fabric-Cluster erstellten Sicherungen aufzulisten. Damit verfügbare Sicherungen aufgelistet werden können, benötigt die Enumerations-API Informationen zum Speicherort der Sicherungen einer Anwendung. 

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$BackupEntity = @{
    EntityKind = 'Application'
    ApplicationName='fabric:/SampleApp'
}

$BackupLocationAndEntityInfo = @{
    Storage = $StorageInfo
    BackupEntity = $BackupEntity
}

$body = (ConvertTo-Json $BackupLocationAndEntityInfo)
$url = "https://myalternatesfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Beispielausgabe für die oben genannte Ausführung:

```
BackupId                : b9577400-1131-4f88-b309-2bb1e943322c
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 20.55.16.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3334
CreationTimeUtc         : 2018-04-06T20:55:16Z
FailureError            : 
*
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
*
BackupId                : 69436834-c810-4163-9386-a7a800f78359
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.25.36.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3764
CreationTimeUtc         : 2018-04-06T21:25:36Z
FailureError            : 
```



Zum Auslösen der Wiederherstellung müssen wir die gewünschte Sicherung auswählen. Nehmen wir an, die gewünschte Sicherung für die aktuelle Notfallwiederherstellung (DR) wäre die folgende:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Für die Wiederherstellungs-API müssen wir die Informationen zu __BackupId__ und __BackupLocation__ bereitstellen. Die Partition in einem alternativen Cluster muss entsprechend dem [partition scheme](service-fabric-concepts-partitioning.md#get-started-with-partitioning) (Partitionsschema) ausgewählt werden. Der Benutzer muss die Zielpartition für die Wiederherstellung der Sicherung aus dem alternativen Cluster entsprechend dem Partitionsschema des ursprünglichen, verlorenen Clusters auswählen.

Angenommen, die Partitions-ID für den alternativen Cluster wäre `1c42c47f-439e-4e09-98b9-88b8f60800c6`, und sie würde der Partitions-ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22` des ursprünglichen Clusters durch Vergleichen des hohen und niedrigen Schlüssels für _Ranged Partitioning (UniformInt64Partition)_ (Bereichsbasierte Partitionierung (UniformInt64Partition)) zugeordnet.

Für _Named Partitioning_ (Namensbasierte Partitionierung) wird der Name-Wert verglichen, um die Zielpartition im alternativen Cluster zu identifizieren.

Die Wiederherstellung der Partition des Sicherungsclusters wird mithilfe der folgenden [Restore API](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition) (Wiederherstellungs-API) angefordert.

```powershell 
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 
Der Fortschrittsstatus des Wiederherstellungsprozesses kann [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress) (Fortschritt des Wiederherstellungsprozesses verfolgen) lauten.

### <a name="data-restore-in-the-event-of-data-corruption--data-loss"></a>Datenwiederherstellung im Fall von _Datenbeschädigung / Datenverlust_

Im Fall von _data loss_ (Datenverlust) oder _data corruption_ (Datenbeschädigung) können die Daten für die Partitionen des zuverlässigen zustandsbehafteten Diensts und Reliable Actors mit jeder der ausgewählten Sicherungen wiederhergestellt werden. Der folgende Fall ist eine Kombination von Beispielen wie unter [Aktivieren der regelmäßigen Sicherung für den zuverlässigen zustandsbehafteten Dienst und Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors) beschrieben. Dort verfügt die Partition über eine aktivierte Sicherungsrichtlinie und wendet die Sicherung mit einer gewünschten Häufigkeit in einem Azure-Speicher an. 

Die gewünschte Sicherung wird aus der Ausgabe der [GetBackup-API](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups) ausgewählt. In diesem Szenario wird die Sicherung aus dem gleichen Cluster generiert wie in der Vergangenheit.
Zum Auslösen der Wiederherstellung müssen wir die gewünschte Sicherung aus der Liste auswählen. Nehmen wir an, die von uns gewünschte Sicherung für den aktuellen Fall _data loss_ / _data corruption_ (Datenverlust/Datenbeschädigung) wäre die folgende Sicherung:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Für die Wiederherstellungs-API müssen wir die Informationen zu __BackupId__ und __BackupLocation__ bereitstellen. Da für den Cluster die Sicherung aktiviert ist, identifiziert der _Backup Restore Service (BRS)_ (Sicherungswiederherstellungsdienst) in Service Fabric den korrekten Speicherort aus der zugeordneten Sicherungsrichtlinie.

```powershell
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4', 
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Der Fortschrittsstatus des Wiederherstellungsprozesses kann [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress) (Fortschritt des Wiederherstellungsprozesses verfolgen) lauten.


## <a name="tracking-restore-progress"></a>Überwachen des Wiederherstellungsfortschritts

Eine Partition eines zuverlässigen zustandsbehafteten Diensts oder Reliable Actor-Diensts akzeptiert nicht mehrere Anforderungen für eine Wiederherstellung gleichzeitig. Eine weitere Anforderung kann nur akzeptiert werden, wenn die aktuelle Anforderung für eine Wiederherstellung abgeschlossen ist. Mehrere Anforderungen für eine Wiederherstellung können gleichzeitig auf unterschiedlichen Partitionen ausgelöst werden.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
 
$restoreResponse = (ConvertFrom-Json $response.Content) 
$restoreResponse | Format-List
```

Die Anforderung für eine Wiederherstellung erfolgt in dieser Reihenfolge:

1. __Accepted__ (Akzeptiert): Der Wiederherstellungszustand _Accepted_ (Akzeptiert) zeigt an, dass die angeforderte Wiederherstellung mit korrekten Anforderungsparametern ausgelöst wurde.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
2. __InProgress__ (In Bearbeitung): Der Wiederherstellungszustand _InProgress_ (In Bearbeitung) zeigt an, dass die Partition mit der in der Anforderung genannten Sicherung wiederhergestellt wird. Die Partition meldet den Zustand _dataloss_ (Datenverlust).
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. __Success__/ __Failure__/ __Timeout__ (Erfolg/Fehler/Timeout): Eine angeforderte Wiederherstellung kann in jedem der folgenden Zustände abgeschlossen werden. Die Bedeutung und die Antwortdetails der einzelnen Zustände werden im Folgenden erläutert.
       
    * __Success__ (Erfolg): Der Wiederherstellungszustand _Success_ (Erfolg) zeigt an, dass der Partitionszustand wiederhergestellt wurde. In der Antwort werden „RestoreEpoch“ und „RestordLSN“ für die Partition zusammen mit der Zeitangabe in UTC bereitgestellt. 
    
        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```
        
    *. __Failure__ (Fehler): Der Wiederherstellungszustand _Failure_ (Fehler) zeigt an, dass die Wiederherstellungsanforderung nicht erfolgreich war. Die Ursache des Fehlers wird in der Anforderung angegeben.
        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    *. __Timeout__: Der Wiederherstellungszustand _Timeout_ zeigt an, dass die Anforderung abgelaufen ist. Empfehlenswert ist eine neue Wiederherstellungsanforderung mit größerem Wert für [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) (Timeout bei Wiederherstellung); Standardmäßig beträgt der Timeout 10 Minuten. Es sollte sichergestellt werden, dass sich die Partition nicht im Zustand „Datenverlust“ befindet, bevor eine erneute Wiederherstellung angefordert wird.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="auto-restore"></a>Automatische Wiederherstellung

Die Partitionen für den zuverlässigen zustandsbehafteten Dienst und Reliable Actors im Service Fabric-Cluster können mit der Einstellung _auto restore_ (automatische Wiederherstellung) konfiguriert werden. Beim Erstellen der Sicherungsrichtlinie kann die Einstellung `AutoRestore` in der Richtlinie auf _TRUE_ festgelegt werden.  Wird die Einstellung _auto restore_ (automatische Wiederherstellung) für eine Partition aktiviert, werden im Fall eines gemeldeten Datenverlusts die Daten aus der aktuellsten Sicherung wiederhergestellt.
 
 [Auto Restore Enablement in Backup Policy (Aktivieren der automatischen Wiederherstellung in einer Sicherungsrichtlinie)](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)


[RestorePartition API reference](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
[GetPartitionRestoreProgress API reference](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Nächste Schritte
- [Grundlegendes zur Konfiguration der regelmäßigen Sicherung](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [REST-API-Referenz zu Sicherung/Wiederherstellung](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
