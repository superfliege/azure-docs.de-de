---
title: Verwalten des Zugriffs mit der RBAC und Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff für Benutzer, Gruppen und Anwendungen mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und Azure Resource Manager-Vorlagen verwalten.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 5e080614d4f0001a0bf1b44dd402f37db2463e03
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206614"
---
# <a name="manage-access-using-rbac-and-azure-resource-manager-templates"></a>Verwalten des Zugriffs mit der RBAC und Azure Resource Manager-Vorlagen

Der Zugriff auf Ressourcen in Azure wird mithilfe der [rollenbasierten Zugriffssteuerung](overview.md) (Role-Based Access Control, RBAC) verwaltet. Neben der Verwendung von Azure PowerShell oder der Azure CLI können Sie den Zugriff auf Azure-Ressourcen mit der RBAC und [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) verwalten. Vorlagen können hilfreich sein, wenn Sie Ressourcen konsistent und wiederholt bereitstellen müssen. In diesem Artikel wird beschrieben, wie Sie den Zugriff mit der RBAC und Vorlagen verwalten können.

## <a name="example-template-to-create-a-role-assignment"></a>Beispielvorlage zum Erstellen einer Rollenzuweisung

In RBAC erstellen Sie zum Gewähren des Zugriffs eine Rollenzuweisung. Die folgende Vorlage veranschaulicht Folgendes:
- Zuweisen einer Rolle zu einem Benutzer, einer Gruppe oder einer Anwendung im Ressourcengruppenkontext
- Angeben der Rollen „Besitzer“, „Mitwirkender“ und „Leser“ als Parameter

Um die Vorlage zu verwenden, müssen Sie Folgendes eingeben:
- Den Namen einer Ressourcengruppe
- Den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung, dem bzw. der die Rolle zugewiesen wird
- Die zuzuweisende Rolle
- Einen eindeutigen Bezeichner, der für die Rollenzuweisung verwendet wird

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "scope": "[resourceGroup().id]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2017-05-01",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('scope')]"
      }
    }
  ]
}
```

Das folgende Beispiel veranschaulicht die Zuweisung der Rolle „Leser“ zu einem Benutzer nach der Bereitstellung der Vorlage.

![Rollenzuweisung mithilfe einer Vorlage](./media/role-assignments-template/role-assignment-template.png)

## <a name="deploy-template-using-azure-powershell"></a>Bereitstellen einer Vorlage mit Azure PowerShell

Um die vorherige Vorlage mithilfe von Azure PowerShell bereitzustellen, führen Sie die folgenden Schritte durch:

1. Erstellen Sie eine neue Datei namens „rbac-rg.json“, und kopieren Sie die obige Vorlage.

1. Melden Sie sich bei [Azure PowerShell](/powershell/azure/authenticate-azureps) an.

1. Rufen Sie den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung ab. Beispielsweise können Sie mit dem Befehl [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) Azure AD-Benutzer auflisten.

    ```azurepowershell
    Get-AzureRmADUser
    ```

1. Verwenden Sie ein GUID-Tool, um einen eindeutigen Bezeichner zu generieren, der für die Rollenzuweisung verwendet wird. Das Format des Bezeichners ist: `11111111-1111-1111-1111-111111111111`

1. Erstellen Sie eine Beispielressourcengruppe.

    ```azurepowershell
    New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Verwenden Sie den Befehl [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment), um die Bereitstellung zu starten.

    ```azurepowershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    ```

    Sie werden aufgefordert, die erforderlichen Parameter anzugeben. Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

    ```Output
    PS /home/user> New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    
    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    principalId: 22222222-2222-2222-2222-222222222222
    builtInRoleType: Reader
    roleNameGuid: 11111111-1111-1111-1111-111111111111
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="deploy-template-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure CLI

Um die vorherige Vorlage mithilfe der Azure CLI bereitzustellen, führen die folgenden Schritte durch:

1. Erstellen Sie eine neue Datei namens „rbac-rg.json“, und kopieren Sie die obige Vorlage.

1. Melden Sie sich bei der [Azure CLI](/cli/azure/authenticate-azure-cli) an.

1. Rufen Sie den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung ab. Beispielsweise können Sie mit dem Befehl [az ad user list](/cli/azure/ad/user#az-ad-user-list) Azure AD-Benutzer auflisten.

    ```azurecli
    az ad user list
    ```

1. Verwenden Sie ein GUID-Tool, um einen eindeutigen Bezeichner zu generieren, der für die Rollenzuweisung verwendet wird. Das Format des Bezeichners ist: `11111111-1111-1111-1111-111111111111`

1. Erstellen Sie eine Beispielressourcengruppe.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Starten Sie mit dem Befehl [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) die Bereitstellung.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    ```

    Sie werden aufgefordert, die erforderlichen Parameter anzugeben. Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    Please provide string value for 'principalId' (? for help): 22222222-2222-2222-2222-222222222222
    Please provide string value for 'builtInRoleType' (? for help):
     [1] Owner
     [2] Contributor
     [3] Reader
    Please enter a choice [1]: 3
    Please provide string value for 'roleNameGuid' (? for help): 11111111-1111-1111-1111-111111111111
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```
    
## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-create-first-template.md)
- [Grundlegendes zur Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md)
- [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?term=rbac)
