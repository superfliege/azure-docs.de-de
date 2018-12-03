---
title: 'Azure PowerShell-Skriptbeispiel: Wiederherstellen einer Web-App aus einer Sicherung in einem anderen Abonnement | Microsoft-Dokumentation'
description: 'Azure PowerShell-Skriptbeispiel: Wiederherstellen einer Web-App aus einer Sicherung in einem anderen Abonnement'
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 7ed593e408b57246ef155ff8e36f054aacb2e063
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291578"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription"></a>Wiederherstellen einer Web-App aus einer Sicherung in einem anderen Abonnement

Mit dem folgenden Skriptbeispiel wird eine zuvor erstellte Sicherung aus einer vorhandenen Web-App abgerufen und in einer Web-App in einem anderen Abonnement wiederhergestellt. 

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Connect-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen. 

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore-diff-sub/backup-restore-diff-sub.ps1?highlight=1-6 "Restore a web app from a backup in another subscription")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Wenn Sie die Web-App nicht mehr benötigen, entfernen Sie mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen:

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) | Fügt ein authentifiziertes Konto hinzu, das für Cmdlet-Anforderungen von Azure Resource Manager verwendet werden soll.  |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Ruft eine Liste der Sicherungen für eine Web-App ab. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Erstellt eine Web-App. |
| [Restore-AzureRmWebAppBackup](/powershell/module/azurerm.websites/restore-azurermwebappbackup) | Stellt eine Web-App aus einer zuvor ausgeführten Sicherung wieder her. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../app-service-powershell-samples.md).
