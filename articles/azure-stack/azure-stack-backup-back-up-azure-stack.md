---
title: Sichern von Azure Stack | Microsoft-Dokumentation
description: Führen Sie eine bedarfsgesteuerte Sicherung von Azure Stack durch, indem Sie den integrierten Sicherungsdienst nutzen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 578bb864f56b788db77d1201533e73d3b9616669
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "41947934"
---
# <a name="back-up-azure-stack"></a>Sichern von Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Führen Sie eine bedarfsgesteuerte Sicherung von Azure Stack durch, indem Sie den integrierten Sicherungsdienst nutzen. Anweisungen zum Konfigurieren der PowerShell-Umgebung finden Sie unter [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md). Informationen zur Anmeldung bei Azure Stack finden Sie unter [Verwenden des Administratorportals in Azure Stack](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Starten der Azure Stack-Sicherung

### <a name="start-a-new-backup-without-job-progress-tracking"></a>Starten einer neuen Sicherung ohne Nachverfolgung des Auftragsstatus
Verwenden Sie „Start-AzSBackup“, um eine neue Sicherung sofort und ohne Nachverfolgung des Auftragsstatus zu starten.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>Starten der Azure Stack-Sicherung mit Nachverfolgung des Auftragsstatus
Verwenden Sie „Start-AzSBackup“, um eine neue Sicherung mit der Variable „-AsJob“ zu starten und den Fortschritt nachzuverfolgen.

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob 
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " `
    + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) `
    -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output

    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

## <a name="confirm-backup-has-completed"></a>Sicherstellen, dass die Sicherung abgeschlossen wurde

### <a name="confirm-backup-has-completed-using-powershell"></a>Mithilfe von PowerShell sicherstellen, dass die Sicherung abgeschlossen wurde
Verwenden Sie die folgenden PowerShell-Befehle, um sicherzustellen, dass die Sicherung erfolgreich abgeschlossen wurde:

```powershell
   Get-AzsBackup
```

Das Ergebnis sollte der folgenden Ausgabe ähneln:

```powershell
    BackupDataVersion : 1.0.1
    BackupId          : <backup ID>
    RoleStatus        : {NRP, SRP, CRP, KeyVaultInternalControlPlane...}
    Status            : Succeeded
    CreatedDateTime   : 7/6/2018 6:46:24 AM
    TimeTakenToCreate : PT20M32.364138S
    DeploymentID      : <deployment ID>
    StampVersion      : 1.1807.0.41
    OemVersion        : 
    Id                : /subscriptions/<subscription ID>/resourceGroups/System.local/providers/Microsoft.Backup.Admin/backupLocations/local/backups/<backup ID>
    Name              : local/<local name>
    Type              : Microsoft.Backup.Admin/backupLocations/backups
    Location          : local
    Tags              : {}
```

### <a name="confirm-backup-has-completed-in-the-administration-portal"></a>Im Verwaltungsportal sicherstellen, dass die Sicherung abgeschlossen wurde
Führen Sie im Azure Stack-Verwaltungsportal diese Schritte aus, um sicherzustellen, dass die Sicherung erfolgreich abgeschlossen wurde:

1. Öffnen Sie das [Azure Stack-Verwaltungsportal](azure-stack-manage-portals.md).
2. Wählen Sie **Weitere Dienste** > **Infrastructure Backup** aus. Klicken Sie auf der Registerkarte **Infrastruktursicherung** auf **Konfiguration**.
3. Suchen Sie in der Liste **Verfügbare Sicherungen** nach **Name** und **Abschlussdatum** der Sicherung.
4. Überprüfen Sie, ob der **Status** als **Erfolgreich** angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den Workflow zur Wiederherstellung nach einem Datenverlust. Siehe [Wiederherstellen nach schwerwiegendem Datenverlust](azure-stack-backup-recover-data.md).
