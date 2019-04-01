---
title: 'Azure PowerShell-Skript: Replikation in mehreren Regionen für Azure Cosmos DB'
description: 'Azure PowerShell-Skriptbeispiel: Replikation in mehreren Regionen für Azure Cosmos DB'
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
author: SnehaGunda
ms.author: sngun
ms.devlang: PowerShell
ms.topic: sample
ms.date: 05/10/2017
ms.reviewer: sngun
ms.openlocfilehash: 02628401bed6e65784bf7ddc4a7082f617640cf9
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496143"
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a>Replizieren eines Azure Cosmos DB-Datenbankkontos in mehreren Regionen und Konfigurieren der Failoverprioritäten mithilfe von PowerShell

Dieses Beispiel repliziert alle Arten von Azure Cosmos DB-Datenbankkonten in mehreren Regionen und konfiguriert die Failoverprioritäten mithilfe von PowerShell. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/cosmosdb/replicate-database-multiple-regions/replicate-database-multiple-regions.ps1?highlight=37-44,47-48,51-55 "Replicate an Azure Cosmos DB account across multiple regions")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Erstellt einen logischen Server, der eine Datenbank oder einen Pool für elastische Datenbanken hostet. |
| [Set-AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource) | Ändert das Datenbankkonto. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../powershell-samples.md).
