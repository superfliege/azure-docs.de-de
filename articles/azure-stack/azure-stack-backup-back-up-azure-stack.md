---
title: Sichern von Azure Stack | Microsoft-Dokumentation
description: Führen Sie eine bedarfsgesteuerte Sicherung von Azure Stack durch, indem Sie den integrierten Sicherungsdienst nutzen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/08/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 2570423a3cae2a15f0cfd294f1d91e6730748f68
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38973058"
---
# <a name="back-up-azure-stack"></a>Sichern von Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Führen Sie eine bedarfsgesteuerte Sicherung von Azure Stack durch, indem Sie den integrierten Sicherungsdienst nutzen. Anweisungen zum Konfigurieren der PowerShell-Umgebung finden Sie unter [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md). Weitere Informationen zur Anmeldung bei Azure Stack finden Sie unter [Konfigurieren der Betreiberumgebung und Anmelden bei Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="start-azure-stack-backup"></a>Starten der Azure Stack-Sicherung

Verwenden Sie Start-AzSBackup, um eine neue Sicherung mit der -AsJob-Variablen zu starten und den Fortschritt nachzuverfolgen. 

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output
```

## <a name="confirm-backup-completed-via-powershell"></a>Bestätigen über PowerShell, dass die Sicherung abgeschlossen wurde

```powershell
    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

- Das Ergebnis sollte der folgenden Ausgabe ähneln:

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

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Bestätigen der erfolgreichen Sicherung im Verwaltungsportal

1. Rufen Sie das Azure Stack-Verwaltungsportal unter [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external) auf.
2. Wählen Sie **Weitere Dienste** > **Infrastructure Backup** aus. Klicken Sie auf der Registerkarte **Infrastruktursicherung** auf **Konfiguration**.
3. Suchen Sie in der Liste **Verfügbare Sicherungen** nach **Name** und **Abschlussdatum** der Sicherung.
4. Überprüfen Sie, ob der **Status** als **Erfolgreich** angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den Workflow zur Wiederherstellung nach einem Datenverlust. Siehe [Wiederherstellen nach schwerwiegendem Datenverlust](azure-stack-backup-recover-data.md).
