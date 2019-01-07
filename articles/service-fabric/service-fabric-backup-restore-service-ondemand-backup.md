---
title: Bedarfsgesteuerte Sicherung in Azure Service Fabric | Microsoft-Dokumentation
description: Verwenden Sie das Feature für Sicherungen und Wiederherstellungen von Service Fabric, um eine Sicherung Ihrer Anwendungsdaten auf Bedarfsbasis zu ermöglichen.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 02DA262A-EEF6-4F90-842E-FFC4A09003E5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 35d97f1da6b5d1c75073264c70e1cd1607d5fe0d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730586"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Bedarfsgesteuerte Sicherung in Azure Service Fabric

Die Daten zuverlässiger zustandsbehafteter Dienste sowie von Reliable Actors können gesichert werden, um für Notfall- oder Datenverlustszenarios verwendet zu werden.

Service Fabric verfügt über Features zur [regelmäßigen Sicherung von Daten](service-fabric-backuprestoreservice-quickstart-azurecluster.md) und zur Sicherung von Daten auf Bedarfsbasis. Die bedarfsgesteuerte Sicherung ist nützlich, da sie vor _Datenverlust bzw. Datenbeschädigung_ aufgrund von geplanten Änderungen im zugrunde liegenden Dienst oder dessen Umgebung schützt.

Die Features zur bedarfsgesteuerten Sicherung sind für die Erfassung von Dienstzuständen hilfreich, noch bevor ein Vorgang im Zusammenhang mit dem Dienst oder der Dienstumgebung manuell ausgelöst wird. Das kann z.B. das Ändern der Dienstbinärdateien sein, also etwa ein Upgrade oder Downgrade des Diensts, da die Daten gegen Datenbeschädigung durch Fehler im Anwendungscode geschützt sind.

## <a name="triggering-on-demand-backup"></a>Auslösen der bedarfsgesteuerten Sicherung

Die bedarfsgesteuerte Sicherung erfordert Speicherdetails zum Hochladen von Sicherungsdateien. Die bedarfsgesteuerte Sicherung wird im Speicher ausgeführt, der in der regelmäßig aktualisierten Sicherungsrichtlinie angegeben ist, oder im angegebenen Speicher in der bedarfsgesteuerten Sicherungsanforderung.

### <a name="on-demand-backup-to-storage-specified-by-periodic-backup-policy"></a>Bedarfsgesteuerte Sicherung in einem angegebenen Speicher

Die bedarfsgesteuerte Sicherung kann für eine Partition eines zuverlässigen zustandsbehafteten Diensts oder eines Reliable Actors gemeinsam mit den Speicherinformationen angefordert werden. 

Im folgenden Fall wird das unter [Aktivieren der regelmäßigen Sicherung für den zuverlässigen zustandsbehafteten Dienst und Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors) beschriebene Beispiel fortgeführt. Dort verfügt die Partition über eine aktivierte Sicherungsrichtlinie und wendet die Sicherung mit einer gewünschten Häufigkeit in Azure Storage an.

Die bedarfsgesteuerte Sicherung für die Partitions-ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22` kann durch die [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition)-API ausgelöst werden. 

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Der [Status der bedarfsgesteuerten Sicherung](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress) kann durch die [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress)-API nachverfolgt werden.

### <a name="on-demand-backup-to-specified-storage"></a>Bedarfsgesteuerte Sicherung auf dem angegebenen Speicher

Die bedarfsgesteuerte Sicherung kann für eine Partition eines zuverlässigen zustandsbehafteten Diensts oder eines Reliable Actor-Diensts zusammen mit den Speicherinformationen angefordert werden. Die Speicherinformationen sollten als Teil der bedarfsgesteuerten Sicherungsanforderung bereitgestellt werden.

Die bedarfsgesteuerte Sicherung für die Partitions-ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22` kann durch die [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition)-API mit den wie unten dargestellten Azure Storage-Informationen ausgelöst werden.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Der [Status der bedarfsgesteuerten Sicherung](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress) kann durch die [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress)-API nachverfolgt werden.


## <a name="tracking-on-demand-backup-progress"></a>Nachverfolgung des Prozesses der bedarfsgesteuerten Sicherung

Eine Partition eines zuverlässigen zustandsbehafteten Diensts oder Reliable Actor-Diensts akzeptiert nur eine Anforderung für die bedarfsgesteuerte Sicherung auf einmal. Eine andere Anforderung kann nur akzeptiert werden, wenn die aktuelle Anforderung der bedarfsgesteuerten Sicherung abgeschlossen ist. 

Mehrere Anforderungen der bedarfsgesteuerten Sicherung können gleichzeitig auf unterschiedlichen Partitionen ausgelöst werden.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

Der Fortschritt der Anforderung der bedarfsgesteuerten Sicherung befindet sich in einem der folgenden Status:

* **Accepted** (Akzeptiert): Die Sicherung wurde auf der Partition initiiert und wird nun ausgeführt.
    ```
    BackupState             : Accepted
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          : 
    EpochOfLastBackupRecord : 
    LsnOfLastBackupRecord   : 0
    FailureError            : 
    ```
    
* **Success/Failure/Timeout** (Erfolg/Fehler/Timeout): Eine angeforderte bedarfsgesteuerte Sicherung kann in jedem der folgenden Status abgeschlossen werden. Die Bedeutung und die Antwortdetails der einzelnen Zustände werden im Folgenden erläutert.

    * **Success** (Erfolg): Der Sicherungsstatus _Success_ gibt an, dass der Partitionsstatus erfolgreich gesichert wurde. In der Antwort werden __BackupEpoch__ und __BackupLSN__ für die Partition zusammen mit der Zeitangabe in UTC bereitgestellt.
        ```
        BackupState             : Success
        TimeStampUtc            : 2018-11-21T20:00:01Z
        BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
        BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
        EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
        LsnOfLastBackupRecord   : 36
        FailureError            : 
        ```
        
    * **Failure** (Fehler): Der Sicherungsstatus _Failure_ gibt an, dass während der Sicherung des Status der Partition ein Fehler aufgetreten ist. Die Ursache des Fehlers wird in der Antwort angegeben.
        ```
        BackupState             : Failure
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
        ```
       
    * **Timeout**: Der Sicherungsstatus _Timeout_ gibt an, dass die Sicherung des Partitionsstatus nicht im angegebenen Zeitrahmen durchgeführt werden konnte. Der Standardtimeoutwert beträgt 10 Minuten. In einem solchen Szenario wird empfohlen, in Anforderungen für bedarfsgesteuerte Sicherungen eine neue Sicherungsanforderung mit größerem [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) zu initiieren.

        ```
        BackupState             : Timeout
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
        ```

## <a name="next-steps"></a>Nächste Schritte
- [Grundlegendes zur Konfiguration der regelmäßigen Sicherung](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [REST-API-Referenz zu Sicherung/Wiederherstellung](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

