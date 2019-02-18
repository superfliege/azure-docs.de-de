---
title: PowerShell-Beispiel zum Wiederherstellen einer Azure SQL-Datenbank aus einer Sicherung | Microsoft-Dokumentation
description: Azure PowerShell-Beispielskript zum Wiederherstellen einer Azure SQL-Einzeldatenbank aus georedundanten Sicherungen
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 730d944edfc16e9ef399333540b14fcfced4ce44
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996388"
---
# <a name="use-powershell-to-restore-an-azure-sql-single-database-from-backups"></a>Verwenden von PowerShell zum Wiederherstellen einer Azure SQL-Einzeldatenbank aus Sicherungen

Dieses PowerShell-Skriptbeispiel stellt eine Azure SQL-Datenbank aus einer georedundanten Sicherung, eine gelöschte Azure SQL-Datenbank auf den Stand der letzten Sicherung und eine Azure SQL-Datenbank auf den Stand eines bestimmten Zeitpunkts wieder her.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. | [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Erstellt einen SQL-Datenbankserver, der eine Einzeldatenbank oder einen Pool für elastische Datenbanken hostet. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Erstellt eine Datenbank auf einem SQL-Datenbankserver als Einzeldatenbank oder in einem Pool zusammengefasste Datenbank. |
[Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | Ruft eine georedundante Sicherung einer Einzeldatenbank oder in einem Pool zusammengefassten Datenbank ab. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | Stellt eine SQL-Einzeldatenbank oder in einem Pool zusammengefasste SQL-Datenbank wieder her. |
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | Entfernt eine SQL-Einzeldatenbank oder in einem Pool zusammengefasste SQL-Datenbank. |
| [Get-AzureRmSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Ruft eine gelöschte Einzeldatenbank oder in einem Pool zusammengefasste Datenbank ab, die Sie wiederherstellen können. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).
