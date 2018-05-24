---
title: Bereitstellen von Azure-Ressourcen für mehrere Abonnements und Ressourcengruppen | Microsoft-Dokumentation
description: Hier wird gezeigt, wie während der Bereitstellung mehrere Azure-Abonnements und -Ressourcengruppen als Ziel festgelegt werden.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2018
ms.author: tomfitz
ms.openlocfilehash: c7e9807e7195be47bf7874837ff9428c90abbcee
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Bereitstellen von Azure-Ressourcen für mehrere Abonnements oder Ressourcengruppen

In der Regel stellen Sie alle Ressourcen in der Vorlage als einzelne [Ressourcengruppe](resource-group-overview.md) bereit. Es gibt jedoch Szenarien, bei denen Sie eine Reihe von Ressourcen zwar gemeinsam, aber in verschiedenen Ressourcengruppen oder Abonnements bereitstellen möchten. Sie möchten beispielsweise den virtuellen Sicherungscomputer für Azure Site Recovery in einer separaten Ressourcengruppe und an einem separaten Standort bereitstellen. Resource Manager ermöglicht die Verwendung geschachtelter Vorlagen, um nicht das Abonnement und die Ressourcengruppe, die für die übergeordnete Vorlage verwendet werden, sondern andere Abonnements und Ressourcengruppen als Ziel festzulegen.

> [!NOTE]
> Sie können nur fünf Ressourcengruppen in einer Bereitstellung bereitstellen. Normalerweise bedeutet diese Einschränkung, dass Sie die Bereitstellung für eine Ressourcengruppe durchführen können, die für die übergeordnete Vorlage angegeben ist, sowie bis zu vier Ressourcengruppen in geschachtelten oder verknüpften Bereitstellungen. Wenn Ihre übergeordnete Vorlage aber nur geschachtelte oder verknüpfte Vorlagen enthält und selbst keine Ressourcen bereitstellt, können Sie bis zu fünf Ressourcengruppen in geschachtelte oder verknüpfte Bereitstellungen einbinden.

## <a name="specify-a-subscription-and-resource-group"></a>Angeben eines Abonnements und einer Ressourcengruppe

Um eine andere Ressource als Ziel festzulegen, verwenden Sie eine geschachtelte oder verknüpfte Vorlage. Der Ressourcentyp `Microsoft.Resources/deployments` stellt Parameter für `subscriptionId` und `resourceGroup` bereit. Diese Eigenschaften ermöglichen Ihnen die Angabe eines anderen Abonnements und einer anderen Ressourcengruppe für die geschachtelte Bereitstellung. Alle Ressourcengruppen müssen vorhanden sein, bevor Sie die Bereitstellung ausführen. Ohne Angabe von Abonnement-ID oder Ressourcengruppe werden das Abonnement und die Ressourcengruppe aus der übergeordneten Vorlage verwendet.

Das zum Bereitstellen der Vorlage verwendete Konto muss über Berechtigungen für die Bereitstellung unter der angegebenen Abonnement-ID verfügen. Wenn das angegebene Abonnement in einem anderen Azure Active Directory-Mandanten vorhanden ist, müssen Sie [Gastbenutzer aus einem anderen Verzeichnis hinzufügen](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).

Gehen Sie zum Angeben einer anderen Ressourcengruppe und eines Abonnements wie folgt vor:

```json
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "resourceGroup": "[parameters('secondResourceGroup')]",
        "subscriptionId": "[parameters('secondSubscriptionID')]",
        ...
    }
]
```

Wenn sich Ihre Ressourcengruppen in demselben Abonnement befinden, können Sie den Wert **subscriptionId** entfernen.

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

Verwenden Sie zum Bereitstellen der Beispielvorlage die Azure PowerShell-Version 4.0.0 oder höher bzw. die Azure CLI-Version 2.0.0 oder höher.

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

## <a name="example-templates"></a>Beispielvorlagen

Die folgenden Vorlagen veranschaulichen die Bereitstellungen von mehreren Ressourcengruppen. Skripts zum Bereitstellen der Vorlagen werden nach der Tabelle angezeigt.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[Abonnementübergreifende Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) |Stellt ein Speicherkonto für eine Ressourcengruppe und ein weiteres Speicherkonto für eine zweite Ressourcengruppe bereit. Schließen Sie einen Wert für die Abonnement-ID ein, wenn sich die zweite Ressourcengruppe in einem anderen Abonnement befindet. |
|[Übergreifende Vorlage für Ressourcengruppeneigenschaften](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) |Veranschaulicht, wie die `resourceGroup()`-Funktion aufgelöst wird. Sie stellt keine Ressourcen bereit. |

### <a name="powershell"></a>PowerShell

PowerShell: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Ressourcengruppen im **gleichen Abonnement** Folgendes:

```azurepowershell-interactive
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

PowerShell: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in **zwei Abonnements** Folgendes:

```azurepowershell-interactive
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

PowerShell: Verwenden Sie zum Testen, wie das **Ressourcengruppenobjekt** für die übergeordnete Vorlage, die Inlinevorlage und die verknüpfte Vorlage aufgelöst wird, Folgendes:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Im vorherigen Beispiel werden sowohl **parentRG** als auch **inlineRG** in **parentGroup** aufgelöst. **linkedRG** wird in **linkedGroup** aufgelöst. Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

```powershell
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 inlineRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 linkedRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
                                               "name": "linkedGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Azure CLI: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Ressourcengruppen im **gleichen Abonnement** Folgendes:

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

Azure CLI: Verwenden Sie zum Bereitstellen von zwei Speicherkonten in **zwei Abonnements** Folgendes:

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

Azure CLI: Verwenden Sie zum Testen, wie das **Ressourcengruppenobjekt** für die übergeordnete Vorlage, die Inlinevorlage und die verknüpfte Vorlage aufgelöst wird, Folgendes:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

Im vorherigen Beispiel werden sowohl **parentRG** als auch **inlineRG** in **parentGroup** aufgelöst. **linkedRG** wird in **linkedGroup** aufgelöst. Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

```azurecli
...
"outputs": {
  "inlineRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "linkedRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
      "location": "southcentralus",
      "name": "linkedGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "parentRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  }
},
...
```

## <a name="next-steps"></a>Nächste Schritte

* Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md).
