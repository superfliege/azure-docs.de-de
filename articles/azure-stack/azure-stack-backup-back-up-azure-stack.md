---
title: Sichern von Azure Stack | Microsoft-Dokumentation
description: "Führen Sie eine bedarfsgesteuerte Sicherung von Azure Stack durch, indem Sie den integrierten Sicherungsdienst nutzen."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: df1f4c6fadd08b17a1a1eb8bbe41ab71ae4729fc
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="back-up-azure-stack"></a>Sichern von Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Führen Sie eine bedarfsgesteuerte Sicherung von Azure Stack durch, indem Sie den integrierten Sicherungsdienst nutzen. Wenn Sie den Dienst für die Infrastruktursicherung aktivieren müssen, finden Sie weitere Informationen unter [Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal](azure-stack-backup-enable-backup-console.md).

## <a name="start-azure-stack-backup"></a>Starten der Azure Stack-Sicherung

Öffnen Sie Windows PowerShell mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:

   ```powershell
   Start-AzSBackup -Location $location
   ```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Bestätigen der erfolgreichen Sicherung im Verwaltungsportal

1. Öffnen Sie das Azure Stack-Administratorportal unter [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Wählen Sie **Weitere Dienste** > **Infrastruktursicherung** aus. Klicken Sie auf der Registerkarte **Infrastruktursicherung** auf **Konfiguration**.
3. Suchen Sie in der Liste **Verfügbare Sicherungen** nach **Name** und **Abschlussdatum** der Sicherung.
4. Überprüfen Sie, ob der **Status** als **Erfolgreich** angezeigt wird.

Sie können den erfolgreichen Abschluss der Sicherung auch über das Verwaltungsportal bestätigen. Navigieren Sie zu `\MASBackup\<datetime>\<backupid>\BackupInfo.xml`.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den Workflow zur Wiederherstellung nach einem Datenverlust. Siehe [Wiederherstellen nach schwerwiegendem Datenverlust](azure-stack-backup-recover-data.md).