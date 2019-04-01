---
title: 'Azure PowerShell-Skript: Erneutes Generieren von Azure Cosmos DB-Kontoschlüsseln'
description: 'Azure PowerShell-Skriptbeispiel: erneutes Generieren von Azure Cosmos DB-Kontoschlüsseln'
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
author: SnehaGunda
ms.author: sngun
ms.devlang: PowerShell
ms.topic: sample
ms.date: 05/10/2017
ms.reviewer: sngun
ms.openlocfilehash: bd4e512710130a59461e515792c1d6d216d02621
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496768"
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-powershell"></a>Erneutes Generieren von Azure Cosmos DB-Kontoschlüsseln mithilfe von PowerShell

Dieses Beispiel generiert alle Arten von Azure Cosmos DB-Kontoschlüsseln erneut mithilfe der Azure CLI.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/cosmosdb/regenerate-account-keys/regenerate-account-keys.ps1?highlight=36-41 "Regenerate Azure Cosmos DB account keys")]

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
| [Invoke-AzResourceAction](https://docs.microsoft.com/powershell/module/az.resources/invoke-azresourceaction) | Ruft eine Aktion für das Azure Cosmos DB-Konto auf |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../powershell-samples.md).
