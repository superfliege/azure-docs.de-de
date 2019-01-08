---
title: 'Azure PowerShell-Skript: Erstellen eines API-Kontos für MongoDB von Azure Cosmos DB'
description: 'Azure PowerShell-Skriptbeispiel: Erstellen eines API-Kontos für MongoDB von Azure Cosmos DB'
services: cosmos-db
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.date: 05/29/2018
ms.openlocfilehash: c12f2f86e56ac90fbea366e43959dd934c89d931
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811051"
---
# <a name="create-an-azure-cosmos-db-account-with-azure-cosmos-dbs-api-for-mongodb-using-powershell"></a>Erstellen eines Azure Cosmos DB-Kontos mit der API für MongoDB von Azure Cosmos DB mithilfe PowerShell

Dieses PowerShell-Beispielskript erstellt ein Cosmos-Konto mit der API für MongoDB von Azure Cosmos DB. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-and-configure-mongodb-database/create-and-configure-mongodb-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | Erstellt einen logischen Server, der eine Datenbank oder einen Pool für elastische Datenbanken hostet. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../powershell-samples.md).