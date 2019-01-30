---
title: Key Vault-Geheimnis mit Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: Informationen zum Übergeben eines geheimen Schlüssels aus einem Schlüsseltresor als Parameter während der Bereitstellung.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: e04ee6b74be0bb7ec81166f43295a8a9377f8cf8
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54381520"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung

Wenn Sie einen zu schützenden Wert (z.B. ein Kennwort) während der Bereitstellung als Parameter übergeben müssen, können Sie den Wert aus einer Instanz von [Azure Key Vault](../key-vault/key-vault-whatis.md) abrufen. Sie rufen den Wert ab, indem Sie den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei angeben. Der Wert wird nie offengelegt, da Sie nur auf die Schlüsseltresor-ID verweisen. Der Schlüsseltresor kann unter einem anderen Abonnement als die Ressourcengruppe für die Bereitstellung vorhanden sein.

## <a name="deploy-a-key-vault-and-secret"></a>Bereitstellen eines Schlüsseltresors und eines geheimen Schlüssels

Verwenden Sie zum Erstellen eines Schlüsseltresors und eines Geheimnisses die Azure CLI oder PowerShell. `enabledForTemplateDeployment` ist eine Schlüsseltresoreigenschaft. Um aus der Resource Manager-Bereitstellung auf die Geheimnisse innerhalb dieses Schlüsseltresors zuzugreifen, muss `enabledForTemplateDeployment` `true` sein. 

Das folgende Beispielskript für Azure PowerShell und Azure CLI veranschaulicht die Erstellung eines Schlüsseltresors und eines Geheimnisses.

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
keyVaultName='{your-unique-vault-name}'
resourceGroupName='{your-resource-group-name}'
location='centralus'
userPrincipalName='{your-email-address-associated-with-your-subscription}'

# Create a resource group
az group create --name $resourceGroupName --location $location

# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

# Create a secret with the name, vmAdminPassword
password=$(openssl rand -base64 32)
echo $password
az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
```

Verwenden Sie für PowerShell Folgendes:

```azurepowershell-interactive
$keyVaultName = "{your-unique-vault-name}"
$resourceGroupName="{your-resource-group-name}"
$location='Central US'
$userPrincipalName='{your-email-address-associated-with-your-subscription}'

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

New-AzureRmKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list

$password = openssl rand -base64 32
echo $password
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretvalue
```

Verwenden Sie beim Ausführen des PowerShell-Skripts außerhalb der Cloud Shell stattdessen den folgenden Befehl, um das Kennwort zu generieren:

```powershell
Add-Type -AssemblyName System.Web
[System.Web.Security.Membership]::GeneratePassword(16,3)
```

Informationen zur Verwendung der Resource Manager-Vorlage Eine Schritt-für-Schritt-Anleitung finden Sie im [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md#prepare-the-key-vault).

> [!NOTE]
> Jeder Azure-Dienst hat bestimmte Kennwortanforderungen. Die Anforderungen des virtuellen Azure-Computers finden Sie beispielsweise unter [Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="enable-access-to-the-secret"></a>Aktivieren des Zugriffs auf das Geheimnis

Über das Festlegen von `enabledForTemplateDeployment` auf `true` hinaus muss der Benutzer, der die Vorlage bereitstellt, über die Berechtigung `Microsoft.KeyVault/vaults/deploy/action` für den Bereich verfügen, die den Schlüsseltresor einschließlich Ressourcengruppe und Schlüsseltresor enthält. Die Rollen [Besitzer](../role-based-access-control/built-in-roles.md#owner) und [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) gewähren diesen Zugriff. Wenn Sie den Schlüsseltresor erstellen, sind Sie der Besitzer, also verfügen Sie über die Berechtigung. Wenn der Schlüsseltresor unter einem anderen Abonnement ausgeführt wird, muss der Besitzer des Schlüsseltresors den Zugriff erteilen.

Die folgende Prozedur zeigt das Erstellen einer Rolle mit der Mindestberechtigung und das Zuweisen des Benutzers
1. Erstellen einer benutzerdefinierten Rollendefinition (JSON-Datei):

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    Ersetzen Sie „00000000-0000-0000-0000-000000000000“ durch die Abonnement-ID des Benutzers, der die Vorlagen bereitstellen muss.

2. Erstellen Sie die neue Rolle mithilfe der JSON-Datei:

    ```azurepowershell
    $resourceGroupName= "<Resource Group Name>" # the resource group which contains the Key Vault
    $userPrincipalName = "<Email Address of the deployment operator>"
    New-AzureRmRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzureRmRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    Im `New-AzureRmRoleAssignment`-Beispiel wird die benutzerdefinierte Rolle dem Benutzer auf der Ressourcengruppenebene zugewiesen.  

Wenn ein Schlüsseltresor zusammen mit der Vorlage für eine [verwaltete Anwendung](../managed-applications/overview.md) verwendet wird, müssen Sie Zugriff auf den Dienstprinzipal des **Ressourcenanbieters der Appliance** erteilen. Weitere Informationen finden Sie unter [Zugreifen auf das Geheimnis im Schlüsseltresor bei der Bereitstellung von Azure Managed Applications](../managed-applications/key-vault-access.md).

## <a name="reference-a-secret-with-static-id"></a>Verweisen auf ein Geheimnis mit einer statischen ID

