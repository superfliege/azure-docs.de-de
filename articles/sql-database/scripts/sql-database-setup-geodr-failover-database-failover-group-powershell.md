---
title: PowerShell-Beispiel – Failovergruppe bei der Georeplikation – einzelne Azure SQL-Datenbank | Microsoft-Dokumentation
description: Azure PowerShell-Beispielskript zum Einrichten einer Failovergruppe bei der aktiven Georeplikation für eine einzelne Azure SQL-Datenbank und zum Ausführen eines Failovers
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: d8ec80f417883874796d25c2c1a427d03073080b
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390766"
---
# <a name="use-powershell-to-configure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a>Verwenden von PowerShell zum Konfigurieren einer Failovergruppe bei der aktiven Georeplikation für eine einzelne Azure SQL-Datenbank

In diesem PowerShell-Beispielskript wird eine Failovergruppe bei der aktiven Georeplikation für eine einzelne Azure SQL-Datenbank konfiguriert und ein Failover für ein sekundäres Replikat von Azure SQL-Datenbank ausgeführt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-scripts"></a>Beispielskripts

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Erstellt einen logischen Server, der eine Datenbank oder einen Pool für elastische Datenbanken hostet. |
| [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Erstellt einen Pool für elastische Datenbanken auf einem logischen Server. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Aktualisiert Datenbankeigenschaften oder verschiebt eine Datenbank in Pools für elastische Datenbanken, daraus hinaus oder zwischen ihnen. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Erstellt eine sekundäre Datenbank für eine vorhandene Datenbank und startet die Datenreplikation. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Ruft mindestens eine Datenbank ab. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Erklärt die sekundäre Datenbank zur primären und wechselt zu ihr – dadurch wird das Failover gestartet.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Ruft die Georeplikationsverknüpfungen zwischen einer Azure SQL-Datenbank und einer Ressourcengruppe oder einer SQL Server-Instanz ab. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | Beendet die Datenreplikation zwischen einer SQL-Datenbank und der angegebenen sekundären Datenbank. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
| [New-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/new-azurermsqldatabasefailovergroup) | Erstellt eine neue Azure SQL-Datenbank-Failovergruppe für die angegebenen Server. |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Vertauscht die Rollen der Server in der Failovergruppe und legt alle sekundären Datenbanken auf die primäre Rolle fest. |
| [Get-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Ruft eine bestimmte Azure SQL-Datenbank-Failovergruppe ab oder listet die Failovergruppen auf einem Server auf. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).
