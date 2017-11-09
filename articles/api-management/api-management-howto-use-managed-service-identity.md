---
title: "Verwenden der verwalteten Azure-Dienstidentität in Azure API Management | Microsoft-Dokumentation"
description: "Informationen zur Verwendung der verwalteten Azure-Dienstidentität in API Management"
services: api-management
documentationcenter: 
author: miaojiang
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: 70bf207cc173caf7d8cae3c4c9111ee2f427405b
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="how-to-use-azure-managed-service-identity-in-azure-api-management"></a>Verwenden der verwalteten Azure-Dienstidentität in Azure API Management

> [!Note]
> Die verwaltete Dienstidentität für Azure API Management befindet sich derzeit in der Vorschauversion.

In diesem Thema erfahren Sie, wie Sie eine verwaltete Dienstidentität für eine API Management-Dienstinstanz erstellen und auf andere Ressourcen zugreifen. Mithilfe einer von Azure Active Directory (AAD) generierten verwalteten Dienstidentität kann Ihre API Management-Instanz einfach und sicher auf andere mit AAD geschützte Ressourcen wie Azure Key Vault zugreifen. Da die verwaltete Dienstidentität von Azure verwaltet wird, müssen Sie keine Geheimnisse bereitstellen oder rotieren. Weitere Informationen zur verwalteten Azure-Dienstidentität finden Sie in der [Übersicht über die verwaltete Dienstidentität](../active-directory/msi-overview.md).

## <a name="creating-an-api-management-instance-with-an-identity-using-an-azure-resource-manager-template"></a>Erstellen einer API Management-Instanz mit einer Identität anhand einer Azure Resource Manager-Vorlage

Eine API Management-Instanz kann mit einer Identität erstellt werden, indem die folgende Eigenschaft in die Ressourcendefinition aufgenommen wird. 

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Hierdurch erhält Azure die Anweisung, die Identität für Ihre API Management-Instanz zu erstellen und zu verwalten. 

Eine vollständige Vorlage kann beispielsweise wie folgt aussehen:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "serviceName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The name of the api management service"
            }
        },
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "defaultValue": "admin@contoso.com",
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "minLength": 1,
            "defaultValue": "Contoso",
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": [
                "Developer",
                "Standard",
                "Premium"
            ],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-01",
            "name": "[parameters('serviceName')]",
            "type": "Microsoft.ApiManagement/service",
            "location": "[resourceGroup().location]",
            "tags": {},
            "sku": {
                "name": "[parameters('sku')]",
                "capacity": "[parameters('skuCount')]"
            },
            "properties": {
                "publisherEmail": "[parameters('publisherEmail')]",
                "publisherName": "[parameters('publisherName')]"
            },
            "identity": { 
                "type": "systemAssigned" 
            }
        }
    ]
}
```

## <a name="obtaining-a-certificate-from-azure-key-vault"></a>Anfordern eines Zertifikats von Azure Key Vault

Das folgenden Beispiel zeigt, wie Sie ein Zertifikat von Azure Key Vault erhalten. Es umfasst die folgenden Schritte:

* Erstellen einer API Management-Instanz mit einer Identität
* Aktualisieren der Zugriffsrichtlinien einer Azure Key Vault-Instanz und Gestatten des Abrufs von Geheimnissen aus diesem Key Vault durch die API Management-Instanz
* Aktualisieren der API Management-Instanz durch Festlegen eines benutzerdefinierten Domänennamens mithilfe eines Zertifikats aus der Key Vault-Instanz

> [!Important]
> Wenn die Objektversion des Zertifikats nicht angegeben wird, erhält API Management automatisch die neuere Version des Zertifikats, nachdem sie in Key Vault hochgeladen wurde. 

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
            
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",        
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    { 
      "apiVersion": "2017-05-10", 
      "name": "apimWithKeyVault", 
      "type": "Microsoft.Resources/deployments",
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": {
            "publisherEmail": { "value": "[parameters('publisherEmail')]"},
            "publisherName": { "value": "[parameters('publisherName')]"},
            "sku": { "value": "[parameters('sku')]"},
            "skuCount": { "value": "[parameters('skuCount')]"},
            "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
            "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
        }
      } 
    }]
}
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur verwalteten Azure-Dienstidentität
  * [Übersicht über verwaltete Dienstidentitäten](../active-directory/msi-overview.md)
  * [Weitere Azure Resource Manager-Vorlagen](https://github.com/Azure/azure-quickstart-templates)

