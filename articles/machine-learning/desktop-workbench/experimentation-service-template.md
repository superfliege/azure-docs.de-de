---
title: Erstellen eines Azure Machine Learning-Experimentieren-Kontos mit einer Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: Dieser Artikel enthält ein Beispiel zum Erstellen eines Azure Machine Learning-Experimentieren-Kontos mithilfe einer Azure Resource Manager-Vorlage.
services: machine-learning
author: hning86
ms.author: haining
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 24ed164f4a1dfdb9a3913efa78fe58fab2b53696
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991962"
---
# <a name="configure-the-azure-machine-learning-experimentation-service"></a>Konfigurieren des Azure Machine Learning-Experimentieren-Diensts

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

## <a name="overview"></a>Übersicht
Konten für Azure Machine Learning-Experimentieren, Arbeitsbereiche und Projekte sind Azure-Ressourcen. Daher können sie mithilfe von Resource Manager-Vorlagen bereitgestellt werden. Resource Manager-Vorlagen sind JSON-Dateien, mit denen die Ressourcen definiert werden, die Sie für Ihre Lösung bereitstellen müssen. Weitere Informationen zu den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen finden Sie unter [Übersicht über Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="deploy-a-template"></a>Bereitstellen einer Vorlage
Zum Bereitstellen einer Vorlage sind nur wenige Schritte in der Azure-Befehlszeilenschnittstelle oder im Azure-Portal erforderlich.

### <a name="deploy-a-template-from-the-command-line"></a>Bereitstellen einer Vorlage über die Befehlszeile
Bei Verwendung der Befehlszeilenschnittstelle kann mit einem Befehl eine Vorlage in einer vorhandenen Ressourcengruppe bereitgestellt werden.
Nachfolgend finden Sie Informationen zum Erstellen einer Vorlage:

```sh
# Login and validate your are in the right subscription context
az login

# Create a new resource group (you can use an existing one)
az group create --name <resource group name> --location <supported Azure region>
az group deployment create -n testdeploy -g <resource group name> --template-file <template-file.json> --parameters <parameters.json>
```

### <a name="deploy-a-template-from-the-azure-portal"></a>Bereitstellen einer Vorlage über das Azure-Portal
Sie können auch das Azure-Portal zum Erstellen und Bereitstellen einer Vorlage verwenden. Gehen Sie wie folgt vor:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **Alle Dienste**, und suchen Sie nach „Vorlagen“.
3. Klicken Sie auf **Vorlagen**.
4. Klicken Sie auf **+ Hinzufügen**, und kopieren Sie die Vorlageninformationen. 
5. Wählen Sie die in Schritt 4 erstellte Vorlage aus, und klicken Sie auf **Bereitstellen**.


## <a name="create-a-template-from-an-existing-azure-resource-in-the-azure-portal"></a>Erstellen einer Vorlage über eine vorhandene Azure-Ressource im Azure-Portal
Falls bereits ein Konto für Azure Machine-Experimentieren verfügbar ist, können Sie im [Azure-Portal](https://portal.azure.com) über diese Ressource eine Vorlage erstellen. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu einem Azure-Experimentieren-Konto.
2. Klicken Sie unter **Einstellungen** auf **Automatisierungsskript**.
3. Laden Sie die Vorlage herunter. 

Alternativ können Sie die Vorlagendateien manuell bearbeiten. Weiter unten sehen Sie ein Beispiel für eine Vorlage und Parameterdateien. 

### <a name="template-file-example"></a>Beispiel für eine Vorlagendatei
Erstellen Sie eine Datei namens „template-file.json“ mit folgendem Inhalt: 

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "Name of the machine learning experimentation team account"
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of the machine learning experimentation account and other dependent resources."
            }
        },
        "storageAccountSku": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "metadata": {
                "description": "Sku of the storage account"
            }
        }
    },
    "variables": {
        "mlexpVersion": "2017-05-01-preview",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
    },
    "resources": [
        {
            "name": "[parameters('accountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2016-12-01",
            "tags": {
                "mlteamAccount": "[parameters('accountName')]"
            },
            "sku": {
                "name": "[parameters('storageAccountSku')]"
            },
            "kind": "Storage",
            "properties": {
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            }
        },
        {
            "apiVersion": "[variables('mlexpVersion')]",
            "type": "Microsoft.MachineLearningExperimentation/accounts",
            "name": "[parameters('accountName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
            ],
            "properties": {
                "storageAccount": {
                    "storageAccountId": "[variables('stgResourceId')]",
                    "accessKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName')), '2016-12-01').keys[0].value]"
                }
            }
        }
    ]
}
```

### <a name="parameters"></a>Parameter 
Erstellen Sie eine Datei mit folgendem Inhalt, und speichern Sie sie als „<parameters.json>“. 

Es gibt drei Werte, die Sie ändern können. 
* AccountName: Der Name des Experimentieren-Kontos
* Location: Eine der unterstützten Azure-Regionen
* StorageAccountSku: Azure ML unterstützt nur Standard-Speicher, Storage Premium wird nicht unterstützt. Weitere Informationen zum Speicher finden Sie unter [Einführung in Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction). 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "accountName": {
         "value": "MyExperimentationAccount"
     },
     "location": {
         "value": "eastus2"
     },
     "storageAccountSku": {
         "value": "Standard_LRS"
     }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* [Create and Install Azure Machine Learning (Erstellen und Installieren von Azure Machine Learning)](quickstart-installation.md)
