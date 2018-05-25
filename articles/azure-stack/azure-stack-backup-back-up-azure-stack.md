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
ms.openlocfilehash: c2a6727692a7a74b3e5fe32de8800722a9ed91b5
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="back-up-azure-stack"></a>Sichern von Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Führen Sie eine bedarfsgesteuerte Sicherung von Azure Stack durch, indem Sie den integrierten Sicherungsdienst nutzen. Wenn Sie den Dienst für die Infrastruktursicherung aktivieren müssen, finden Sie weitere Informationen unter [Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  Anweisungen zum Konfigurieren der PowerShell-Umgebung finden Sie unter [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md).

## <a name="start-azure-stack-backup"></a>Starten der Azure Stack-Sicherung

Öffnen Sie Windows PowerShell in der Umgebung für die Bedienerverwaltung mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:

```powershell
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Bestätigen der erfolgreichen Sicherung im Verwaltungsportal

1. Öffnen Sie das Azure Stack-Verwaltungsportal unter [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Wählen Sie **Weitere Dienste** > **Infrastructure Backup** aus. Klicken Sie auf der Registerkarte **Infrastruktursicherung** auf **Konfiguration**.
3. Suchen Sie in der Liste **Verfügbare Sicherungen** nach **Name** und **Abschlussdatum** der Sicherung.
4. Überprüfen Sie, ob der **Status** als **Erfolgreich** angezeigt wird.

<!-- You can also confirm the backup completed from the administration portal. Navigate to `\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

In ‘Confirm backup completed’ section, the path at the end doesn’t make sense (ie relative to what, datetime format, etc?)
\MASBackup\<datetime>\<backupid>\BackupInfo.xml -->


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den Workflow zur Wiederherstellung nach einem Datenverlust. Siehe [Wiederherstellen nach schwerwiegendem Datenverlust](azure-stack-backup-recover-data.md).
