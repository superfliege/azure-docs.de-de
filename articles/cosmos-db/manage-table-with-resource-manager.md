---
title: Azure Resource Manager-Vorlagen für die Tabellen-API von Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der Tabellen-API von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: 33e47d67365e76142d5b584d49d8e7265445bf03
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080462"
---
# <a name="create-azure-cosmos-db-table-api-resources-from-a-resource-manager-template"></a>Erstellen von Ressourcen für die Tabellen-API von Azure Cosmos DB anhand einer Resource Manager-Vorlage

Erfahren Sie, wie Sie mit einer Azure Resource Manager-Vorlage Ressourcen für die Tabellen-API von Azure Cosmos DB erstellen. Im folgenden Beispiel wird eine Tabellen-API für Azure Cosmos DB anhand einer [Azure-Schnellstartvorlage](https://aka.ms/table-arm-qs) erstellt. Diese Vorlage erstellt ein Azure Cosmos-Konto für die Tabellen-API mit einer Tabelle mit einem Durchsatz von 400 RU/s (Ressourceneinheiten pro Sekunde).

Hier ist eine Kopie der Vorlage angegeben:

[!code-json[create-cosmos-table](~/quickstart-templates/101-cosmosdb-table/azuredeploy.json)]

## <a name="deploy-via-powershell"></a>Bereitstellen über PowerShell

Um die Resource Manager-Vorlage mit PowerShell bereitzustellen, **kopieren** Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$tableName = Read-Host -Prompt "Enter the table name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json" `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -tableName $tableName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Wenn Sie eine lokal installierte Version von PowerShell anstelle von Azure Cloud Shell verwenden möchten, müssen Sie das Azure PowerShell-Modul [installieren](/powershell/azure/install-az-ps). Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu finden. 

Im vorherigen Beispiel haben Sie auf eine Vorlage verwiesen, die auf GitHub gespeichert ist. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.


## <a name="deploy-via-azure-cli"></a>Bereitstellen über Azure CLI

Um die Resource Manager-Vorlage mit der Azure CLI bereitzustellen, **kopieren** Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the table name: ' tableName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion tableName=$tableName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Der Befehl `az cosmosdb show` zeigt das neu erstellte Azure Cosmos-Konto, nachdem es bereitgestellt wurde. Wenn Sie eine lokal installierte Version von Azure CLI anstelle von CloudShell verwenden möchten, lesen Sie den Artikel [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/).

Im vorherigen Beispiel haben Sie auf eine Vorlage verwiesen, die auf GitHub gespeichert ist. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.


## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

- [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/)
- [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
- [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)