---
title: 'PowerShell-Beispiel: Erstellen einer verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation'
description: Azure PowerShell-Beispielskript zum Erstellen einer verwalteten Azure SQL-Datenbank-Instanz
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: ''
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: bbb22f8d5eab3e60eba13bb642edb8f8d190cf67
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54387930"
---
# <a name="use-powershell-with-azure-resource-manager-template-to-create-an-azure-sql-database-managed-instance"></a>Verwenden von PowerShell mit einer Azure Resource Manager-Vorlage zum Erstellen einer verwalteten Azure SQL-Datenbank-Instanz

Eine verwaltete Azure SQL-Datenbank-Instanz kann mit einer Azure PowerShell-Bibliothek und Azure Resource Manager-Vorlagen erstellt werden. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

Mit Azure PowerShell-Befehlen kann die Bereitstellung gestartet werden, indem eine vordefinierte Azure Resource Manager-Vorlage verwendet wird. Die folgenden Eigenschaften können in der Vorlage angegeben werden:
- Instanzname
- Benutzername und Kennwort des SQL-Administrators 
- Größe der Instanz (Anzahl von Kernen und maximale Speichergröße)
- VNET und Subnetz, in dem die Instanz angeordnet wird
- Sortierung der Instanz auf Serverebene (Vorschauversion)

Der Instanzname, der Benutzername des SQL-Administrators, das VNET/Subnetz und die Sortierung können später nicht mehr geändert werden. Andere Eigenschaften der Instanz können geändert werden.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Beispiel wird vorausgesetzt, dass Sie für Ihre verwaltete Instanz [eine gültige Netzwerkumgebung erstellt](../sql-database-managed-instance-create-vnet-subnet.md) oder [ein vorhandenes VNET geändert](../sql-database-managed-instance-configure-vnet-subnet.md) haben. Da im Beispiel die Cmdlets [New-AzureRmResourceGroupDeployment](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) und [Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) verwendet werden, stellen Sie sicher, dass Sie die folgenden PowerShell-Module installiert haben:

```
Install-Module AzureRM.Network
Install-Module AzureRM.Resources
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage

Der folgende Inhalt sollte in einer Datei angeordnet werden, die als Vorlage für die Erstellung der Instanz dient:
```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "instance": {
            "type": "string"
        },
        "user": {
            "type": "string"
        },
        "pwd": {
            "type": "securestring"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('instance')]",
            "location": "West Central US",
            "tags": {
                "Description":"GP Instance with custom instance collation - Serbian_Cyrillic_100_CS_AS"
            },          
            "sku": {
                "name": "GP_Gen4",
                "tier": "GeneralPurpose"
            },
            "properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen4", 
                "collation": "Serbian_Cyrillic_100_CS_AS"
            },
            "type": "Microsoft.Sql/managedInstances",
            "identity": {
                "type": "SystemAssigned"
            },
            "apiVersion": "2015-05-01-preview"
        }
    ]
}
```
Es wird vorausgesetzt, dass Azure VNET mit dem richtig konfigurierten Subnetz bereits vorhanden ist. Wenn dies nicht der Fall ist, sollten Sie die Netzwerkumgebung mit einer separaten [Azure Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) vorbereiten, die unabhängig ausgeführt oder in diese Vorlage einbezogen werden kann.

Speichern Sie den Inhalt dieser Datei als JSON-Datei, fügen Sie den Dateipfad in das folgende PowerShell-Skript ein, und ändern Sie die Namen der Objekte im Skript: 

```powershell
$subscriptionId = "ed827499-xxxx-xxxx-xxxx-xxxxxxxxxx"
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Managed Instance properties
$resourceGroup = "rg_mi"
$location = "West Central US"
$name = "managed-instance-name"
$user = "miSqlAdmin"
$secpasswd = ConvertTo-SecureString "<Put some strong password here>" -AsPlainText -Force

# Network configuration
$vNetName = "my_vnet"
$vNetResourceGroup = "rg_mi_vnet"
$subnetName = "ManagedInstances"
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $vNetResourceGroup
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vNet
$subnetId = $subnet.Id

# Deploy Instance using Azure Resource Manager template:
New-AzureRmResourceGroupDeployment  -Name MyDeployment -ResourceGroupName $resourceGroup  `
                                    -TemplateFile 'C:\...\create-managed-instance.json' `
                                    -instance $name -user $user -pwd $secpasswd -subnetId $subnetId
```
Nach erfolgreicher Ausführung des Skripts können alle Azure-Dienste und die konfigurierte IP-Adresse auf die SQL-Datenbank zugreifen. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).



