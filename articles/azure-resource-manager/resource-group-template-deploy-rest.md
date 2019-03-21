---
title: Bereitstellen von Ressourcen mit REST-API und Vorlagen | Microsoft Docs
description: Verwenden Sie Azure Resource Manager und Resource Manager-REST-API, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: bd574eb2d3537d3e5c0774f57e37283817cc7879
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58112023"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API

In diesem Artikel wird erläutert, wie Ihre Ressourcen mithilfe der Resource Manager-REST-API und von Azure Resource Manager-Vorlagen in Azure bereitgestellt werden.  

> [!TIP]
> Hilfe zum Beheben eines Fehlers während der Bereitstellung finden Sie unter:
> 
> * [Anzeigen von Bereitstellungsvorgängen](resource-manager-deployment-operations.md). Erfahren Sie, wie Sie Informationen zum Beheben von Fehlern abrufen.
> * [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md). Hier erfahren Sie, wie Sie häufig auftretende Bereitstellungsfehler beheben.
> 
> 

Sie können entweder Ihre Vorlage in den Anforderungstext einschließen oder eine Verknüpfung zu einer Datei erstellen. Bei einer Datei kann es sich um eine lokale Datei oder eine externe Datei handeln, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriff auf die Vorlage beschränken und während der Bereitstellung ein SAS-Token (Shared Access Signature) angeben.

## <a name="deploy-with-the-rest-api"></a>Bereitstellen mit der REST-API
1. Legen Sie [allgemeine Parameter und Header](/rest/api/azure/) fest, einschließlich Authentifizierungstoken.

1. Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist. Geben Sie Ihre Abonnement-ID, den Namen der neuen Ressourcengruppe und den Speicherort für Ihre Lösung an. Weitere Informationen finden Sie unter [Erstellen einer Ressourcengruppe](/rest/api/resources/resourcegroups/createorupdate).

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2018-05-01
   ```

   Mit dem Anforderungstext ähnlich dem folgenden:
   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. Überprüfen Sie die Bereitstellung vor der Implementierung, indem Sie den Vorgang zum [Überprüfen einer Vorlagenbereitstellung](/rest/api/resources/deployments/validate) ausführen. Geben Sie die Parameter beim Testen der Bereitstellung genauso an wie beim Ausführen der Bereitstellung (wie im nächsten Schritt zu sehen).

1. Erstellen Sie eine Bereitstellung. Geben Sie Ihre Abonnement-ID, den Namen der Ressourcengruppe und der Bereitstellung sowie einen Link zu Ihrer Vorlage an. Informationen über die Vorlagendatei finden Sie unter [Parameterdatei](#parameter-file). Weitere Informationen über die REST-API zum Erstellen einer Ressourcengruppe finden Sie unter [Erstellen einer Vorlagenbereitstellung](/rest/api/resources/deployments/createorupdate). Beachten Sie, dass **mode** auf **Incremental** festgelegt ist. Legen Sie zum Ausführen einer vollständigen Bereitstellung **mode** auf **Complete** fest. Gehen Sie bei Verwendung des Modus „Complete“ sehr umsichtig vor, da Sie versehentlich Ressourcen löschen können, die nicht in Ihrer Vorlage enthalten sind.

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

   Mit dem Anforderungstext ähnlich dem folgenden:

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      }
    }
   }
   ```

    Wenn Sie den Antwortinhalt und/oder den Anforderungsinhalt protokollieren möchten, fügen Sie **debugSetting** in die Anforderung ein.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    Sie können das Speicherkonto so einrichten, das ein SAS-Token (Shared Access Signature) verwendet wird. Weitere Informationen finden Sie unter [Delegieren des Zugriffs mit einer SAS (Shared Access Signature)](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

1. Anstatt für die Vorlage und die Parameter eine Verknüpfung mit Dateien zu erstellen, können Sie diese auch in den Anforderungstext einschließen.

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

5. Rufen Sie den Status der Vorlagenbereitstellung ab. Weitere Informationen finden Sie unter [Abrufen von Informationen zu einer Vorlagenbereitstellung](/rest/api/resources/deployments/get).

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="redeploy-when-deployment-fails"></a>Erneute Bereitstellung bei Bereitstellungsfehlern

Wenn eine Bereitstellung fehlschlägt, können Sie automatisch eine frühere, erfolgreiche Bereitstellung aus Ihrem Bereitstellungsverlauf bereitstellen. Geben Sie mit der Eigenschaft `onErrorDeployment` im Anforderungstext die erneute Bereitstellung an.

Zur Verwendung dieser Option müssen die Bereitstellungen eindeutige Namen aufweisen, damit sie im Verlauf identifiziert werden können. Wenn die Bereitstellungen keine eindeutigen Namen aufweisen, wird die vorherige erfolgreich ausgeführte Bereitstellung im Verlauf möglicherweise durch die aktuelle fehlerhafte Bereitstellung überschrieben. Diese Option kann nur für Bereitstellungen auf Stammebene verwendet werden. Bereitstellungen aus einer geschachtelten Vorlage können nicht erneut bereitgestellt werden.

Um die letzte erfolgreich ausgeführte Bereitstellung erneut bereitzustellen, wenn bei der aktuellen Bereitstellung Fehler auftreten, verwenden Sie folgenden Code:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

Um eine bestimmte Bereitstellung erneut bereitzustellen, wenn bei der aktuellen Bereitstellung Fehler auftreten, verwenden Sie folgenden Code:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

Die angegebene Bereitstellung muss erfolgreich ausgeführt worden sein.

## <a name="parameter-file"></a>Parameterdatei

Bei der Verwendung einer Parameterdatei zum Übergeben von Parameterwerten während der Bereitstellung müssen Sie eine JSON-Datei in einem Format wie im folgenden Beispiel erstellen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

Die Parameterdatei darf nicht größer als 64 KB sein.

Wenn Sie einen vertraulichen Wert für einen Parameter (z.B. ein Kennwort) angeben müssen, fügen Sie den Wert einem Schlüsseltresor hinzu. Rufen Sie den Schlüsseltresor während der Bereitstellung wie im vorherigen Beispiel gezeigt ab. Weitere Informationen finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Nächste Schritte
* Wenn Sie angeben möchten, wie Ressourcen behandelt werden sollen, die in der Ressourcengruppe enthalten sind, aber nicht in der Vorlage definiert wurden, lesen Sie die Informationen unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).
* Informationen zum Arbeiten mit asynchronen REST-Vorgängen finden Sie unter [Nachverfolgen asynchroner Vorgänge in Azure](resource-manager-async-operations.md).
* Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).

