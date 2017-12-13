---
title: "Bereitstellen von Azure-Ressourcen für mehrere Abonnements und Ressourcengruppen | Microsoft-Dokumentation"
description: "Hier wird gezeigt, wie während der Bereitstellung mehrere Azure-Abonnements und -Ressourcengruppen als Ziel festgelegt werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2017
ms.author: tomfitz
ms.openlocfilehash: 763f46b9b5be7edf06ee0604bfc51a2482405b60
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Bereitstellen von Azure-Ressourcen für mehrere Abonnements oder Ressourcengruppen

In der Regel stellen Sie alle Ressourcen in der Vorlage als einzelne Ressourcengruppe bereit. Es gibt jedoch Szenarien, bei denen Sie eine Reihe von Ressourcen zwar gemeinsam, aber in verschiedenen Ressourcengruppen oder Abonnements bereitstellen möchten. Sie möchten beispielsweise den virtuellen Sicherungscomputer für Azure Site Recovery in einer separaten Ressourcengruppe und an einem separaten Standort bereitstellen. Resource Manager ermöglicht die Verwendung geschachtelter Vorlagen, um nicht das Abonnement und die Ressourcengruppe, die für die übergeordnete Vorlage verwendet werden, sondern andere Abonnements und Ressourcengruppen als Ziel festzulegen.

Die Ressourcengruppe ist der Lebenszykluscontainer für die Anwendung und ihre Ressourcensammlung. Sie erstellen die Ressourcengruppe außerhalb der Vorlage und geben die gewünschte Ressourcengruppe für die Bereitstellung an. Eine Einführung zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](resource-group-overview.md).

## <a name="specify-a-subscription-and-resource-group"></a>Angeben eines Abonnements und einer Ressourcengruppe

Um eine andere Ressource als Ziel festzulegen, müssen Sie während der Bereitstellung eine geschachtelte oder verknüpfte Vorlage verwenden. Der Ressourcentyp `Microsoft.Resources/deployments` stellt Parameter für `subscriptionId` und `resourceGroup` bereit. Diese Eigenschaften ermöglichen Ihnen die Angabe eines anderen Abonnements und einer anderen Ressourcengruppe für die geschachtelte Bereitstellung. Alle Ressourcengruppen müssen vorhanden sein, bevor Sie die Bereitstellung ausführen. Ohne Angabe von Abonnement-ID oder Ressourcengruppe werden das Abonnement und die Ressourcengruppe aus der übergeordneten Vorlage verwendet.

Im folgenden Beispiel werden zwei Speicherkonten bereitgestellt: eins in der während der Bereitstellung angegebenen Ressourcengruppe und eins in einer im Parameter `secondResourceGroup` angegebenen Ressourcengruppe:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        },
        "secondResourceGroup": {
            "type": "string"
        },
        "secondSubscriptionID": {
            "type": "string",
            "defaultValue": ""
        },
        "secondStorageLocation": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
        "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
    },
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "[parameters('secondResourceGroup')]",
            "subscriptionId": "[parameters('secondSubscriptionID')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
                            "location": "[parameters('secondStorageLocation')]",
                            "sku":{
                                "name": "Standard_LRS"
                            },
                            "kind": "Storage",
                            "properties": {
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        }
    ]
}
```

Wenn Sie für `resourceGroup` den Namen einer nicht vorhandenen Ressourcengruppe angeben, tritt bei der Bereitstellung ein Fehler auf.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Verwenden Sie zum Bereitstellen der Beispielvorlage eine Azure PowerShell- oder Azure CLI-Version von Mai 2017 oder später. Verwenden Sie für diese Beispiele die [abonnementübergreifende Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) in GitHub.

### <a name="two-resource-groups-in-the-same-subscription"></a>Zwei Ressourcengruppen im gleichen Abonnement

PowerShell: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Ressourcengruppen im gleichen Abonnement Folgendes:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzureRmResourceGroup -Name $firstRG -Location southcentralus
New-AzureRmResourceGroup -Name $secondRG -Location eastus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

Azure CLI: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Ressourcengruppen im gleichen Abonnement Folgendes:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Nach Abschluss der Bereitstellung werden zwei Ressourcengruppen angezeigt. Jede Ressourcengruppe enthält ein Speicherkonto.

### <a name="two-resource-groups-in-different-subscriptions"></a>Zwei Ressourcengruppen in unterschiedlichen Abonnements

PowerShell: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Abonnements Folgendes:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzureRmSubscription -Subscription $secondSub
New-AzureRmResourceGroup -Name $secondRG -Location eastus

Select-AzureRmSubscription -Subscription $firstSub
New-AzureRmResourceGroup -Name $firstRG -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

Azure CLI: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Abonnements Folgendes:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

## <a name="use-the-resourcegroup-function"></a>Verwenden der resourceGroup()-Funktion

Für ressourcengruppenübergreifende Bereitstellungen wird die [resouceGroup()-Funktion](resource-group-template-functions-resource.md#resourcegroup) anders aufgelöst, je nachdem, wie Sie die geschachtelte Vorlage festlegen. 

Wenn Sie eine Vorlage in eine andere Vorlage einbetten, wird „resouceGroup()“ in der geschachtelten Vorlage in die übergeordnete Ressourcengruppe aufgelöst. Eine eingebettete Vorlage verwendet das folgende Format:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

Wenn Sie einen Link zu einer getrennten Vorlage einrichten, wird „resouceGroup()“ in der verlinkten Vorlage in die geschachtelte Ressourcengruppe aufgelöst. Eine verlinkte Vorlage verwendet das folgende Format:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

Um die verschiedenen Optionen zur Auflösung von `resourceGroup()` zu testen, stellen Sie eine [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) bereit, die das Ressourcengruppenobjekt für die übergeordnete Vorlage, die Inlinevorlage und die verknüpfte Vorlage zurückgibt. Die übergeordnete Vorlage und die Inlinevorlage werden beide in die gleiche Ressourcengruppe aufgelöst. Die verknüpfte Vorlage wird in die verknüpfte Ressourcengruppe aufgelöst.

Verwenden Sie für PowerShell Folgendes:

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

## <a name="next-steps"></a>Nächste Schritte

* Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md).
