---
title: 'PowerShell-Beispiel: Kopieren einer Azure SQL-Datenbank auf einen neuen Server | Microsoft-Dokumentation'
description: Kopieren einer SQL-Datenbank auf einen neuen Server mit einem Azure PowerShell-Beispielskript
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: douglaslMS
ms.author: douglasl
ms.reviewer: carlrab
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: 410f199dc056ce77d01c5097f75315be3c7ada91
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389371"
---
# <a name="use-powershell-to-copy-a-sql-database-to-a-new-server"></a>Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell

In diesem PowerShell-Skriptbeispiel wird eine Kopie einer vorhandenen Datenbank auf einem neuen Server erstellt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="copy-a-database-to-a-new-server"></a>Kopieren einer Datenbank auf einen neuen Server

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database to new server")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $sourceresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $targetresourcegroupname
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Erstellt einen logischen Server, der eine Datenbank oder einen Pool für elastische Datenbanken hostet. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Erstellt eine Datenbank auf einem logischen Server als einzelne oder zu einem Pool gehörende Datenbank. |
| [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | Erstellt eine Kopie einer Datenbank, die die Momentaufnahme zum jetzigen Zeitpunkt verwendet. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).