Die Vorlage, die ein Geheimnis für einen Schlüsseltresor empfängt, unterscheidet sich nicht von anderen Vorlagen. Der Grund hierfür ist, dass **Sie auf den Schlüsseltresor in der Parameterdatei verweisen, nicht in der Vorlage.** Die folgende Abbildung zeigt, wie die Parameterdatei auf das Geheimnis verweist und diesen Wert an die Vorlage übergibt.

![Statische ID](./media/resource-manager-keyvault-parameter/statickeyvault.png)

Die [folgende Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) stellt z.B. eine SQL-Datenbank bereit, die ein Administratorkennwort enthält. Der Kennwortparameter ist auf eine sichere Zeichenfolge festgelegt. Aber die Vorlage gibt nicht den Ursprung dieses Werts an.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Erstellen Sie jetzt eine Parameterdatei für der vorherige Vorlage. Geben Sie in der Parameterdatei einen Parameter an, der dem Namen des Parameters in der Vorlage entspricht. Verweisen Sie für den Parameterwert auf das Geheimnis aus dem Schlüsseltresor. Sie verweisen auf den geheimen Schlüssel, indem Sie den Ressourcenbezeichner des Schlüsseltresors und den Namen des geheimen Schlüssels übergeben. In der [folgenden Parameterdatei](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.parameters.json) muss das Geheimnis für den Schlüsseltresor bereits vorhanden sein, und Sie geben einen statischer Wert für dessen Ressourcen-ID ein. Kopieren Sie diese Datei lokal, und legen Sie die Abonnement-ID, den Tresornamen und den SQL Server-Namen fest.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "examplesecret"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Wenn Sie den geheimen Schlüssel in einer Version verwenden müssen, die nicht die aktuelle Version ist, verwenden Sie die `secretVersion`-Eigenschaft.

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Stellen Sie jetzt die Vorlage bereit, und übergeben Sie sie in der Parameterdatei. Sie können die Beispielvorlage von GitHub verwenden, müssen jedoch eine lokale Parameterdatei mit Werten nutzen, die auf Ihre Umgebung festgelegt sind.

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Verwenden Sie für PowerShell Folgendes:

```powershell-interactive
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Verweisen auf ein Geheimnis mit dynamischer ID

Im vorherigen Abschnitt wurde für das Geheimnis des Schlüsseltresors eine statische Ressourcen-ID aus dem Parameter übergeben. Manchmal muss jedoch auf einen geheimen Schlüsseltresorschlüssel verwiesen werden, der je nach aktueller Bereitstellung variiert. Oder Sie möchten einfach Parameterwerte an die Vorlage übergeben, statt einen Referenzparameter in der Parameterdatei zu erstellen. In beiden Fällen können Sie die Ressourcen-ID für ein Schlüsseltresorgeheimnis mithilfe einer verknüpften Vorlage dynamisch erstellen.

Da in der Parameterdatei keine Vorlagenausdrücke zulässig sind, kann die Ressourcen-ID nicht dynamisch in der Parameterdatei generiert werden. 

Die verknüpfte Vorlage wird der übergeordneten Vorlage hinzugefügt, und es wird ein Parameter mit der dynamisch generierten Ressourcen-ID übergeben. Die folgende Abbildung zeigt, wie ein Parameter in der verknüpften Vorlage auf das Geheimnis verweist.

![Dynamische ID](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

Die [folgende Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-key-vault-use-dynamic-id) erstellt dynamisch die Schlüsseltresor-ID und übergibt sie als Parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location where the resources will be deployed."
            }
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the keyvault that contains the secret."
            }
        },
        "secretName": {
            "type": "string",
            "metadata": {
                "description": "The name of the secret."
            }
        },
        "vaultResourceGroupName": {
            "type": "string",
            "metadata": {
                "description": "The name of the resource group that contains the keyvault."
            }
        },
        "vaultSubscription": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "The name of the subscription that contains the keyvault."
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located. When the template is deployed using the accompanying scripts, a private location in the subscription will be used and this value will be automatically generated."
            },
            "defaultValue": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/"
        },
        "_artifactsLocationSasToken": {
            "type": "securestring",
            "metadata": {
                "description": "The sasToken required to access _artifactsLocation.  When the template is deployed using the accompanying scripts, a sasToken will be automatically generated."
            },
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "dynamicSecret",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "contentVersion": "1.0.0.0",
                    "uri": "[uri(parameters('_artifactsLocation'), concat('./nested/sqlserver.json', parameters('_artifactsLocationSasToken')))]"
                },
                "parameters": {
                    "location": {
                        "value": "[parameters('location')]"
                    },
                    "adminLogin": {
                        "value": "ghuser"
                    },
                    "adminPassword": {
                        "reference": {
                            "keyVault": {
                                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                            },
                            "secretName": "[parameters('secretName')]"
                        }
                    }
                }
            }
        }
    ],
    "outputs": {
        "sqlFQDN": {
            "type": "string",
            "value": "[reference('dynamicSecret').outputs.sqlFQDN.value]"
        }
    }
}
```

Stellen Sie die vorherige Vorlage bereit, und geben Sie Werte für die Parameter an. Sie können die Beispielvorlage von GitHub verwenden, müssen jedoch Parameterwerte für Ihre Umgebung angeben.

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=<your-vault> vaultResourceGroupName=examplegroup secretName=examplesecret
```

Verwenden Sie für PowerShell Folgendes:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName <your-vault> -vaultResourceGroupName examplegroup -secretName examplesecret
```

## <a name="next-steps"></a>Nächste Schritte
* Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md).
* Vollständige Beispiele für Verweise auf geheime Schlüssel finden Sie unter [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)(in englischer Sprache).