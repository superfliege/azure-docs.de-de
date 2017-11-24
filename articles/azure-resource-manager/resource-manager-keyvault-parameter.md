---
title: Key Vault-Geheimnis mit Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: "Informationen zum Übergeben eines geheimen Schlüssels aus einem Schlüsseltresor als Parameter während der Bereitstellung."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2017
ms.author: tomfitz
ms.openlocfilehash: e789a234979be877d990665902fd6219ae7ec40b
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung

Wenn Sie einen zu schützenden Wert (z.B. ein Kennwort) während der Bereitstellung als Parameter übergeben müssen, können Sie den Wert aus einer Instanz von [Azure Key Vault](../key-vault/key-vault-whatis.md) abrufen. Sie rufen den Wert ab, indem Sie den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei angeben. Der Wert wird nie offengelegt, da Sie nur auf die Schlüsseltresor-ID verweisen. Sie müssen den Wert für das Geheimnis nicht jedes Mal manuell eingeben, wenn Sie die Ressourcen bereitstellen. Der Schlüsseltresor kann unter einem anderen Abonnement als die Ressourcengruppe für die Bereitstellung vorhanden sein. Beim Verweisen auf den Schlüsseltresor geben Sie die Abonnement-ID an.

Legen Sie beim Erstellen des Schlüsseltresors die Eigenschaft *enabledForTemplateDeployment* auf *true* fest. Durch Festlegen des Werts auf „true“ ermöglichen Sie den Zugriff über Resource Manager-Vorlagen während der Bereitstellung.

## <a name="deploy-a-key-vault-and-secret"></a>Bereitstellen eines Schlüsseltresors und eines geheimen Schlüssels

Verwenden Sie zum Erstellen eines Schlüsseltresors und eines Geheimnisses die Azure CLI oder PowerShell. Beachten Sie, dass der Schlüsseltresor für die Vorlagenbereitstellung aktiviert ist. 

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create \
  --name $vaultname \
  --resource-group examplegroup \
  --location 'South Central US' \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Verwenden Sie für PowerShell Folgendes:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault `
  -VaultName $vaultname `
  -ResourceGroupName examplegroup `
  -Location "South Central US" `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Aktivieren des Zugriffs auf das Geheimnis

Stellen Sie unabhängig davon, ob Sie einen neuen oder einen bereits vorhandenen Schlüsseltresor verwenden, Folgendes sicher: Der Benutzer, der die Vorlage bereitstellt, muss auf das Geheimnis zugreifen können. Der Benutzer, von dem eine Vorlage bereitgestellt wird, in der auf ein Geheimnis verwiesen wird, muss über die Berechtigung `Microsoft.KeyVault/vaults/deploy/action` für den Schlüsseltresor verfügen. Die Rollen [Besitzer](../active-directory/role-based-access-built-in-roles.md#owner) und [Mitwirkender](../active-directory/role-based-access-built-in-roles.md#contributor) gewähren diesen Zugriff.

## <a name="reference-a-secret-with-static-id"></a>Verweisen auf ein Geheimnis mit einer statischen ID

Die Vorlage, die ein Geheimnis für einen Schlüsseltresor empfängt, unterscheidet sich nicht von anderen Vorlagen. Der Grund hierfür ist, dass **Sie auf den Schlüsseltresor in der Parameterdatei verweisen, nicht in der Vorlage.** Die folgende Vorlage stellt z.B. eine SQL-Datenbank bereit, die ein Administratorkennwort enthält. Der Kennwortparameter ist auf eine sichere Zeichenfolge festgelegt. Aber die Vorlage gibt nicht den Ursprung dieses Werts an.

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

Erstellen Sie jetzt eine Parameterdatei für der vorherige Vorlage. Geben Sie in der Parameterdatei einen Parameter an, der dem Namen des Parameters in der Vorlage entspricht. Verweisen Sie für den Parameterwert auf das Geheimnis aus dem Schlüsseltresor. Sie verweisen auf den geheimen Schlüssel, indem Sie den Ressourcenbezeichner des Schlüsseltresors und den Namen des geheimen Schlüssels übergeben. Im folgenden Beispiel muss das Geheimnis für den Schlüsseltresor bereits vorhanden sein, und Sie geben einen statischer Wert für dessen Ressourcen-ID ein.

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

Stellen Sie jetzt die Vorlage bereit, und übergeben Sie sie in der Parameterdatei. Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az group create --name datagroup --location "Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-file sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Verwenden Sie für PowerShell Folgendes:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateFile sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Verweisen auf ein Geheimnis mit dynamischer ID

Im vorherigen Abschnitt wurde für das Geheimnis des Schlüsseltresors eine statische Ressourcen-ID übergeben. Manchmal muss jedoch auf einen geheimen Schlüsseltresorschlüssel verwiesen werden, der je nach aktueller Bereitstellung variiert. In einem solchen Fall kann die Ressourcen-ID nicht in der Parameterdatei hartcodiert werden. Da in der Parameterdatei keine Vorlagenausdrücke zulässig sind, kann die Ressourcen-ID leider nicht dynamisch in der Parameterdatei generiert werden.

Wenn die Ressourcen-ID für das Geheimnis eines Schlüsseltresors dynamisch generiert werden muss, müssen Sie die Ressource, die das Geheimnis benötigt, in eine geschachtelte Vorlage verschieben. Die geschachtelte Vorlage wird der Mastervorlage hinzugefügt, und es wird ein Parameter mit der dynamisch generierten Ressourcen-ID übergeben. Ihre geschachtelte Vorlage muss über einen externen URI verfügbar sein. Im weiteren Verlauf dieses Artikels wird davon ausgegangen, dass Sie die vorherige Vorlage zu einem Speicherkonto hinzugefügt haben und auf sie über den `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json` zugegriffen werden kann.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "vaultResourceGroup": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      },
      "adminLogin": {
        "type": "string"
      },
      "sqlServerName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(subscription().subscriptionId,  parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          },
          "adminLogin": { "value": "[parameters('adminLogin')]" },
          "sqlServerName": {"value": "[parameters('sqlServerName')]"}
        }
      }
    }],
    "outputs": {}
}
```

Stellen Sie die vorherige Vorlage bereit, und geben Sie Werte für die Parameter an.

## <a name="next-steps"></a>Nächste Schritte
* Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md).
* Vollständige Beispiele für Verweise auf geheime Schlüssel finden Sie unter [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)(in englischer Sprache).
